---
layout: post
title: 'Substrate: noun; an underlying substance or layer'
date: '2019-03-31 02:25:00'
author: Danny Nissenfeld
---

It’s been a while and things have progressed far deeper than I had originally anticipated. For memory’s sake: [https://mudcoders.com/off-the-cliff-ep6-substratum-f8fce0e3f159](https://mudcoders.com/off-the-cliff-ep6-substratum-f8fce0e3f159)

The lexica is alive and humming. It is hard to explain how deeply integrated the lexica is to the Warrens game engine mostly because the “game” part of that engine hardly exists. You can load into the game world and move around the few rooms but the only real things happening are those that contribute to the room’s description.

> _The Void feels swampy and freezing. The Void is fishy, microscopic, and abandoned. The Great Sword is in the sky. Lunasword is in the sky. Alto-cumulus is in the sky. You see a vast swordree forest. A large armory of swords roams around you. You see a sparse outcropping of swordock. You hear a loud beeping noise far away. You sense a poofy, sparkly, and serene bridge near by._

The above passage is the one thing I’ve been working towards for the past month or so and ultimately what the lexica has been designed to do from the beginning. None of that was written by hand. I’ll break it down line by line to try and elucidate the gravity of what’s happening.

> _The Void feels swampy and freezing. The Void is fishy, microscopic, and abandoned_

The Void is actually the name of the room we are in. This does need to be retooled to not use the proper name but what went into making these two sentences are a group of single word adjectives. “Swampy” and “Freezing” are both descriptive states chosen by the weather system describing the humidity and temperature of the environment you’re in.

The verb for the first sentence is actually deliberate (feels) but the verb for the second is not (the system defaults to an “existential verb” when one is missing) and the conjunctive measures (and, commas) are automatically added.

“Fishy” is actually an additional descriptive added by the builder (me in this case) which is a Visual descriptive. Descriptives all have a sensory type (visual, olefactory, auditory, taste, tactile, psychic) and descriptives of a similar type are automatically bundled together. “Microscopic” and “abandoned” are both properties of the area describing how large it is and how populated it is by other players/sentient NPCs. The three visual descriptives were bundled together in the Sentence generation part of the engine much like the two weather based tactile ones were for the first sentence.

> _The Great Sword is in the sky. Lunasword is in the sky. Altocumulus is in the sky._

This is more weather influence. The Great Sword and Lunasword are actually the “sun” and “moon” of the world. Warrens simulates a full galaxy to achieve day/night cycles and day time brightness/ambient temperature. They’re both visible so they are added to the list. Altocumulus is, of course, clouds which are not being described very well but the input being used is just a clone of the celestial body input as a temporary shim. All three are simply the proper name of the thing with a direct object “sky” and a context of being inside of it.

All articles (is, in, the) are chosen and added automatically by the system to fill out the grammar.

> _You see a vast swordree forest. A large armory of swords roams around you. You see a sparse outcropping of swordock._

These three are actually natural resources inside of the area that you can see. Everything has the word “sword” in the name for a demo-site joke. Everything is a sword. The amount of trees is “vast”, the “sword” animal packs are considered large (armory is the collective noun for sword animals) and the swordock rocks are not very exposed so you can only see a small amount of them.

These sentences are a bit more deliberate. Everything but the articles and prepositions are included.

> _You hear a loud beeping noise far away. You sense a poofy, sparkly, and serene bridge near by._

Warrens has sub-areas called Locales which entities can enter and interact with things more closely. The locale here actually has a “noise” auditory descriptive that has its own descriptive of “beeping”. The context given for “far away” to be added is “far” similar to how the pathway into that locale (the bridge) is contextually “near”. The three adjectives are just descriptives on the bridge path.

* * *

The lexica, naturally, does a lot more than just describe things flatly. Each word and phrase in the dictionary has a set of three weighted values which cause the system to potentially choose different words than what is provided (such as change warm to hot or cold or freezing automatically) or change the grammar entirely.

> _The Great Sword is in the sky. Lunasword is in the sky._

The two celestial sentences may actually be combined by the system into one sentence by the Paragraph engine resulting in: The Great Sword and Lunasword are in the sky. With a pair of subjects the sentence shifts into a plural context changing the verb (is to are) while retaining the duplicated predicate. (in the sky) This is accomplished by upshifting the “elegance” weight.

The core of the lexica is based on such shifting via the synonym network. Each word and phrase is connected to each other via synonym/antonym/hypernym relativity. While this was something I was originally developing on my own I’ve also recently integrated [Wordnet](https://wordnet.princeton.edu/) which is an amazing resource and entirely aligned with my own theories on grammar construction. Their synonym network is a massive step forward for Warrens and will obviate weeks of manual data curation.

Now you may be skeptical that so much of this is just hard coded. It’s pretty easy to make an engine that’s just static changes and while that may have been true of the lexica 2 months ago it isn’t now.

For one, you can verify my claims [here](https://github.com/SwiftAusterity/Warrens/tree/White-Sand) or simply take a look at the Language editor in the admin:

![Most of the rules](/content/images/2019/03/substratum.png)
*Most of the rules*

This is the editor for a single language in Warrens. The first section is for punctuation and some internal details. The second section is 16 base words (mostly prepositions, articles, conjunctions and needed verbs) and the rest is all rules for rules engines. Each of those grey pillbars expands into a set of properties for each rule. (some numbering in the dozens)

The lexica is driven by seven individual rule sets.

- Single Word Grammar: Defines how words modify themselves. This is where verbs and nouns get prepositions and determination articles and where those fall in relation to the words configured. Words can also have contextual augmentation defined (adding ‘s in english for possessive form) and full replacement in some situations.
- Word Pair Grammar: Much of the sentence structure is had here. This defines the order of word placement in relation to each other. The fact that adjectives get placed before a noun but after an article of the noun and that adverbs and prepositions follow a verb.
- Contractions: Contractions are really specific things and get their own ruleset. This is simple 2 words and the word they become.
- Transformational Rules: Similar to contractions but more complex. This is how a/an gets switched for english depending on the following word’s phonetics and how gender forms change for most other languages.
- Sentence Rules: This is where a lot of the derivation occurs for the varying languages. These rules define the order of sentence parts within the sentence depending on the type of sentence. Languages where verbs fall in the predicate or the placement shift for conjugated verbs between statements and questions in english.
- Sentence Complexity Rules: The newest addition to the lex: this is how the aforementioned 2 sentence combining happens. One can define detection across sentence parts (subject, verb, object) to allow the paragraph engine to find two similar sentences and turn them into one.
- Phrase Detection Rules: These aren’t really grammatical as much as systematic. What constitutes a phrase (2 prepositions next to each other like “near by” or “far away”) can be added and the system will automatically scan sentences that have been built and add new Phrase objects to the data pool which can then be configured into the synonym network for eligibility for thesaurus replacement.

* * *

So that’s the lexica in a nutshell. Once the wordnet integration is finished there are only a few steps left before I turn this into a public API that anyone will be able to send data to and receive descriptions in any language that has been configured. Out of the gate only english and italian are planned (being ridiculously similar in grammar) but there are plans to keep adding more languages as I find people that can help consult on the rules configuration for them.


