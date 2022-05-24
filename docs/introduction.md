# Introduction

This page explains the basics behind HASS.Agent: what is it and what can it do. We'll be using the abbreviation **HA** for Home Assistant, so when HA is mentioned, it refers to your Home Assistant instance.

Tip: you can use Home Assistant's [glossary](https://www.home-assistant.io/docs/glossary/) to look up words you don't know yet.

> Note: this document may include features that are only available in the latest beta. Check the [release page](https://github.com/LAB02-Research/HASS.Agent/releases) for a list of all releases.

----

### Overview

In general, HASS.Agent is a client (*companion*) application for Home Assistant. It's being developed for Windows, with Windows 10 and 11 in mind - previous versions *may* work. 

The communication goes both ways: HASS.Agent can send data to HA, and HA can send data to HASS.Agent. In other words: you can use HASS.Agent to control and inform HA, and you can use HA to control HASS.Agent.

Globally, these are the functionalities:

| Function | Description |
| ------------ | ------------- |
| [Commands](#commands) | Control your PC from HA. Example: lock your session, open notepad or emulate a keypress. |
| [Sensors](#sensors) | Send PC telemetry to HA. Example: cpu load, is your webcam active or is the current user active. |
| [Quick Actions](#quick-actions) | Trigger any HA entity. Example: turn on your lights or open the garage door. |
| [Notifications](#notifications) | Receive a notification from HA, and show it as a Windows notification. Example: image of who's at the front door. |
| [Media Player](#media-player) | Control your PC from HA as if it were a mediaplayer. Example: see what Spotify song's playing, or send a text-to-speech message. |
| [WebView](#webview) | Show any webpage, without launching a browser. Example: show a HA dashboard when rightclicking HASS.Agent's tray icon. |
| Satellite Service | Process commands and sensors even when no user's logged in. Example: shutdown your PC at any time, locked or not. |

Let's go over every function for a bit more in-depth.

----

### Commands

Commands have their own introduction page: [Command Basics](commands/command-basics.md).

If you want to go a bit more advanced, have a look at the actions page: [Actions Usage and Examples](commands/actions-usage-and-examples.md). Actions allow you to send variables alongside your command, so you can for example have you HA automation decide what URL you want to show.

----

### Sensors

Sensors allow you to send all sorts of telemetry from your PC to HA. A few common examples are *what's the current cpu load*, *are you using your webcam/mic*, *are you logged in* or *is your session locked* - but there are over 30 of them, so there's much more to explore.

When you create a new sensor, it shows up in HA as an entity:

![image](https://user-images.githubusercontent.com/81011038/170042142-705937df-f0ad-4808-aee4-7a1e87ec0aab.png)

To find the entities, go to your HA instance, and press `c`. This will show the search bar. Now type `devices` and press **enter**. In the `Search devices` bar that's shown on top, type the name of your HASS.Agent instance:

![image](https://user-images.githubusercontent.com/81011038/167813646-22cd747f-9094-42d4-b48b-2b17766d582f.png)

> Tip: you can always find (and change) the name of your HASS.Agent instance by clicking on the `configuration` button in the main window.

If your HASS.Agent is properly configured, your pc will now show as a result. You can easily verify it's an HASS.Agent instance by confirming it says `LAB02 Research` under the `Manufacturer` column.

Click on your pc to see what commands and sensors are currently configured.

#### Multi-Value Sensors

Some sensors have multiple relevant data points. For instance, if you add a storage sensor, statistics for all your disks will get sent. You can easily see which sensors are multi-value through this column:

![image](https://user-images.githubusercontent.com/81011038/170042833-dcfe8f90-d972-4758-92bc-fdb6beaa8e47.png)

And here you can see the multiple sensors being added (they all start with the name you gave it):

![image](https://user-images.githubusercontent.com/81011038/170043145-d7b28d68-be3b-4009-8600-20956be7c62a.png)

----

### Quick Actions

One of the reasons for developing HASS.Agent was the ability to easily trigger HA entities. For instance, if I want to turn on a light, I don't want to open an app or browser and go to the right dashboard. Nor do I always want to say it out load to a Google Home-device of sorts. To make that possible, HASS.Agent has Quick Actions. 

You configure them once by selected the desired entity and the desired state:

![New Quick Actions](https://raw.githubusercontent.com/LAB02-Research/HASS.Agent/main/images/hass_agent_new_quickaction.png)

And from that on you can either execute them by their individual hotkey, or pull up the list of quick actions by their global hotkey:

![Quick Actions](https://raw.githubusercontent.com/LAB02-Research/HASS.Agent/main/images/hass_agent_quickactions.png)

----

### Notifications

Another reason for HASS.Agent's creation was the ability to receive notifications from HA. For instance: someone presses the doorbell, and you get a notification with a picture of the guest on your PC. Because I'm usually listening to music with earpods in while working, this would be a good way to get my attention.

There's a [dedicated notifications page](https://hassagent.readthedocs.io/en/latest/notifications/notification-usage-and-examples/) to help you set things up (with debugging manuals in the left menu), and you can also check its [GitHub readme](https://github.com/LAB02-Research/HASS.Agent-Notifier) for more info.

----

### Media Player

The mediaplayer integration allows you to use your HASS.Agent as a mediaplayer device: see and control what's playing (regardless of what application you're actually using to play media), and send text-to-speech notifications. 

There's a [dedicated mediaplayer page](https://hassagent.readthedocs.io/en/latest/mediaplayer/mediaplayer-usage-and-examples/) to help you set things up, and you can also check its [GitHub readme](https://github.com/LAB02-Research/HASS.Agent-MediaPlayer) for more info.

----

### WebView

The webview capability is not something you can see directly like commands and sensors, but more of an underlying technique. It's basically a way to show a website, without having to launch a browser.

There are currently two implementations for the webview component.

#### WebView Command

This command allows you to open any webpage with some extra configuration, like where on your monitor, and what size. You can use this to launch a predefined URL (like a HA dashboard) or even dynamically when using an Action.

Example configuration:

![image](https://user-images.githubusercontent.com/81011038/170069096-3be32f83-1d4b-44e1-9242-91f780673e7f.png)

#### Tray Icon

By default, when you right-click the HASS.Agent tray icon (near your clock on the taskbar) you'll get a simple menu. However, you can also configure (through `Configuration` -> `Tray Icon`) to show a webview. This is really neat if you want to quickly show a dashboard with a bit more options than the quick actions:

![image](https://user-images.githubusercontent.com/81011038/170071165-b2f0fc28-9112-4186-963d-7df213008035.png)

----


