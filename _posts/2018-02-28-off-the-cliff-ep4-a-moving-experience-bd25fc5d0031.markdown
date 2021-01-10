---
layout: post
title: 'Off the Cliff ep4: A Moving Experience'
date: '2018-02-28 02:54:00'
author: Danny Nissenfeld
---

Movement in MUDs is not something people thought about all that much. There was the occasional full-coordinate based system, but mostly it was a lot of “north” and “enter door” back in the 1990s. Things are a bit different now for me — having recently undergone quite a bit of rework trying to figure out how best to represent physical movement — and it’s worth reflecting on, given the fact I ended up in mostly the same place I started.

Long, long ago in DikuMUD you had 4+2 cardinal directions: north, south, east, west, up and down. In some places you could name these to make “doors” so not only could you go “north” but you could also “enter door” to get there. Some people added more cardinals (northeast, southwest, etc.) and some people introduced fully independent portals (doors that had no directional basis) but it was mostly the same.

Most worlds didn’t actually care if they made sense. You could path rooms in such ways that if represented realistically would have resembled Klein bottles. Mine didn’t.

The first significant changes came with the “bump” system. A new builder came on and wanted to have a way to make players run into a cactus if their dexterity was too low. It seemed like a pretty reasonable request, given there were already ways to restrict pathing based on skills like climbing, swimming, and flying. Thus, bump was born (and, a topic for another time, the initial design cues for Kender kleptomania). As a bit of a digression, what I did not know is that the cactus was merely an excuse. Later on, when I and my senior staff member would do a full review of her new area before linking it to the main world, we found that she wanted the bump mechanic so she could make dildos. Loads of dildos. Dildos all over a large house. Dildo weapons. Dildo loot you could trip over. Dildos stuck to walls that you could accidentally fall into.

It was ridiculous.

Sex toys aside, bump had me rethinking a lot of things. It gave way to a lot of changes to the combat system, making it more active and mobile. Movement became more than just wandering from room to room. Rooms had physical space now, and everyone and everything had a position in that space. You could potentially be facing towards — or away from — something which limited perception. Movement was still basically going in a specified direction from one room to the next, but you could also move _within_ the rooms.

Fast forward to one year ago, at the beginning of The Great Redesign. The initial prototype started with rooms but as some point I decided rooms wasn’t enough. I wanted a full coordinate based open world.

This design went through a **_LOT_** of iteration, and even consumed the master branch of the code for a solid few months. I never actually got a working prototype, mainly because the movement never felt right. With coordinates, movement had to go from a deterministic tile-by-tile room system to a relativistic momentum system. You couldn’t just “go one unit north,” you had to start walking north and stop yourself when you wanted to change direction. It was primarily modeled after Eve Online (yes, a space game). You could tell the game to move you towards a specific landmark (or object or person) or simply “walk direction” infinitely until you ran into something impassable or issued a stop command.

No matter how I thought about this, it never felt good without a graphical map representation. The bad points outweighed the benefits by magnitudes, but I just kept hammering at it thinking there had to be a way to make it right. A month or so back, when I picked things back up, I recognized that this had to go.

I took a real hard look at what benefits I wanted to reap from it, and came upon a revelation: I can introduce a more striated room-like design. Typically, MUDs collect rooms into meta supertypes called areas, or zones. These data points don’t do a whole lot normally, but this would be the starting point for me.

Initially, I wanted zones to be coordinate-based areas that contained collections of rooms, called locales. Nothing could really happen in these zones other than moving from locale to locale, or between zones. Borrowing a bit from ancient JRPG designs, NPCs could be in zones, but if one initiated combat it would randomize a locale up like a pseudo-arena, and combat would happen there.

This felt “interesting.” It raised a lot of questions like: What if you die? How long will these arenas live without anyone in them? What if you escape? What happens to the NPCs and objects in them? I didn’t like trying to answer these questions. It felt counter-intuitive from every angle.

Coordinates were dropped entirely at that point, but not the zone-locale design. My main design goal was to have a massive and expansive world that rewarded exploration, but genning up millions of rooms would just create a ton of pointlessly empty space. Zones and locales [would solve this](https://www.reddit.com/r/TwinMUD/comments/7kvx2l/world_zone_locale/).

Zones remained inhabitable, but now resembled something more like ports in Sunless Sea, or locations in Fallen London. They were now descriptions with occupants and locales (still collections of connected rooms) in them. You could move from zone to zone, or enter locales and explore their rooms. Nodding to my desire to not have to build out thousands of rooms by hand, each zone would also have “adventure” locale configs. Players could “explore” a locale config, which would randomize up a new locale and [put them inside](https://www.reddit.com/r/TwinMUD/comments/7muquj/procedurally_generated_locale_mechanics/).

Building out ~90 zones then made for a fairly large world that wouldn’t feel quite so unoccupied. Randomized adventure locales would contain the bulk of the world and gating zone-to-zone movement behind actually exploring them at least once or twice ensured exploration had meaning. I feel like I’ve arrived at an actual workable solution finally that meets [my design goals](https://www.reddit.com/r/TwinMUD/comments/7ko8nz/the_world_stage_continental_structure/).

Always question your designs. There might be something better out there.


