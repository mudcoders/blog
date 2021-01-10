---
layout: post
title: 'Off the Cliff ep3: Crafty Crafters Craftily Crafting Crafty Crafts'
date: '2018-01-26 02:51:00'
author: Danny Nissenfeld
---

This is truly larger than just crafting but I couldn’t resist the title. Crafting is the heart of my macroeconomics model, but the heart is only a small part of the body as a whole. Were going to have to start at the beginning again with this one so buckle in and hopefully enjoy the ride of failures.

At the original time of inception, back when twinMUD was just Persecutions and Paradoxi and it was just stock DikuMUD — and it wasn’t even twinMUD since it only listened on one port — I was playing a few MUDs aside from Discworld. One of them had something to do with Dreams and at some point they introduced a fairly shallow stock market system. I, being one of the more renowned and powerful players, promptly broke it so badly I managed to drive my bank account so high it exceeded [max integer](https://en.wikipedia.org/wiki/2,147,483,647) one night and left me with negative max integer in funds.

My initial economics model stemmed from that. I created an automated “stock market” of sorts driven by object type. Weapon, Armor, Food, Drink, Container, Trash, etc. all participated in this simplistic demand system. If you bought that item from a shop the stock value went up; if you sold that item stock value went down. Over time the value normalized back to 1. This number served as a multiplier for sale and buy prices in the NPC shop code.

My players, naturally, broke my system too. Once they figured it out a bunch of them colluded. They would burst sell a single type of very inexpensive items to drive the multiplier down and then buy a bunch of expensive ones to take advantage of how the math worked. Once the stock value normalized they would buy back their cheap stuff to drive it up and sell all their expensive stuff to profit. Merchants had no actual money so they generated tons of gold from thin air all within a few minutes.

In all honesty it didn’t much matter at the time. There was no consignment or auction houses. Players didn’t need to actively trade so the fiat currency was pretty much pointless and — much like in Diablo II’s economy — merchants didn’t sell anything particularly useful unless a player had sold it to them first; drops were king and there was no functional crafting yet.

This is the state of most stock DikuMUD derivatives. The economy is an illusion, and only a robust and active player base can spur a barter economy to thrive. The hidden “stock market” was more or less a toy to cause slight variance in shop prices. For what it was worth, it accomplished that and did create a bit of a supply and demand system when it wasn’t being exploited.

My first step towards financial redemption was to make crafting work. At the time you could create potions and explosives. I had some crafting code I got from somewhere that resembled the Oasis OLC system, but it was using values for objects that I had gone well beyond by then. I would have to overhaul the entire thing which is something that would never happen. What I did get from it, though, was a new NPC merchant type: the smithy.

Smithies would on a timer create randomized equipment and sell them. On boot they would generate 10 pieces; 4 weapons and 6 pieces of armor. It used the same code from the crafting module I found but randomized all the values within parameter ranges. The new smiths became fairly popular as they could generate better equipment than could be found in drops.

Their code was also co-opted into the new Champion system (another Diablo II rip of their random elites) to generate “epic” armor and weapons for the champion spawns. The champions ended up providing the highest tier equipment, but much like Diablo II you might end up with a claymore sword that boosts intelligence and wisdom instead of strength.

Champions and smiths began a bit of a war with the builder staff too. Builder level staff could create equipment too, but their max puissance caps were lower than what champions — and even some smiths — could generate. Caps could be violated with permission from higher ranked staff, but that never actually happened. Generated equipment was intended to be the most powerful you could get until player driven crafting could be debuted.

Other wrinkles were added to give economic agency to the players in the meantime. Body profile armor became a thing with races having different overall sizes. The sprite race was handed an ability (and they still retain it) that resized equipment to fit a target character’s body. Liquid Metal became a material which could become any weapon or armor type by using a command to shift it, creating essentially a loadout gap filler for players. The smithing system was also expanded to accommodate the first steps towards what is now the modern economic system.

Materials could now be gathered (ore, wood, leather) and refined to produce raw material. If given raw material, smith NPCs would use it to make their next piece. Depending on gathering skill, raw materials would extract with different levels of potential puissance, which would drive the initial cap of the crafted object up when used. Smiths were now able to make even more advanced equipment with the players’ help.

This gave economic advantage to certain races. Sprites clearly had an economic system of their own, serving as makeshift tailors. Dwarves had natural bonuses to mining, so they could squat smiths to generate better equipment in use for trading. Centaurs had superior logging abilities and felixi were the most skilled butchers, getting the best hides for leatherworking.

Canidae were also introduced at this point and they altered the economy more than I had anticipated. Canidae players who wear or hold equipment that has room for enchantments will automatically add random enchantments (using the same code the smiths do) over time. The few canidae players started flooding the markets and playerbase with enchantment capped equipment just by sitting still next to a merchant.

I can’t say it was truly exploitative, as that was the system I created. I think it helped the system a bit overall but it was unbalancing. It created a disinterest in gathering as the wide amount of equipment giveaways eclipsed the time consuming and methodical methods of getting quality ore, refining it and hoping the smith NPCs would produce what you wanted.

Actual crafting came in at some point, but by then the population of the world had waned significantly. Crafting truly produced the highest quality of goods in the end, but it was a bit late as twinMUD shut down for good shortly after.

That brings us to the modern day, and to netMUD, which is detailed below. Ignore the talk of the 3D models but players will be able to create their own 2.5d object models and submit them for review.

[https://www.reddit.com/r/TwinMUD/comments/53ol5u/craft\_the\_world/](https://www.reddit.com/r/TwinMUD/comments/53ol5u/craft_the_world/?utm_content=title&utm_medium=post_embed&utm_name=221fa83352d94eff8d0b1abeb3f45d5c&utm_source=embedly&utm_term=53ol5u)

There is more than that, though, in the economics model. Racial identity is a very important thing, given this is a class-free system. Each basic race (one you can start the game as) and the achievement races (ones you can unlock to create new characters with) all have culturally based crafting specialties. The post below will outline who gets what:

[https://www.reddit.com/r/TwinMUD/comments/7ggizi/racial\_rework\_identity/](https://www.reddit.com/r/TwinMUD/comments/7ggizi/racial_rework_identity/?utm_content=title&utm_medium=post_embed&utm_name=47914a3cf8754e6aa0759e23ad04c562&utm_source=embedly&utm_term=7ggizi)

What this means is when you create a loaf of bread as an elf the bread will gain random buff affects based on some internal advancement numbers. Bake more goods and the buffs will get better. All of the crafted randomization is puissance-neutral so the cap will be raised in accordance with the affects applied leaving the same amount of “enchantability” intact.

All crafting will also be done via the web interface. You will navigate your character to the appropriate room but the actual crafting form where you input the values will be a popup page.


