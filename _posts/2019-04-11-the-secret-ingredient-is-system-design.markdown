---
layout: post
title: 'The Secret Ingredient is: System Design'
date: '2019-04-11 18:00:00'
author: Danny Nissenfeld
---

That joke is probably not well executed; we’re not in Kitchen Stadium and I am definitely not taking a bite out of a raw bell pepper for anyone’s amusement. Still the title is probably better than the one I used for [this](https://www.reddit.com/r/TwinMUD/comments/b8jr5y/cooking_design/).

The script is being flipped a bit as I plan to go further into depth here in this article than I did in the original design post I just linked for you. The real rub is this isn’t just the underlying mechanics for the food preparation system, it’s going to be the mechanical basis for nearly everything.

[Warrens](https://github.com/SwiftAusterity/Warrens) has a system called Qualities. I’ve gone into this one before but it’s essentially the same mechanical basis Fallen London and all of the Failbetter games have. You have Qualities which are just a string/int key value pair. Qualities can, and do, describe literally everything about you. You’ve got qualities in Sunless Sea to keep track of how many times you’ve visited each port which drives some events forward such as the Sisters near London and their unfortunate house fire. You’ve got qualities that describe things about you and the crew more intimately like if you have a taste for the peculiar meat, a.k.a. cannibalism.

In Warrens they also describe most anything, but it’s a bit more nuanced. You have systematic qualities which replace traditional Stats, like strength, muscle definition, or dexterity. You’ve got qualities that define your history which will be used for quest tracking purposes. At the bottom are the qualities that track mundane aspects of your being, such as Wet or Smelly.

This is, in and of itself, the base mechanics of everything. If you jump in a puddle you will become Wet. If you find something that has Dry properties, or is made of something that is associated with being dry (such as sand), it can be used to remove the Wet quality and itself will lose dry quality or gain wet quality. This is also not a hard coded, or even hand-data driven, design. Water is a Liquid state (of water) and liquids can be absorbed into materials or cover them (wet) based on various material properties. You will be Wet (Water) by getting liquid state water on you. Through the power of the lexica, various things will be considered Dry. Dry, also via the lexica, is the opposite of wet. Dry things can remove Wet through the magic of synonyms. The Lexica isn’t, for once, the point of this article so let’s move on to the new stuff.

Quite some time ago, I put up a design that would upend the Warrens design and enforce states of matter and energy to everything. The materials system would be completely removed and everything would be made of atomic structures (I’m not kidding) or a form of energy. Everything would _then_ be governed by the new Reactions system. Every single atomic structure and energy form could be mapped with reactions which would happen in real time as things in the game—a text based game—touched each other. It’s one of the few systems I’ve deleted from my design Reddit. The amount of work required to build this out is exponentially larger than the lexica will ever be.

Some of these ideas, however, are not bad. Well, one of them isn’t bad: reactions. While I thought of this for cooking first this really makes sense as part of the Qualities substrate. I already wrote it out in the design entry so…

> Each reaction will consist of a name, a nullable second quality, a minimum critical mass, a ratio, a catalyst (elemental type + value) and a result set. HMR already has something similar to this so that reaction system will probably be coopted.

HMR specifically has an [Actions](https://www.reddit.com/r/HMRMud/comments/9lvxvw/scriptless_scripting_a_working_theory/) system and to be honest it’s pretty well developed and already has a competent web admin. What’s needed out of the Actions system is the results data set. What or who is affected, what the resulting spawns (npc, items) will be, how much will spawn as a ratio, what energy release is there (explosions) and what happens to the original components. It even has a reaction grouping system that allows multiple outcomes to be randomly chosen with accompanying chance ratios.

As an example: A leaf item, which is a necessary part of making plant data, may have a quality of Tannin which may react with Water with a heat catalyst adding a `<leaf name>` Tea quality to the water, while simultaneously adding Wet (water) quality to the leaves, and removing some portion of the Tannin quality.

These reactions can be set up individually, such as the Tea brewing one above, or in a chained sequence for something like baking a cake. You could chain Wet flour with egg into batter. Batter with heat into flat bread. Batter with yeast and heat into bread. Batter with butter and heat into pancakes. And change the ratios to make waffles or crepes.

The possibilities are literally endless and all of it is tied into the lexica, the qualities system and is ultimately data driven.


