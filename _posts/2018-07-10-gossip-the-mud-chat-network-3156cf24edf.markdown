---
layout: post
title: 'Gossip: The MUD Chat Network'
date: '2018-07-10 02:16:00'
author: Eric Oestrich
---

Gossip is a new cross game chat network, located at [gossip.haus](https://gossip.haus/). This is a new network using WebSockets as the underlying protocol. Gossip is written in Elixir and MIT licensed, the source code is on [GitHub](https://github.com/oestrich/gossip).

We currently have three games connected to the network, you can view the games that are online [here](https://gossip.haus/games). If you wish to join the network, the documentation for the WebSocket protocol is [available on Gossip](https://gossip.haus/docs).

Right now only cross game channels are supported. The current list of channels to see what is available and also follow along on the web is [here](https://gossip.haus/chat).

If you have an [ExVenture](https://github.com/oestrich/ex_venture) or [Ranvier](https://github.com/shawncplus/ranviermud/) game, support for Gossip is built in already! ExVenture natively supports it and there is a bundle for Ranvier on [GitHub](https://github.com/oestrich/gossip-ranvier).

In the future I would like to add direct messaging and sending remote mail to other users. I am looking for feedback on protocol, [see the docs](https://gossip.haus/docs), if you decide to add Gossip support to your game. You can find me on the [MUD Coders Guild Slack](https://slack.mudcoders.com/).

Finally, you might be asking why a new network?

I created Gossip to be based around the standardized technologies of WebSockets and JSON, and to make sure the transport layer was secured via TLS. I am aware the [I3 network](http://lpmuds.net/intermud.html) exists. I wanted to create something with new standards in technology in mind.

### Sample Connection

Below is a sample of connecting to Gossip, sending, and receiving messages on public channels.

First connect to the WebSocket and send an authenticate event.

    SEND
    {
      "event": "authenticate",
      "payload": {
        "client_id": "4519e9ab-f58f-4585-bcef-fedc389d0ad6",
        "client_secret": "42f00f84-d856-484f-a463-4c8c222f0692",
        "supports": ["channels"],
        "channels": ["gossip"],
        "user_agent": "ExVenture 0.23.0"
      }
    }

After you are authenticated, Gossip will start sending heartbeats. You must respond to these to stay connected to the network.

    RECEIVE
    {
      "event": "heartbeat",
    }
    SEND
    {
      "event": "heartbeat",
      "payload": {
        "players": ["player"]
      }
    }

You can send a new message to a channel you are subscribed to with the “messages/new” event.

    SEND
    {
      "event": "messages/new",
      "payload": {
        "channel": "gossip",
        "name": "Player",
        "message": "Hello everyone!"
      }
    }

When another game broadcasts on a channel you are subscribed to, you will receive a “messages/broadcast” event.

    RECEIVE
    {
      "event": "messages/broadcast",
      "ref": "89036074-446f-41ab-b87a-44ef1f962f2e",
      "payload": {
        "channel": "gossip",
        "message": "Hello everyone!",
        "game": "ExVenture",
        "name": "Player"
      }
    }

There are a few other events that you can do, but this covers almost all of the current interactions with Gossip. See the [docs](https://gossip.haus/docs) to research more.


