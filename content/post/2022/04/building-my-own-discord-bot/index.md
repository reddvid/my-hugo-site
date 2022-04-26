---
title: "Building My Own Discord Bot"
description: "Learning NodeJS and JavaScript. Might as well build my own Discord bot"
date: 2022-04-27T02:03:51+08:00
featuredimage: "alfred-bot.png"
ogimage: "alfred-bot.png"
hidden: false
comments: true
draft: false
tags:
  [
    "Discord",
    "DiscordJS",
    "NodeJS",
    "JavaScript",
    "Discord Bot",
    "VS Code",
    "Coding",
  ]
categories: ["Coding"]
---

A new coding adventure begins! I always wanted to get deep with JavaScript specifically NodeJS to build apps and I never got into it due to confusion into what it is, what it can do, what UI framework to use. Though in this phase of learning NodeJS, I skipped building the UI since the bot is all code.

Let’s get into it!

## Intro

Why build a bot from scratch? Most free Discord bots out there does a wide-range of tasks from automessaging, ping/share Twitch links, play music, XP level system, but the features are somehow pay-to-use and managing more than 2 bots is somehow distracting for my small server.

I also was encouraged after building a very simple NodeJS app that changes my custom status on Discord with a single button using Flutter and Discord user token.

## Building with DiscordJS

Before doing the actual code, I got overwhelmed with more coding jargons like OAuth, tokens, intents, etc. - since I haven’t really used them in my personal projects. And ever since coding with C# and/or Flutter/Dart, building apps with their respective documentation, examples, and _playground_ makes it easier, which then means that getting started with [DiscordJS](https://discord.js.org/) somewhat confused me at first.

### Create a Bot and Get It’s Token

This should be fairly easy to get, head over to [Discord Developer Portal](https://discord.com/developers/applications) and build an application/bot. Note: The tutorials online might be a little outdated from what the settings.

### Install NodeJS

This should be included in Windows installation really but they have [guide online](https://nodejs.org/). Be careful, installing node may produce errors on terminal due to a bug in setting environment path.

### Use VSCode

Please.

### Bot Capabilities

As of this writing, here are a few capabilities I put on my Discord bot:

- Welcome message to new members
- Farewell message to tell who left
- Twitch Ping - sends a message when I go live
- YouTube Ping - sends a message when I post a new video on my gaming channel
- Roles selection via emojis
- Few slash commands
- XP and Level system

## Heroku Deployment

Running the app locally works but it’s not good practice so I once learned a new platform/service with [Heroku](https://www.heroku.com/), it hosts and deploys NodeJS apps for free which allows my bot to run online.

## Codes?

I will share the codes/tutorial soon but this post will just serve as an announcement or invitation to share what I have been doing lately.

## Try It Out

Shamelessly, you can join our Discord community, it now houses people who watches my gaming streams, likes my coding, and just a place to hangout for friends and family.

[**Join our Discord Community, Bahay ni Kuya**](https://discord.gg/5dNqkjcTxZ)
