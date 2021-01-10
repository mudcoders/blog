---
layout: post
title: 'Off the Cliff ep6: Substratum'
date: '2018-09-20 02:23:00'
author: Danny Nissenfeld
---

I’ve been toying with something like this basically forever and finally realized what I’ve been trying to articulate. I am going to link my middling thoughts on this idea and while my self-links are normally for context or history this one is one of those “go there and read this first”:

[https://www.reddit.com/r/TwinMUD/comments/8v2al8/the\_lexical\_engine](https://www.reddit.com/r/TwinMUD/comments/8v2al8/the_lexical_engine)

So those are not the initial thoughts but the middle ones. I’ve moved beyond those as far as mounting significance but they are the technical details so they are important. If you keep reading this without at least skimming that Reddit post the terms and concepts I am about to use will be confusing.

The lexical engine is, at this point, the substrate of the entire netMUD platform. Some of the members of The MUD Coders Guild are a part of the staff for Important MUDs. They have large populations and/or have been running “forever” which in mud terms is 20 years or more. I won’t say they’re all recent additions to the community but some of them tend to hip-check my design rantings with “What benefit is this to players?”

It’s a question I tend to ask other people about their designs because for the most part people design games with the intent of actual live humans playing them. I, in contrast, am not doing that with this game. While the netMUD platform itself is intended for use by the standard player-centric design ethos the game I am making out of it is being made for the AIs that will exist in the game world.

This thing, the lexical engine, was originally conceived to facilitate the AI. It ensures all system borne output is already parsed and codified with meaning so the AI layer won’t have to parse free text every time something happens.

I’ve come to realize this goal can’t be accomplished without upending the entire system. Every single description and line of output must be constructed programmatically (except help files). Even one source of uncodified free text corrupts the entire process.

So I went about that goal and that Reddit post is the technical result. Then The Guild gained a new member. A fellow who ports and translates MUDs to Portuguese for the mostly visually impaired community where he lives.

I had always intended for netMUD to have a UI translation layer so that it might be at least functional for non-English speakers. As I looked into moving the human language layer ahead in my schedule I realized the lexical engine doesn’t just translate. Given the entirety of output is constructed first using the lexical engine and then built out using language based grammar rules even English is technically a translation.

This meant every language I can implement will be treated by everything, except for player speech, as a first order translation. I won’t have to take free text English and awkwardly translate it to French or Spanish or Portuguese. I can build entire dictionaries and rule sets with the help of people who speak these languages and have the system build output using any language. I could have staff making content in Portuguese and have it come out in English next to English content coming out in Portuguese.

This was the first revelation.

The second, and perhaps more important one, is that I have invented procedural generation for user experience which warrants deeper definition or perhaps some examples.

Right now on the platform (which is live, by the way, at [netMUD.swiftausterity.com](https://netmud.swiftausterity.com/)) you can make a character and enter the test client. You’ll be put in the testing zone where you can see the zone description. This description is an amalgam of all conditions in the zone which currently includes temperature, humidity, celestial bodies (the sun and a single moon at times), and an extra feature which is a smelly bog.

Originally these extra features, descriptives, were the end of the road for lexical processing. They’d be things you couldn’t get to like large craters on the moon, clearings in the distance or immovable wall decorations.

While fleshing out zones describing themselves the second revelation was that everything could describe itself. If I put mandatory descriptions on structural data like Material (what something is made of) and Affects I could not only have an entity describe itself more fully I would have a description that mutates itself in response to changes in real time.

If I make a particular wood material smell “earthy and nutty” then anything made with that wood in game would also smell of that. If I make the effects that live material rotting adds to an object’s smell, like rot, then anything that is decaying will also smell like rot, and you could potentially track corpses rooms away by following the rot smell.

All of this comes “for free" in the system I designed to facilitate my AIs. I don’t suggest anyone “try this at home” as it is definitely not easy and not intuitive. I spend quite a bit of time trying to figure out not how to describe a sword but how to codify describing every potential kind of sword all in the same procedure.

I am not sure how easy it will be to find people willing to build for such a system. There are no handwritten descriptions at all. This may be the final point that tips it all over to the point where I am alone in building the game I want.

Still, though, I think I’ve found what I’ve been trying to accomplish for the past 25 years.


