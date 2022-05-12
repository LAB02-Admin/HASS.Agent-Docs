# Introduction

This page explains the basics behind HASS.Agent: what is it and what can it do. We'll be using the abbreviation **HA** for Home Assistant, so when HA is mentioned, it refers to your Home Assistant instance.

Tip: you can use Home Assistant's [glossary](https://www.home-assistant.io/docs/glossary/) to look up words you don't know yet.

> Note: this document includes features that are available starting with [2022.12.0-beta2](https://github.com/LAB02-Research/HASS.Agent/releases/tag/2022.12.0-beta2), and will be publicly available with 2022.12.0.

### Overview

In general, HASS.Agent is a client (*companion*) application for Home Assistant. It's being developed for Windows, with Windows 10 and 11 in mind - previous versions *may* work. 

The communication goes both ways: HASS.Agent can send data to HA, and HA can send data to HASS.Agent. In other words: you can use HASS.Agent to control and inform HA, and you can use HA to control HASS.Agent.

Globally, these are the functionalities:

| Function | Description |
| ------------ | ------------- |
| Commands | Control your PC through HASS.Agent from HA. Example: lock your session, open notepad or emulate a keypress. |
| Sensors | Send PC telemetry to HA. Example: cpu load, is your webcam active or is the current user active. |
| Quick Actions | Trigger any HA entity. Example: turn on your lights or open the garage door. |
| Notifications | Receive a notification from HA, and show it as a Windows notification. Example: image of who's at the front door. |
| Media Player | Control your PC from HA as if it were a mediaplayer. Example: see what Spotify song's playing, or send a text-to-speech message. |
| WebView | Show any webpage, without launching a browser. Example: show a HA dashboard when rightclicking HASS.Agent's tray icon. |
| Satellite Service | Process commands and sensors even when no user's logged in. Example: shutdown your PC at any time, locked or not. |
