---
layout: post
title: 'Kick out the Jams: ep2'
date: '2018-05-24 02:12:00'
author: Danny Nissenfeld
---

Let’s not call it a comeback. We’re well into week 3 but that title up there says this is episode 2. Really this is episodes 2 and 3. I know it’s a bit confusing but let’s move on. Week 2 was a dead week for me. I knew it would be going into this. What I did not know was that some time Saturday morning I would find The Motivation(tm). The elusive thing that gets someone to program at night after they’ve spent all day at work programming.

Most of the entire weekend was spent coding and testing but also putting everything on a live server! [http://echoes.swiftausterity.com/](http://echoes.swiftausterity.com/) is where this strange project will live forever. Speaking of strange let’s get into it. There’s a whole bunch of technical things happening on the front and back end and both received a significant amount of thought and attention.

### Front

The front end started out a bit wonky. I had found a javascript lib called [Textillate](https://textillate.js.org/). Textillate essentially chops up every letter in an html container into separate spans and does css animate against it. This would be the starting point for me. I could make text bounce, zip, spin and fade onto and off of the screen.

My next step was to try and make text go in different directions. The first stab was a bit… bad. I utilized the writing-mode property. I used the **writing-mode** property by itself without incorporating text-direction logic so things were kind of hard to read. This was an important step, however, as it forced me to consider container bounding. What resulted from that thought was a not-entirely-perfect system that would take a length of text and calculate the height and width of its container so that it could wrap the text over multiple lines to keep it within the box.

That logic is what lead me to using the **transform** property. The writing-mode property, even if I had discovered text-direction, made the bounding wrap difficult in many situations. Transform would allow me to rotate text along a 360 degree axis which played a lot nicer with the bounding math.

The next step (and possibly one that wont happen until after the jam is over) is to incorporate a fully featured kinetic typography system where incoming text understands the positioning of all other text and can align itself properly without overlap like piecing together a puzzle or playing tetris.

### Back

The back-end mostly received a lot of refactoring and fixes. The cache system was sorely broken with the transition from .net framework 4.6.\* to .net std 2.0. (which is what core uses) The beginnings of the [engine](https://github.com/SwiftAusterity/MarkovianEchoes/wiki/Methodologies) are already hooked in and actually working as of May 3rd.

The cache system was an interesting problem. The old cache present in .net framework relied on what IIS makes available as a web application. This was a fairly easy thing to work with as it had full iteration capabilities. Std’s cache does not have any iteration capabilities at all given it is the standard library for ALL .net applications and therefore does not have the luxury of relying on IIS’ systems, thread pool or memory access.

Of course most of my real problems came from wanting to refactor/change as little as possible which is, generally, the worst way to approach code. You will end up spending more time figuring out where to cut and checking the results than doing the actual cutting if you just dive in. (this is not a methodology I recommend for the medical field, mind you)

### Inspiration

Theorizing and developing the engine and this code has really changed my thinking on quite a bit of my fundamental designs for the netMUD platform. There will likely be more of that on [netMUD’s design space](https://www.reddit.com/r/TwinMUD/) in the coming weeks but right now it’s all still bouncing around in the noggin.

### Where it’s going

I mean, it’s up live right now but clearly it isn’t done. Almost all of the actual engine work, “the game” as it might be called, is yet to be done. All of that work is also theory and conjecture. It needs to be done and tested to see if it feels right.

I am not even sure what it is supposed to do or feel like. I am hoping at some point I will see interaction and response that makes sense, at least to me.

### The Jam Itself


The Jam is going exceedingly well. Zach announced in the May newsletter that we’ve almost reached 60 participants which is unfathomable to me. I was **hoping** for a dozen and we’ve gotten magnitudes beyond that dream. The entire guild is expanding in so many ways with the jam, active participation on the slack and in newsletter subscribers. It is all so amazing.

