# Installation

HASS.Agent's integration isn't integrated in Home Assistant (yet), so you'll need to install it before you can use it. 

#### HACS

The easiest way to do this is by installing [HACS](https://hacs.xyz) (*Home Assistant Community Store*). It's a central hub through which you can install tons of useful integrations, themes, custom cards and scripts. They've written a step-by-step guide on how to do this:

[https://hacs.xyz/docs/setup/prerequisites](https://hacs.xyz/docs/setup/prerequisites/)

Or you can use one of the youtube guides, like this one: [How to install HACS in 2022 in Home Assistant](https://www.youtube.com/watch?v=D6ZlhE-Iv9E)

You can also ask for help in the [HA forums](https://community.home-assistant.io) if you're stuck.

After you've installed HACS (and rebooted HA), you can use this link to jump to the integration (thanks [@c00ldude1oo](https://github.com/c00ldude1oo)):

[![Open your Home Assistant instance and open a repository inside the Home Assistant Community Store.](https://my.home-assistant.io/badges/hacs_repository.svg)](https://my.home-assistant.io/redirect/hacs_repository/?owner=LAB02-Research&repository=HASS.Agent-Integration)

Or if you want to navigate yourself: open HACS by clicking on its icon on the toolbar on the leftside of HA. Then click `Integrations` -> `EXPLORE & DOWNLOAD REPOSITORIES` (bottom right) and search for `HASS.Agent`.

> Note: you ONLY need the `HASS.Agent` integration. The `notification` and `media_player` integrations are legacy, you can ignore them.

#### Manual

Another option is to manually install the integration. The downside of this is that you'll also need to manually update every time. 

Go to the integration's [repository](https://github.com/LAB02-Research/HASS.Agent-Integration). Click on `Code` -> `Download ZIP`:

![image](https://user-images.githubusercontent.com/81011038/201074779-650633aa-f132-4a82-99cb-e692dbd1ce9f.png)

Browse to the `custom_components` folder, and extract the `hass_agent` folder into your HA's `config\custom_components` folder. Restart HA to load it.

#### MQTT

It's highly recommened to have MQTT up and running. This way, you'll have all the latest features. If you don't already have it, follow [this guide](https://www.youtube.com/watch?v=dqTn-Gk4Qeo) to help you get started.
