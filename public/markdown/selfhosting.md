# Selfhosting FredBoat

* * *

**Disclaimer**: This guide is a highly community-driven effort, and as such is always subject to change -- sometimes breaking. If you have carefully read through this guide and find trouble, check the #selfhosters channel in the [FredBoat Hangout](https://discord.gg/cgPFW4q) server and someone from the community may be able to help. As always, pull requests are welcome [here]( https://github.com/FredBoat/fredboat.com/blob/master/public/markdown/selfhosting.md). A best effort is made to keep this guide up-to-date, but there are no specific guarantees.

* * *

This is a tutorial for users who would like to host their own FredBoat instance. Please bear in mind that this is not a requirement for using the FredBoat service as there is a [public FredBoat♪♪](https://goo.gl/cFs5M9) hosted and provided that is simple to use.

**This tutorial is for advanced users. If you can't figure out how to run this, please use the [public FredBoat♪♪](https://goo.gl/cFs5M9)**

As the first part of word *self*hosting implies, this is all about doing things your*self*. If you love tinkering around with things and can use your preferred search engine to resolve 95% of the issues you run into by your*self*, then this guide is definitely for you and we welcome you to join us in the #selfhosters channel on [FredBoat Hangout](https://discord.gg/cgPFW4q) for those last tricky 5% of questions.


Having that out of the way, let's dive right in!


## General notes

FredBoat runs preferably on an x86_64/AMD64 CPU. Strange things may happen if you try running FredBoat on an ARM CPU like on a Raspberry Pi. If you have no idea what this means, you're probably doing it right and don't have to worry.

FredBoat requires a PostgreSQL database to run. The easiest way to achieve that is by using the docker part of this tutorial.

Whenever editing any files, use a proper text editor (like [Sublime](http://www.sublimetext.com/)) and keep the rules for YAML files in mind: never use tabs, always spaces, indentation is important.


## Selfhost with Docker and Docker Compose

**Why is this the recommended way to selfhost FredBoat?**  
- Docker works on almost any architecture and platform out there in the same way - be it Windows, Mac OS, tons of Linux distros or even your spare Raspberry Pi.
- You don't have to install Java or any build tools like Git or Gradle, and copy paste obscure commands to have them spit out a jar, because our [CI server](https://ci.fredboat.com) builds the jar for you and publishes an official FredBoat docker image to the [official docker hub](https://hub.docker.com/r/fredboat/fredboat/).
- **FredBoat has grown over the years.** Although running the bot with a single jar was possible in the past, it has become increasingly more complex as FredBoat requires a backend database (like PostgreSQL), and many parts  of the bot are separated into other modules, such as [the backend](https://github.com/FredBoat/Backend). FredBoat's structure changes over time play an important role for a bot of this magnitude. However, Docker (with [Docker Compose](https://docs.docker.com/compose/install/)) makes this process fairly straightforward. It is important to note that reading the instructions is more important than ever, as syntax and other small portions of the configuration may affect the bot in many ways.
- FredBoat is constantly evolving; there are many parts of the project that are modular. By using Docker, you can ensure you are *much* better off when it comes to future breaking changes, which any selfhoster should expect to encounter once in a while.
- You can easily configure automated updates using an additional, optional 3rd party docker image called [Watchtower](https://github.com/v2tec/watchtower).
- All containers will automatically be started when the machine you are using to host FredBoat reboots / starts.


### Requirements

1. [Docker Community Edition](https://docs.docker.com/engine/installation/)

2. [Docker Compose](https://docs.docker.com/compose/install/)

3. A folder containing all of [these template files](https://github.com/Frederikam/FredBoat/tree/master/config/templates) with the exception of `docker-compose.yml`


Should look like this:

[![Folder with files](https://fred.moe/aKF.png)](https://fred.moe/aKF.png)

You may want to read these configuration files, as they contain a few advanced options to run your FredBoat.

- The `common.yml` is shared by multiple applications, and is where you must put your Discord bot token. This is the only file that is always required to edit.

- The `selfhosting.yml` explains some Windows platform specific configuration for the database, how to configure FredBoat builds, and how to enable automatic updates, as well as the setup for fetching the proper backend.

- The `fredboat.yml` file holds the master setup for FredBoat and tells you what tokens and codes and passwords are needed, which are optional, and where to get them, shows the settings to configure the backend database connection, logging options, and various important setup criteria. This single file replaces the old `credentials.yml` and `config.yml` files.

- The `quarterdeck.yml` file holds the configuration options for the backend (Quarterdeck) database. The basic credentials setup here are used in `fredboat.yml` and must match.

Got all those things together? Continue reading below to learn how to set up config and credentials files, then proceed with the instructions on how to run FredBoat with docker-compose.


### Run FredBoat with docker-compose

You might need to run these commands with `sudo` depending on your docker setup.

Navigate to the directory where you placed the files after filling them out:
```sh
cd /path/to/my/Download/folder/containing/FredBoat
```

Rename the `selfhosting.yml` to `docker-compose.yml`:

```sh
mv selfhosting.yml docker-compose.yml
```

Start everything with:
```sh
docker-compose up -d
```
_Please note: During the first start, the database container requires additional time to be fully set up and, depending on the specs of your machine as well as whether your latest sacrifices have appeased the Docker gods, might not be ready immediately, resulting in the FredBoat container restarting a few times or even exiting. Run the above command a (few) additional times in that case_

You can see running docker containers on your machine with:
```sh
docker ps
```
This will also show stopped containers:
```sh
docker ps -a
```
FredBoat will put its log files into a folder called `logs` so you can access them to see if anything is wrong.

Should you want to follow the logs realtime for troubleshooting purposes, you can use:
```sh
docker-compose logs -f
```

To stop everything, do this from inside the directory where you placed the FredBoat files and ran the initial docker-compose command:
```sh
docker-compose stop
```

Congratulations! Your FredBoat should be running now. If you haven't invited your bot to your guild yet, [visit this tutorial](https://github.com/reactiflux/discord-irc/wiki/Creating-a-discord-bot-&-getting-a-token) to find out how to do it.

## Bot Administration
As a selfhoster, you and your configured bot admins have access to additional administrative commands, which you can find through
```
;;commands admin
```
