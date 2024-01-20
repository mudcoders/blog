---
layout: post
title: How to Dockerize EnvyMUD
author: Zachary Flower
date: '2024-01-20 09:51:00'
---
About 13 years ago, I published a post on my blog titled _[How to Compile Envy MUD](https://flower.codes/2011/09/07/how-to-compile-envymud.html)_ (while my [archive](https://flower.codes/archive.html) page has it listed as the _first_ post, it's actually just the oldest one I've retained after years of prior publishing).  While the process of compiling Envy MUD (or EnvyMUD) hasn't changed much in the last decade plus, our ability to actually _launch_ it has improved in some pretty meaningful ways.

That's right, I'm talking [containers](https://www.docker.com/resources/what-container/), people!

I'm a fan of [Docker](https://www.docker.com). It's a simple way to consistently build and deploy applications—and, sometimes more importantly, development environments—but what makes is particularly useful (at least to me) is its ability to spin up a decades old codebase with minimum overhead.

## Enter the EnvyMUD Container

While this post isn't _technically_ a Docker tutorial itself, I should probably touch on some of the basics of getting started. At the root of any Docker container is the Dockerfile, a source file that defines exactly  _how_ a container should be built and run. As an example, here is a Dockerfile that I wrote to run EnvyMUD (using the source code fixes outlined in my 2011 post linked above) and _placed at the root of the project directory_:

```dockerfile
# Use an official gcc runtime as a parent image
FROM gcc:latest

# Set the working directory in the container to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install make
RUN apt-get update && apt-get install -y make bc

# Set the working directory in the container to /app/src
WORKDIR /app/src

# Run make
RUN make

# Expose port 4000
EXPOSE 4000

# Expose the player directory externally
VOLUME /app/player

# Run the program output from the previous step
CMD ["./startupSH"]
```

While it _should_ be pretty self-explanatory (especially with the comments), let's do the proper tutorial thing and break down each directive and explain what they do:

- `FROM gcc:latest` - This is often the very first line in any Dockerfile, and it defines a base container with which to build a new one off of. You can inherit from multiple containers, and there are security concerns that you should always be mindful of when using public containers, but both of those are beyond the scope of this post. For our purposes, we are going to inherit from the latest release of  the [official `gcc` container](https://hub.docker.com/_/gcc), which—as you can guess—comes with the `gcc` compiler installed.
- `WORKDIR /app` - Next, we need to set the working directory in our container. When defined, `WORKDIR` simply indicates what directory you will be in when running any future commands.
- `COPY . /app` - Here's where the actual fun starts coming in. The next thing we are going to do is copy our entire Envy project directory (which is where the Dockerfile has been placed), into our `/app` directory.
- `RUN apt-get update && apt-get install -y make bc` - Next we need to install a few extra dependencies. As you can see, the `gcc` base image is running on top of some sort of Debian-based operating system (indicated by the `apt-get` command. The two things we need to install in addition to what ships with the base image are `make` (to execute our `Makefile` and `bc` to do some basic math in the included `startupSH` script... more on both of these later).
- `WORKDIR /app/src` - Now it's time to start mucking with the code. To do that, we first need to change our working directory to the `./src` directory that we copied into the `/app` directory a few steps above.
- `RUN make` - Once in the proper directory, it's time to compile the code. This is done with a `RUN` directive, which executes a commend from _inside_ the container (which, in this case, is `make`).
- `EXPOSE 4000` - Getting to our finish line, we have to do some exposures. The first one is exposing port 4000, which is what EnvyMUD will listen on (and that you will connect to using your favorite MUD client... or just `telnet`... your choice).
- `VOLUME /app/player` - The next thing we want to expose is the `/app/player` directory, because what happens in the container _stays_ in the container unless you map it to something externally. In the case of both the `EXPOSE` and `VOLUME` directives, we're identifying the exposures, but they won't take effect until we actually _run_ the container. More on that below.
- `CMD ["./startupSH"]` - The last piece of any Dockerfile is the command that gets run. Generally speaking, this is supposed to be a command that _is not run in the background_, so no daemons people. It must be run in the foreground so Docker knows when something happens (like it crashes or shuts down). The `startupSH` command is a script that is shipped with the EnvyMUD source code, and handles things like logging, in-game reboots, and core dumps.

## If You Build It...

Okay, we're done, but _just_ with the Dockerfile. All this does is _define_ our container, it doesn't actually do anything until we build it. To do that, all we have to do is run the `docker build` command:

```
$ docker build -t envy .
```

I want to call out one thing: the `-t` flag. This means to `tag` the build, which creates an easier-to-remember reference for us to use in the next step, which is actually _running_ our container.

## ... It Will Run

So we've got a container build... now what?

_Time to game, right?!_

Not quite. Now we next have to actually run it. I want to call out that the example I'm going to present will be focused on running a container in the foreground. Restarts, crash recovery, backgrounding, etc. are not going to be covered in this particular post (although I plan on writing more about how to _deploy_ a Docker-based MUD in the future).

To run our container, we need to execute the following command:

```
$ docker run -v ./player:/app/player -p 4000:4000 envy
```

As above, let's break down what's actually happening here:

- The `-v` flag identifies a volume. Since we are running our container from the root of our project directory, we will be mapping  the `./player` directory _into_ the aforementioned `/app/player` directory in the container. This will not only preserve the player files (where individual player data is stored) outside of the container (in the event of a crash, or accidental deletion), but will also allow us to _edit_ the player files without having to log into the container itself (which is useful when making yourself an immortal).
- The `-p` flag identifies a port to expose, which in our case is port `4000`. While we can _absolutely_ map an external port to the container's internal port `4000`, for our purposes we are just going to map `4000` directly into the container.

## Time to Test

Alright, so we've written our Dockerfile, built the container, and are now running it.

_Now_ is it time to game?!

Yes. Yes it is.

To connect to our running game, I'm going to use the extremely basic `telnet` command (which may or may not even be installed on your operating system right now). To do that, all we have to do is execute the following command:

```
$ telnet localhost 4000
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.


                   __ ____  ./|        ./\     ____ ____
Based on          / __|_/"`/ /|_______/ / |   '"\_|_  @ \       By:
Envy 2.0        /  /#/      | @/ _@@@@ \ @/          \#\  @ \     The EnvyMud
( see 'help   / @/##/        \|  .\ @ /. >            \##\  @ \       Staff
 envy' &    / @ |###|        | < .!| |!.|              |##\   @ \  ( see 'help
 'help     | @  |###|        |@ \  \ /  /\             |###|   @@|   wizlist' )
 merc' )   |@@  |###_\___    |@| \(o o)/\@\            |###|    @|
           |@@  .\/'/'/'/\__  \ \_\___/_/\@ \ ________/####|  __@|__
           |@@  | \_\_\_\   |\ \@\  |   | |\@ \  |    |###/  /     /
           |@@  |   \)\)\)__|  \\@ \|   | |/\ @@\|    |#/  /     /
           | @  |  |_____|       \@ |   | |/ \ @@|    | \/     /@|  Mud Version
            \ @ |   _____|   |\    \|   |  \/ \ /     |      /  @|   2.0.0
DikuMud       \ |  |_____|__ |  \       |    \/      /      |   @|
was created    \|           ||    \     |          / |      |\ @@|
by:  Han Henrik |___________||  ____\___| \______/___|______|@ \@|
Staerfeldt, Katja    |     \  /     |   | |   |   _     \     @@ \   Kahn,
Nyboe, Tom Madsen,   |      \/      |   | |   |  | |     |     @@ \   Hatchet,
Michael Seifert,     |   |\    /|   |   |_|   |  |_|     |      @@ \   and
and Sebastian Hammer |___| \__/ |___|\_______/|_________/           |   Staff
( see 'help diku' )                            email: envy@uclink3.berkeley.edu
        WWW Home Page: http://www.teleport.com/-morpheus/envy.html
                                               \_tilde
State thy name:
```

And that's it! Enjoy your new/old containerized MUD.