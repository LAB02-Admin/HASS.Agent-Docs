# Command Basics

This page contains the basics behind HASS.Agent's commands: what they are, how they function and how you can use them. We'll be using the abbreviation HA for Home Assistant, so when HA is mentioned, it refers to your Home Assistant instance.

Tip: you can use Home Assistant's [glossary](https://www.home-assistant.io/docs/glossary/) to look up words you don't know yet.

### Intro

From within HASS.Agent you can create `commands`. These are [entities](https://www.home-assistant.io/docs/glossary/#entity) that will be automatically added to HA. You can use these commands to control your PC from within HA.

> Tip: if you want to get data from your PC to HA, you can use HASS.Agent's sensors.

Commands can be different entity types:

* Button
* Light
* Lock
* Siren
* Switch

Your command is always handled the same way by HASS.Agent, regardless of the type your selection. The type is only relevant for HA: it determines as what type it'll show up. But they all have the same functionality.

### Creation

Throughout this page, we'll be working with an example command: the `Lock` command. This can be easily tested (your session will get locked, hard to miss!) without accidentally shutting down your pc or something like that.

First, open HASS.Agent's main window, and click the `commands` button:

![image](https://user-images.githubusercontent.com/81011038/167809618-5c160b82-67f6-433a-82a3-4374b7c78292.png)

> Note: don't be alarmed if the `system status` for `commands` says `stopped`: that's just because you probably don't have any commands configured yet.

