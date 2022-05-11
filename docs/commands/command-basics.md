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

> Note: don't be alarmed if the **system status** for `commands` says `stopped`: that's just because you probably don't have any commands configured yet.

You'll be shown a (possibly empty) command list:

![image](https://user-images.githubusercontent.com/81011038/167810115-b485f632-8327-4cf7-be2d-340581b741ed.png)

Click the `add new` button to begin creating a new command.

> Tip: want to modify an existing command? Just doubleclick it!

You can see the different command types on the left. Select `Lock` to begin configuring your lock command:

![image](https://user-images.githubusercontent.com/81011038/167811252-2eacc702-763d-43a9-8dca-2fcd37cc30fd.png)

We'll pick `Button` as our entity type. HASS.Agent will try to generate a name for you, in this case `test-vm_lock`, but feel free to change that to anything you like. 

> Note: the names need to be unique! That's why it's a good idea to start with the name of your pc. That way, if you install HASS.Agent on multiple pcs, they won't clash.
