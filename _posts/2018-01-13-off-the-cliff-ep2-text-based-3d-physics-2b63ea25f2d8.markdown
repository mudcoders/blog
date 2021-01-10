---
layout: post
title: 'Off the Cliff ep2: Text Based 3D Physics'
date: '2018-01-13 02:20:00'
author: Danny Nissenfeld
---

A long time ago, in a text world far, far shut down; combat was still stock DIKU code. While the first change wasn’t quite a photon torpedo through the ventilation shaft, it certainly sparked the design rebellion in my head when I came across inspiration in an unlikely place: A GodWars MUD.

I wasn’t much one for playing GW systems. The populations tended to be of the PvP camping mindset from what I had experienced, and that was not my thing. I popped into one regardless, just seeking validation for my opinions, and found something I wanted: combat stances.

It had a psychological element already driving the aggro system. If you got taunted or beat and your HP was high, your “attitude” value would shift towards aggressive; if it was low you would become more defensive. Other abilities could drive this up and down as well. Additionally, NPCs used this to determine their course of action and if one became defensive enough they fled.

Thus the martial stances were born. At the time I was also playing quite a bit of Tekken and Virtua Fighter. I adopted the animal disciplines out of Tekken and split DIKU’s combat code into weaponed and unarmed. Each stance (weaponed, unarmed, unarmed martial) got its own set of branching paths. Essentially, autocombat became a combo system. Weapons and standard unarmed stances were still just hit calculations, but the martial stances had openers, lead-ins, extenders, and finishers. Each stance also had its own set of parameters with regards to attitude shifting, and other factors like stun resist and dodging.

This left weaponed combat kind of boring, and normal unarmed completely outclassed. Martial unarmed still made use of all of the unarmed skills and passives, but had infinitely more value. Weaponed combat was still useful with the right weapon, but clawed races could advance in the martial stances while still making use of a few of the weaponed skills — nevermind Quicksilvers being able to use full weapon stats plus weaponed skills _plus_ the martial stances. Quicksilver monks (there were still pseudo-classes back then) became _overwhelmingly_ strong.

Mitigating this was complex. I created branching combos for each weapon (via damage type) and standard unarmed, but it felt… bad. Plain unarmed would never truly compete on its own, and the weaponed combat just felt wrong entirely.

Plain unarmed would be helped by Virtua Fighter’s design with the introduction of the grapple and counter system. While in a stance, unarmed counters (“redirect”, “reflect” and “blade catch” passives) did not activate. Crane and Monkey stances technically had similar effects, but those situations were more rare and none of them could disarm someone automatically or cause them to hit themselves or an ally. Grapples were a whole other ballgame. None of the stances could utilize a grapple command, and you could do a **LOT** once you had someone grappled. While it halted autocombat, allies could pummel enemies while you held them, and you could do things like choke holds, arm breakers, and suplexes.

Unarmed is more of an aside though. We’re really here to talk about how mitigating the problems with weaponed combat changed the face of the game’s design entirely.

The combos that were added to the weapons mostly followed actual weapons training. Impact damage types (chop, bash) were more boring and mostly gained unarmed linkers and finishers (overhead axe chop, chest kick finisher). Slash damage types (swords) gained follow-thru motions like secondary twirling slashes and backwards return slashes, as well as hilt smashes. Daggers gained multi-proc linkers for stabbity action.

It was still kind of blah, especially with daggers. Stabbing someone a dozen times in a row might have won me the state Soul Edge (arcade) championship as Taki but it looked really dumb in text form. I wanted stab weapons to be able to slice and slash too but what if you were wielding a spear, ice pick, or foil?

I am not sure when it hit me, but I decided to create a 2d model system. I did not yet have a full blown physics engine — and would not for at least another year — but it became the stepping stone for everything else that would be done.

