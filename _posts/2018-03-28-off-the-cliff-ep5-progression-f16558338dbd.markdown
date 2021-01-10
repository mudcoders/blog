---
layout: post
title: 'Off the Cliff ep5: Progression'
date: '2018-03-28 02:04:00'
author: Danny Nissenfeld
---

Of the numerous [Discworld MUD](http://discworld.starturtle.net/lpc/) inspired things, the level-less skill tree system has probably had the most impact on my world. As with many things, though, even that has been left behind in the current design. Turtles and the number eight aside, the world didn’t start off skills-based—and it won't end there either.

Originally, I had levels, experience points (XP), and classes—very few games deviated from a class-based system back then, and even Discworld has its equivalent with guilds. Levels ranged from 1 to 99, and the amount of experience needed to level was set at 1000 per level. There was [remorting](http://mud.co.uk/dvw/whatisremort.html)(which was something I had modeled after the remort system in the old Nightmare MUD I was playing) with full stat and skill retention so you could grind your way to a pretty strong character.

The first change was pretty minor and came along with the whole “twin” part of twinMUD. Classes were cloned for the second port and given new names. Multi-classing also gave you new combinatorial class names so a thief (rogue) / heretic (cleric/priest) would be a “politician”. You wouldn’t gain anything by this, but so it was mostly for the jokes.

After having played Discworld for a while, the next change was a major one: I removed classes and levels entirely in favor of skill trees. A bit of an explanation (with the most recent tree design) can be seen here:

[https://www.reddit.com/r/TwinMUD/comments/4z1fyu/skillsbased\_advancement\_system/](https://www.reddit.com/r/TwinMUD/comments/4z1fyu/skillsbased_advancement_system/?utm_content=title&utm_medium=post_embed&utm_name=382eb53f217d4411866e0eb8c5a26f79&utm_source=embedly&utm_term=4z1fyu)

In its initial form you gained experience points for doing literally anything, and could spend those points by training skills with players that were more advanced than you, or with trainer NPCs. NPCs also had the full skills tree, but still had levels. The numerical level of an NPC became stats, health, and bounty (how much experience they gave for killing) modifier.

The consider system remained a bit useless after this. At some point, I came up with a better formula based on the combat AI the NPC had, as well as an equipment and skills tree comparison. This small design change would be the most important inspiration for the current system, but we’re still a ways from there.

Another thing Discworld had were on-use skillups for their skills tree. I wanted them, but that rework meant quite a bit of plumbing and occupied the remainder of the design time. I renamed skill tree points, normalized and denormalized over the last few years of the game’s active life, but failed to iterate it much more.

The new rebuild started off much the same. I still had a skills tree (though revised) and experience points. Quite a bit of the redesign on the new build is to make things more relevant and interrelated. Amidst people complaining about having to eat and drink in a MUD, I had a revelation: I wanted to make needing to sleep a positive feedback system, as opposed to the current negative one.

Sleep has been classically pinned to the insomnia system. If you fail to sleep you will develop insomnia. Insomnia debuffs give way to insanity affect debuffs. **_You don’t want insanity debuffs._** The new sleep system would be a “rested XP” system.

Earned experience points would be split into two pools: long term and short term memory. You earned XP into short term memory. If you die all short term memory is lost. Sleeping converts short to long term memory thereby “protecting” the XP you’ve earned so you have more time to use it.

This also tied into a dreaming system whereby while asleep a gestalt of you would enter a “dream zone” and you could fight things for more experience and skill-ups. The dream zone would be constructed of things you had encountered recently, pieced together randomly. This XP conversion is something I’m still rather fond of despite it not having a place in the new system.

This brings us to the latest systematic inspiration: the removal of all numbers. It started with the wounds system replacing HP (if you’ll remember a [prior Off the Cliff](https://mudcoders.com/off-the-cliff-ep1-health-systems-99389be1730) about that) but ended with removing the entire skills and stats system—and XP [went with it](https://www.reddit.com/r/TwinMUD/comments/7hqbty/an_end_to_numbers/).

Everything in the system is now on-use advancement. It’s also on-observation as explained here:

[https://www.reddit.com/r/TwinMUD/comments/7ozzoa/death\_and\_taxes/](https://www.reddit.com/r/TwinMUD/comments/7ozzoa/death_and_taxes/?utm_content=title&utm_medium=post_embed&utm_name=6d44d38178f44422872b0296c7002860&utm_source=embedly&utm_term=7ozzoa)

As much as I liked the tie-in for sleeping, I feel like this system fits a lot better. Every skill can be linked to others (like stuff involving arms or swords) for cross-advancement and training. Dreaming can still be a part of it by providing on-observation levels of advancement as you use things in your head.

Consider is also taken care of by this in a much more natural way. That heavily armored guard might be a complete wimp physically but you wouldn’t really know it so the observational consider will be skewed. You can strut your 6 pack abs and ward off potential cutpurses. It feels a lot more realistic in ways the old hard number lists could never achieve, without needing to have hidden engines genning up interpretive descriptions.

It does have its downsides. The attributes will be a fairly large pile at some point, with the primary ones reaching soft cap quickly and the lesser used individual tags stagnating. Displaying these in an intuitive and clean manner for players is probably the biggest challenge. The primary “character score” screen being web based makes this a bit easier, though, as a set of tabbed grids with a search feature will be used.

I am curious to find out as this gets built if I’ve gone too far with the crusade against numbers. While denouncing stats and skill trees, there are still numerical representations of stat-like things, and passives/active abilities still have a number rating themselves, so it’s a bit of a conceit to proclaim a hatred of numbers in general.

I feel like it’s still a significant shift away from character template sheet style design, and a more natural system to discuss on a technical level. When you’re in an RP MUD and people are having to ask if someone’s “dexterity” is higher than 15 for a lockpick check, it’s immersion busting. If you are caught unable to see a player sneaking in and out of rooms, and you openly lament that your “covert.perception” is too low, that’s immersion busting.

Trying to unravel that and build systems you can discuss technically and still be in the world at the same time is a huge design goal for me and I feel like whatever it takes to accomplish that is what I need to do.


