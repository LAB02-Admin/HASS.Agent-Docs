# Introduction

Most of the tricks HASS.Agent does is direct communication between it and Home Assistant: HASS.Agent sends the value of a sensor to HA, HA sends the request to execute a command to HASS.Agent, etc.

That all works great; however, some parts require more extensive integration into HA. And that's where HASS.Agent's integration comes into play. It basically extends HA's functionality.

Currently, the integration adds these two parts:

**Notifications**

The ability to send notifications from within HA to any HASS.Agent. This'll then show up as a toast notification in Windows. You can add images, or actionable buttons if you want. 

**Media Player**

Control your PC like it's a media_player entity from within HA. See what's playing, skip tracks, even send text-to-speech to your PC.

You'll need to install the integration in HA to unlock these functions. Consult the `Installation` page on the left for info on how to do this.
