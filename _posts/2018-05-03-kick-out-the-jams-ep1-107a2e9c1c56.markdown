---
layout: post
title: 'Kick out the Jams: ep1'
date: '2018-05-03 02:08:00'
author: Danny Nissenfeld
---

Ok so technically the MUD Coders game jam has been going on for 8 days but it’s been pretty busy here in the midwest for me so I’ve been a bit quiet. This is going to be more of a personal devblog plus maybe a paragraph of “church announcements” of the jam itself.

As far as jam progress goes we’re nearly at 50 participants and the guild roster is growing rapidly. People are posting screenshots and animated demos and things are honestly going great.

The tl;dr here is: slow.

It shouldn’t be overly surprising but progress is slow. I am tentatively happy to say I’ve pressed myself to do at least one thing every day with only thursday and friday being missed days made up for with a few hours put in saturday night.

You can see the incremental progress in the commit history on [GitHub](https://github.com/swiftausterity/MarkovianEchoes), so i wont bore anyone with a day by day timeline.

Primarily i found the [Textillate.js](https://textillate.js.org/) plugin for the UI.

Most of the structure is pretty simple. Much like netmud it’s a web application. Unlike netmud which uses a web page as a client through websockets echoes is just web pages. You don’t receive output from the system unless you give it input which means the entire game lives inside of a post-response structure. The world being created does not change unless it is acted upon.

This is an integral design decision and a core principal to the game world. When you give the system input it will retrieve all changes done between your last input and the current time and play them in order.

This is the Akashic Record. This is fueled by the principal that there is no federation. You choose a name to exist in and someone else might have chosen that prior and done things. You will receive all history of those actions. More than one person can even inhabit the same name and it will be somewhat like multiball mode on a pinball machine. Actions will be recorded and replayed as fast as they happen.

Additionally once you leave that persona remains where it was and experiences things in the place it is. There is no “logging out” because there wasn’t a logging in. Everything you do including existing affects the world being forged and remains intact for others to act upon.

Technically this is accomplished quite like the standard mud. The data structure follows: Place (where), Thing (what) and Persona (who). The why and how are up to you. Nothing is ever destroyed.

Places are created by changing the last element in the site uri under the Existence route. Talking about other existant places will create links between where you are and where you spoke of.

Things are created and modified by acting upon them even if they never existed prior. By acting upon something you will create it. By acting upon existent things you will modify them.

The more a thing or place is acted upon and spoken about the further defined they will become. This is the echo we produce in reality.


