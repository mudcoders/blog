---
layout: post
title: The Ring is the Thing
date: '2018-07-10 02:20:00'
author: Danny Nissenfeld
---

People keep saying to “put a ring on it” so I did. I put a ring on the MUD Coders Guild: a Webring.

### What the heck is a Webring?

For the benefit of anyone born after the 80s the sharing economy of the internet was more of a lemonade stand and early on the only real way to promote your web presence was cross-promotion. You’d join these collectives called Webrings — heck, look even Wikipedia can tell you about them: [https://en.wikipedia.org/wiki/Webring](https://en.wikipedia.org/wiki/Webring).

The gist was you put a usually ugly banner in your footer that had a left arrow and a right arrow and clicking one or the other cycled you between all of the sites in the ring. Webrings went the way of hit-counters a long time ago. They were — and are — intrusive to the eye and overall pretty weird looking design.

### Why revive the Webring?

Why make a MUD in 2018? Why anything? It seems appropriate to link a bunch of “dead” genre games together using a “dead” web convention. I swear I tried to have blink tags in it too but apparently there isn’t a single browser that actually honors straight blink tags anymore. I’ll probably console myself in an animated mailbox some time later tonight.

### So let me see it already

Yeah, sure, fine let’s do it. It is as simple as adding an empty `\<div\>` somewhere on your website with a specific ID ( **guildWebRing** ) and including a JavaScript file from my public server: [https://cdn.swiftausterity.com/mcgWebRing.js](https://cdn.swiftausterity.com/mcgWebRing.js)

After that you’ll need to actually get in the ring. You’ll need to be a standing member of The MUD Coders Guild which currently means you need to be in our [Slack channel](https://mudcoders.slack.com/). From there you can ask to join the ring or if you’re more code-savvy you can make a pull request against my [publiccdn repository](https://github.com/swiftausterity/publiccdn) and add your site to the JSON array at the top of the JavaScript file.

Current all that is needed is your site’s name, landing URI, domain (the first bit between http:// and /) and you can optionally specify a flair image, background and foreground color or let it default to the rather startling navajoWhite.

The JavaScript uses no libraries (not even jQuery) and includes nothing so it’s as lightweight as I can get it without minification.

### Why, again?

Really this is twofold: I want to grow the Guild and ProjectWonderful is dying. Banner ads are kind of pricey for what you get and ProjectWonderful tried to do something interesting and affordable in that space but for whatever reason it did not last. Failing a spend of 5+ USD per day for the cheapest ad banner deal I can find and not really wanting to use Google AdWords for a number of reasons this Webring is kind of a fading riposte as I gather wind and wind my springs for a true assault.

The only way to build community is to build **with** community and I can’t see a better way to do that than linking our individual efforts to each other.


