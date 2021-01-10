---
layout: post
title: Building an Entity-Component-System Framework in Elixir
date: '2018-01-17 02:21:00'
author: Aldric "Trevoke" Giacomoni
---

### What’s an ECS?

[ECS](https://en.wikipedia.org/wiki/Entity%E2%80%93component%E2%80%93system) is a pattern that has value when there is a need to develop complex (possibly emergent) behavior based on simple properties. It is based upon the two basic ideas of:

- Separating logic and data
- Isolating data in meaningfully identifiable units

These ideas transform into the elements of an ECS as such:

- A `Component` is a bag of properties
- An `Entity` is a bag of Components
- A `System` modifies `Components`

An example might help. Please do keep in mind while reading the following example that there are no rules indicating what a good ECS organization is, so you might find my example great, or you might find it abominable, and both are perfectly valid viewpoints. Determining a good granularity for `Components` is an important decision that impacts the rest of the project. Similarly, determining how `Systems` work and how much responsibility they will have is crucial.

Let’s say we have an Entity. This Entity has a `SocialCharacteristics` Component which holds that its `name` is `Alice`. Alice also has a `Status`Component which indicates that her `health` is `3`. Alice also has a `Fighting`component which indicates that her `target` is `Bob` **—** Bob is a dragon with a health of 804 and he is also fighting Alice. We have a `FightingSystem` which runs every second and resolves the next round of fighting between entities that are fighting each other by finding all entities with the `Fighting` component and operating on them.

Our `FightingSystem` determines that Alice hits Bob for 0 points of health and Bob hits Alice for 34 points of health. The `FightingSystem` then realizes he needs to queue up the `DeathSystem` for Alice after it finishes, because Alice is now at `-31` health, and we have decided that **0** health is the boundary between life and death. The `DeathSystem` will perform several checks, including whether there is a life-saving spell or other such components acting on Alice which might change the outcome.

One of the things that we might not realize upon reading this is just how many design decisions were packed into this paragraph. Let’s go through a couple of the design decisions that were made during the creation of [Ecstatic’s](https://github.com/trevoke/ecstatic) 0.1 release and how they were further constrained by the implementation in Elixir.

### What’s Elixir? The TLDR

[Elixir](https://elixir-lang.org/) is language built on top of the [Erlang](http://erlang/) virtual machine. Erlang’s prime differentiator is that it was built to run distributed code. As such, it uses its own “processes”, which are significantly more lightweight than UN\*X processes. It also has immutable data, and uses what may be the purest form of message-passing that has yet been implemented (for inter-process communication). Creating an Erlang process can be thought of as saying “Hey, virtual machine, when you are allocating the resources for code execution, allocate some separate resources for this code”. When the code is done running, the process dies.

One of Elixir’s additional advantages is that it has real lisp-style [macros](http://en.wikipedia.org/wiki/Macro_%28computer_science%29#Syntactic_macros). This means it is possible to define code to generate additional code, and to create your own Domain-Specific Languages (DSL) with relative ease.

### The first design decisions

Since one of the key elements of Elixir code is code running in separate processes, I always knew this would be a major differentiator in the way I created the framework.

I also decided very early on that I would try and stick as closely as possible to the pure definition for each element of ECS that I described above.

And finally, since I had no idea what I was doing (something that probably hasn’t changed much), a lot of my inspiration for how systems would work came from reading the code for the [Artemis-ODB](https://github.com/junkdog/artemis-odb) project, an ECS in Java.

This left me with a number of unsolved questions:

- How do I keep track of changes made to components? Since there’s immutable data, if I don’t get back a new data structure that becomes the one I work with, and then use that new data structure to feed any subsequent processing, any changes made by systems would be lost
- What will be the interface for my systems? If they take in components, it’s hard to argue that the system can add or remove components from the entity, but if they take in entities, am I really making changes to components? Am I violating the Law of Demeter?
- For most effective usage of resources (efficient? eh… When it works, I’ll worry about that), I will try to use Erlang processes. What will the processes be? What will they not be?
- How do I do recurring work? (taking damage from poison, getting older, etc.)

### The building blocks

One thing I realized very quickly was that I didn’t even know what the API footprint of the framework would be. I knew I wanted to make the footprint as small as possible, because not only will boilerplate become an obstacle to adoption, but… I will be using macros! Any boilerplate, I should be able to write for the user.

So, here is what turned out to be the elements we would start with:

- `Component`, which is still a bag of properties. Each component will have a default value.
- `Entity`, which is still a bag of components. Each entity has a list of default components.
- `Aspect`, which is a way of recognizing an Entity as a particular combination of components (e.g. “with Bleeding, with Mortal, without Dead”).
- `Changes`, representing the changes to be made to an entity (components added, removed, updated).
- `System`, which will take in an `Entity` and return a set of `Changes` if that entity matches the `Aspect` associated with this `System`.
- `Watcher`, a way to encode “when a lifecycle event happens to a component, run this system”, where a lifecycle event is one of “added”, “removed”, or “updated and with a given predicate function returning true”.

The `Watchers` are something I was very proud of because they allowed the systems to be, as much as possible, black boxes. There was no “configuring” the system, there was simply “running” the system.

In such a way, I was building a very neat self-contained framework, forgetting about one crucial element: events triggered by actions taken outside the system. We’ll get to that.

### Connecting the pieces

Conversations in the MUD Coders Slack led me to deciding that I would try a unified log; I had already kind of decided that I wanted to return a set of changes from the systems, as this would allow me to trigger actions based on changes in value (say your health decreases by 2/3 of its max value in one change, you might want to start panicking).

This is a fairly significant turning point in the implementation, and I can’t even justify it by saying I weighed the implementation trade-offs; I just thought I would learn a lot more doing it this way than some other way, so that’s the way I went.

Now that I knew I had a unified log, I needed to both feed it and consume it. This is where [GenStage](https://github.com/elixir-lang/gen_stage) comes in. GenStage is an Elixir library that provides an abstration of a producer-consumer system with backpressure (GenStage: “Generic Stage”, for multi-stage processing), and one of the event dispatchers that comes built-in is a broadcast dispatcher: it will send each event to every consumer, and it will do so only when all consumers have requested an event (that is, when they are all ready to do work). This, along with setting each consumer to only ask for one event at a time, guarantees that the world will not fall out of sync: each event gets processed by the entire world before moving on to the next event.

It’s worth noting that I have done nothing that even remotely resembles performance testing or benchmarking here, so YMMV.

At this point, I knew that the systems needed to actually emit events, and therefore that the actual changes would happen somewhere else.

### Peppering in Erlang processes

It would likely be more standard to have each =System= be its own process (or be run in its own thread, etc.), and this would have the advantage of keeping the growth of Erlang processes at O(1), but this would then lead me down a different set of decisions: I would eventually (and maybe sooner rather than later) have to start to figure out how to split the workload, and maybe process subsets of entities in each systems. I am not yet interested in these decisions, and I would rather see how far I can push the decision to let the Erlang scheduler decides how to distribute the workload across available CPU cycles through liberal application of processes..

This all led me to an interesting choice: each entity in the ECS will have its own process, the only responsibility of which will be to take the incoming events that affect its matched entity and apply the given changes. A benefit of this, I think, is that I’m actually using a process to _do work_ instead of just _holding state_.

### Summarizing design choices

The main benefit of all these choices is that logic around entities \_actually\_ changing is completely encapsulated within a single function, and all paths that lead to that function are squeezed through the unified log.

At this point, what we have is the ability to trigger systems based on components being added, removed, or changed.

What we don’t have is the ability to trigger systems based on external actions… Including, say, the player wanting to move, or, even simpler, time ticks!

### My first hack, shame be unto me

A tick is just something that happens regularly. In most programming languages, the way to do this is something like “sleep for a while, send a message, sleep some more”, probably in a separate thread. Maybe even a completely different UNIX process (cronjobs, queues, etc). Within Erlang/Elixir, there are a few ways to do it, and the most canonical one is an Erlang stdlib package called `:timer` that has a function called `send_interval`. It sends a given message to a given process every X milliseconds.

This sounds perfect, right? There’s only one downside: every time you call this function, it creates a new process responsible for sending the given message. Thinking in the future, since any one entity may have multiple components, and any given component may trigger a tick, this might mean a O(mn) growth for the number of processes, where m is the number of components and n is the number of entities, instead of the current approach which is O(n). In a world where many entities exist, this might seriously limit how much the game can grow on a single machine… And I’m considering a world where the number of entities can grow dynamically (because of reproduction), so that’s a concern.

The good news is that there’s a function called `Process.send_after` which will ask the current process to send a message to some other process after some time interval. I can simply make sure that whatever message is received also calls `Process.send_after` again. It looks like this:Process.send\_after(destination, message, time\_until\_message\_is\_sent, options)

This gives me two relatively simple additional choices: I could have one “god tick process”, which is responsible for all ticks, though that choice might also offset some of the messages more than I want (if many messages should trigger around the same millisecond, will that turn out to be a problem?), or I could have the entity process manage its own ticks, which severely limits the number of simultaneous ticks that a given process has to handle. Either way, I’m still at O(n).

I went with the second choice. And I realized I had some additional difficulties: how would I get the initial message to start a tick to that process?

So, I cheated. The current implementation of `Watchers` takes a lambda as a predicate function, so for ticks, I decided that when a component gets attached, its predicate function would run the `Process.send_after` code, because the predicate function gets run by the entity’s event consumer.

The message I send has enough information to determine which system to run and how long the delay before the next message is.

When I detach a component, I also use the predicate function to send the consumer process a message to stop the given tick; when it receives the next tick message, it will know not to run the system and to not queue another message.

### Say it with code!

The following is code taken from my project, [Dwarlixir](https://github.com/Trevoke/dwarlixir), which was started under the laughably pretentious premise to be a mix between a MUD and Dwarf Fortress (it’s unclear how I thought this might be playable).

```elixir
defmodule Dwarlixir.Components.Age do
    use Ecstatic.Component
    @default_value %{age: 1, life_expectancy: 80}
end
defmodule Dwarlixir.Components.Mortal do
    use Ecstatic.Component
    @default_value %{mortal: true}
end
```

This is fairly simple: we’re just looking at two components as described above, with a default value. Since we are doing `use Ecstatic.Component` above, we can call `Dwarlixir.Components.Age.new` — the function is provided for us.

```elixir
defmodule Dwarlixir.Mobs.Dwarf do
    use Ecstatic.Entity
    alias Dwarlixir.Components, as: C
    @default_components [C.Age, C.Mortal]
end
```

And here we have a dwarf. When you initialize the dwarf, some components are marked as being set by default. This is almost “a factory”: it’s a convenience to create new entities quickly from given presets.

Now let’s connect the dots from the other side, starting with the watchers:

```elixir
defmodule Dwarlixir.Watchers do
    use Ecstatic.Watcher
    alias Dwarlixir.Components, as: C
    alias Dwarlixir.Systems, as: S
    watch_component C.Age, run: S.Aging, every: 6_000
    watch_component C.Age,
    run: S.DyingOfOldAge,
    when: fn(_e, c) -> c.state.age > c.state.life_expectancy end
end
```

This should be fairly readable by now. When the `Age` component is attached, run the `Aging` system every six seconds, and when the `Age` component has been updated and the age is greater than the life expectancy, trigger the `DyingOfOldAge` system.

```elixir
defmodule Dwarlixir.Systems.Aging do
    use Ecstatic.System
    alias Dwarlixir.Components, as: C
    def aspect, do: %Ecstatic.Aspect{with: [C.Age]}
    def dispatch(entity) do
        age = Entity.find_component(entity, C.Age)
        %Ecstatic.Changes{updated: [%{age | state: %{age.state | age: age.state.age + 1}}]}
    end
end
```

The `Aging` system will only run on entities that have the `Age` component, and on the last line you can see the one abstraction leak I currently still have: the actual values of the component are stored under a key called “state”, and the client code should have no knowledge of this, but right now does.

```elixir
defmodule Dwarlixir.Systems.DyingOfOldAge do
    use Ecstatic.System
    alias Dwarlixir.Components, as: C
    def aspect, do: %Ecstatic.Aspect{with: [C.Age, C.Mortal]}
    def dispatch(entity) do
        %Ecstatic.Changes{attached: [C.Dead], removed: [C.Age]}
    end
end
```

The `DyingOfOldAge` system will only run if the entity has both the `Age` and `Mortal` component (immortal entities certainly won’t die of old age)

And what happens when an entity “dies” (receives the `Dead` component)? Oh, I haven’t figured that out yet, so… There’s no code for that. Hopefully, though, you get a sense of how the ECS system creates disconnected pieces that combine to create a powerful effect.

### Conclusion

There’s a number of warts in this 0.1 version that I will work on and tweak; the API footprint will change and get much cleaner. Some logic is sort of duplicated, some logic feels bolted on.

Nonetheless, this \_works\_ and I am proud of this accomplishment. If you decide to try out [ecstatic](https://github.com/trevoke/ecstatic), I’d be delighted to hear about your experiences with it, as well as any and all feedback you have!

### Acknowledgements

Just about everyone in the MUD Coders’ Slack space provided inspiration, encouragement, guidance, advice, and support while I got started on this path. Without them, this post — as well as [ecstatic](https://github.com/trevoke/ecstatic) — would never have seen the light of day. The folks I know I need to thank, in no particular order, are:

- **Zachary Flower** for creating the MUD Coders’ Slack and therefore allowing this community to come to life
- **SwiftAusterity**, for showing me that I was placing artificial constraints on myself
- **bbuck**, for suggesting that maybe, just maybe, an entity and a component could simply be data structures
- **sazzer**, for providing calibrating advice on what an ECS should and should not be responsible for
- **wot**, for building an ECS _completely differently_ in common lisp, and allowing me to get a real-time difference in perspective when I was trying to define the reasoning that my ECS would use
- **christhekeele**, for teaching me a lot about how macros work in Elixir
- **vy**, for telling me about his idea to build a system that borrowed from React for resolving changes
- **craigp**, for telling me about his idea to use a unified log

Outside the MUD Coders, I need to thank:

- The folks who created and work on [Artemis-ODB](https://github.com/junkdog/artemis-odb), without which my path to understanding would have been significantly slowed down
- [**fishcakez**](https://github.com/fishcakez), a member of the Elixir core team and one of the minds behind [GenStage](https://github.com/elixir-lang/gen_stage), for patiently explaining misconceptions I had about both Elixir macros and GenStage
- **rustedgrail** , who helped me write the very first version of [Dwarlixir](https://github.com/trevoke/dwarlixir), the project which eventually became the _raison d’être_ of my ECS framework

For everyone else who helped, and whom I didn’t list: thank you! I’m sorry I forgot you in this list.


