---
layout: post
title: 'Off the Cliff ep1: Health Systems'
date: '2017-12-31 02:08:00'
author: Danny Nissenfeld
---

Off the Cliff is going to be a new series where I do a deep dive into one of my own mechanical designs. Some will be more of a post-mortem on something that used to exist but was scrapped, some will be how a particular system evolved over time, and others will be designs that have yet to see the ASCII of day. I do a **lot** of design work. My primary hobby is designing systems, and most will never be implemented but they all serve a purpose of mental exercise.

This system in particular is of the “evolved” type. Every game has some sort of health system. Most seem to opt for simply calling it Hit/Health Points (HP) but it’s a far larger system than just the number that marks life and death. This is something I mostly took for granted over the years and it left me with a very similar system to everyone else. Everyone had HP, attacks reduced it and healing restored it. Resistances and armor prevented loss.

My initial stab at making my system arbitrarily different was to rename and expand it. HP then became (B)lood (L)oss (P)otential. BLP was the absolute floor where you died if it hit zero. Additionally every living thing, through their race, acquired anatomical parts. Each part also had its own percentage based status number and those had consequences for hitting zero. Head and torso was always death. Arms, legs, and variant parts were a loss of use.

There were now two ways to die: full on zero health (BLP) death and getting hit in the head/chest too many times. This system would remain in place for the rest of the game’s online lifetime with a few variances here and there. Honestly it wasn’t that much of a difference in practice.

It felt better; it felt more “realistic” to me. Chests were generally heavily armored, though, and heads were harder to hit. Spells and ranged weapons only dealt general damage so the anatomical numbers remained the same.

It lead me to create the most regrettable system I ever designed: limb loss. To increase the importance of targeting other bits, limbs could now be torn entirely off. Heads were a lot harder to remove but they could still be severed. Limb loss was a bit of an amusement at first launch, but quickly became a nightmare. AI had a habit of picking things up and wielding them as weapons. Limbs were mostly bone and served as low powered maces. NPCs were now tearing the arms off of lower ranked characters, wielding them, and beating the players the rest of the way to death with their own severed arms.

It was kind of funny in an extraordinarily tragic way. Plenty of players did not see the humor. Limb loss was removed less than 2 months after it went live. Following that, the health system received few revisions. A medicine/triage mechanic was added and refined to deal with limb-specific damage and magic healing was relegated to only assisting in restoring blood loss.

Fast forward to today: the MUD has been offline for over a decade and I am roughly twice the age I was when I decided renaming something was “better”. I can more readily see that I still don’t like raw number health systems and that a rename and very minor expansion doesn’t cut it for me.

The inspiration for moving forward comes from an unlikely place: The Penny Arcade Thornwatch TCG. Being a Trading Card Game, players have hands and hands have a maximum number of cards. Damage comes in the form of “wound” cards that get put in your hand, limiting your future play options.

This is the system I want for health. The more in-depth post about the new Wounds system is linked below:

[https://www.reddit.com/r/TwinMUD/comments/7gf5yh/no\_more\_hp/](https://www.reddit.com/r/TwinMUD/comments/7gf5yh/no_more_hp/)

It affects more than just how health is tracked, though. It changes the entire system and I believe it does such for the better. I find general Diku-inspired MUD combat pretty boring. A thousand attacks are inflicted over a dozen+ rounds of combat, hoping you reduce the opponent’s red number faster than they do yours. Yes there are spells and skills and equipment and all that but ultimately it fails entirely to resemble actual armed combat.

You can’t really sustain all that many injuries to the same anatomical location. Wounds also compound. Cutting an existing cut stands to make it a much larger gash. Pounding on an existing bruise threatens to cause major internal bleeding. Real combat is mostly intimidation, sizing up the opposition, and waiting for the opportunity to strike.

Feedback becomes a lot more visceral. If you hit it won’t be a status line of “Your **iron sword** \_\_\_\_\_s the **rat** ”. It will be far more natural to produce a message like, “You swing your **iron sword** towards the **rat** who **dodges** to the **left** and is caught by the **tip** inflicting a **light scratch** to its **torso**.”

My old prose combat translator did the legwork on producing something akin to the second line but it was a translation. Having actual graded wounds produces a more natural language flow. Even better, lines like this could arise:

“You swing your **iron sword** towards the **rat** and **slash** across the **torso** causing its **light scratch** to rupture into a **heavy gash**.”

It also makes the healing aspect far more interesting. Upon examination/triage a character won’t just display a bunch of status bars “Torso: 50/200; Arms: 85/100” but an actual collection of wound descriptions. Burns, bruises, gashes in addition to shifting diseases and poisons to actual “infection” wounds as opposed to pseudo magical affects. This type of system makes curative magic and skills explode with flavor and mechanical design.

This is really where I realized your game’s “health system” is so much more than just having a bunch of status ratios and some “cure \_\_\_\_ wounds” spells. Bandages don’t have to be just pieces of cloth infused with bs instant status changing magic. Truly getting my hands into this as a system versus a simplistic mechanic has inspired me to reevaluate all simplistic systems.

So now I am on a bit of a crusade against numbers. Yes there will always be numbers under the hood but the wounds system proved to me that I could eliminate far more than I thought possible.