Initially every weapon type object had a 1x10 grid of a character+number pair. The character was the damage type that would be inflicted if something was hit by that part of the model, and the number was the “weight” of the part (out of a total 10). A sword, for instance, would be 1 pierce (tip), 6 slash (blade), and 3 bash (hilt) starting from the top. The tip and blade would get higher weights while the hilt got all 0s indicating it wasn’t the bit you wanted to hit with. An axe would look more like 3 chop (blade) and 7 bash (handle). Hit chance became more a calculation of what part you hit with rather than an all or nothing affair.

I actually felt pretty clever at this change. I could have a unified weaponed combo path engine relying on the physics models to guide linkers and finishers. It also happened to display in game pretty effectively for most things right off the bat.

Minor tweaks became apparent quickly. I didn’t like “0”s being anywhere so the out of 10 weight became out of 20 and the model went from 10 to 11 so there was a center point to each model. Each of the 11 layers also could be named for the combat engine to use for display. Unarmed combat (including martial) got their own pseudo weapon models to use in the hit calculations.

After this is where the idea train went completely off the rails. It hit me that I did not want to have object types anymore. Like a holiday episode of Oprah, I had to give every single object in the system its own physical model.

The first change was to shift it from a damage type driven model to an actual two dimensional object model. The 1x11 grid became 11x11. This change, while time consuming, created a far more intuitive model system. The thing you made actually looked like a representation. You could have a spiked hilt on your sword or a real nailbat. Maces could have spiky heads and you could create a serrated blade.

It didn’t complicate the combat engine all that much really, but it did allow for a very significant design goal to be fulfilled which I had mostly given up on: you could wield literally anything in the world as a weapon.

In-game this gave rise to what I usually describe as the Trash Can Era. Metal trashcans were readily available and weighed a LOT compared to most other things. Anyone who could pick up a trash can could throw it and when it hit it did a lot of damage and stun. There were other objects that had significant weight, like bricks and concrete blocks, but they were smaller.

Tossing trash cans at one another was a popular method of combat. NPCs in urban environments also had a tendency to grab whatever was available and got in on the fun as well. They mostly used them for ambushes, though, which if you can envision a cutpurse NPC hiding in an alley and surprising someone by bashing them on the head with a metal trash can is extraordinarily funny. I never really put a stop to the trash can era. There were better ways to conduct a fight (like throwing explosives at people) so it was never a huge problem, just an amusing one.

Trash cans almost catches us up to today. The revival (NetMud) saw a stab at my true original goal: a fully 3D model system. The 2D system assumes object supersymmetry. It makes calculations against the idealized plane of the model, as opposed to something more realistic. You could never hit someone with the flat of a sword unless the blade was so wide it had a center point.

In the downtime between what TwinMUD was and what NetMUD would become I specced out a 3D system utilizing an 11x11x11 plane. 11 z axis slices of an object. It was created by uploading a text file containing all 11 11x11 planes utilizing a centered top-down view of the object.

Actually creating each model was exhausting. I managed to make 4 of them: a dagger, a sword, a t-shirt, and a backpack. I created an engine that would normalize 2 models for dimensional size and overlay them to determine if one could be worn by the other. This was especially important given the huge form and size diversity the playable races have.

I got up to the implementation point of created a web based model viewer with axis rotation buttons. Still no online construction (only upload) but the viewer also made use of a new piece of transformation code so the system could be rotating and skewing objects for fit determinations. The code would actually tell you if you could squeeze through a window or hole in the wall.

I would never manage to get the rotation code correct. I got derailed for three solid weeks of coding getting it to work and in the end it would always end up failing full testing. A full writeup on what would be my first major design concession can be found below:

[https://www.reddit.com/r/TwinMUD/comments/574hy6/physics\_and\_the\_3d\_model\_system/](https://www.reddit.com/r/TwinMUD/comments/574hy6/physics_and_the_3d_model_system/)

I feel like it’s still in a good place. It’s not perfect but design concessions are the name of the game right now after all.


