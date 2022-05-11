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

If you like, you can click through the command types on the left, and read their decriptions. This way you'll get a feeling for what's possible. 

> Note: missing a command? Open a [ticket on GitHub](https://github.com/LAB02-Research/HASS.Agent/issues) with your idea!

When you're ready, click the `store command` button to continue. 

At this point, your command isn't yet sent to HA, and it isn't even stored locally on HASS.Agent. So if you made a naming or type mistake, just doubleclick your command to edit.

Your commands config window will now have at least one command:

![image](https://user-images.githubusercontent.com/81011038/167812415-711a2ee2-65ed-4635-98f7-ecae1e19c621.png)

The columns on the right indicate `action` and `low integrity`, those are a bit more advanced and aren't relevant now.

Satisfied with your new command? Go ahead and click `store and activate commands`. 

Your command is now sent to HA, and stored locally. Awesome!

### Usage

Let's look up our new command in HA. There are multiple ways to do this, but for now we'll look up our pc (an easy way to see all sensors/commands).

Go to your HA instance, and press `c`. This will show the search bar. Now type `devices` and press **enter**. In the `Search devices` bar that's shown on top, type the name of your HASS.Agent instance:

![image](https://user-images.githubusercontent.com/81011038/167813646-22cd747f-9094-42d4-b48b-2b17766d582f.png)

> Tip: you can always find (and change) the name of your HASS.Agent instance by clicking on the `configuration` button in the main window.

If your HASS.Agent is properly configured, your pc will now show as a result. You can easily verify it's an HASS.Agent instance by confirming it says `LAB02 Research` under the `Manufacturer` column.

Click on your pc to see what commands and sensors are configured. It should at least show our new command:

![image](https://user-images.githubusercontent.com/81011038/167814285-24eaf895-c182-4c85-99db-1baf07943efc.png)

> Note: if you can't find your device, or can't find your command, something's off in your HASS.Agent configuration. Feel free to post your problem on [the HA forums](https://community.home-assistant.io/t/hass-agent-windows-client-to-receive-notifications-use-commands-sensors-quick-actions-and-more/369094). The more info you can provide, the better, but if you don't know how don't worry - we'll help.

