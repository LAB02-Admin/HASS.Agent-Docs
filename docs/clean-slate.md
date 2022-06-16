# Clean Slate

This manual shows you how to clean up HASS.Agent in both Home Assistant (*HA*) and locally. Use this whenever you want to start over, or something goes wrong with a sensor or command.

#### Home Assistant

HASS.Agent should keep everything tidy, but sometimes, old devices or entities get orphaned in HA. If you're like me, you want to clean them up. Other reasons can be that a sensor or command got messed up, tangled up because of duplicate device names, or you simply want to start over.

> When you delete your device in HA, it won't mess with your automations and scripts. Simply have HASS.Agent re-register (which is as easy as restarting it) and everything's up and running!

Before you get started, make sure you completely close HASS.Agent by chosing `exit` in the exit dialog. Otherwise it'll keep re-registering.

Afterwards, open HA, press `c`, type `devices` and press enter. This'll bring you to the devices list. Now type the name of the device you want to clean, and click it to open:

![image](https://user-images.githubusercontent.com/81011038/174077501-afd7458e-9579-4f30-9e1a-8596c4a31d43.png)

Check that you have a HASS.Agent device open, by looking at the `Device info` - it should say `LAB02 Research`:

![image](https://user-images.githubusercontent.com/81011038/174077693-8e55217b-88fa-431a-8fc3-72c62827d5b1.png)

Now click the hamburger menu (the three vertical dots) and click `Delete device`:

![image](https://user-images.githubusercontent.com/81011038/174077836-6b79412c-7273-4f05-bc1f-05bfb93ccaa2.png)

You'll get a popup asking you to confirm, simply click `OK`. HA will show you a `Device / service not found.` page:

![image](https://user-images.githubusercontent.com/81011038/174077930-07496805-8e97-49c6-a5c7-9572418bc3c3.png)

This means all went well, and you can go back to the devices list by clicking `GO BACK`.

At this point HA has been cleaned up. You can repeat the process for other HASS.Agent (orphaned) devices. When you're satisfied, you can start HASS.Agent. Your device will get registered again, and all your stored sensors and commands are added.

#### Local

If you want to remove HASS.Agent, simply do so through Windows' configuration screen (the exact place depends on your Windows version):

![image](https://user-images.githubusercontent.com/81011038/174079239-0042e932-ff03-4905-a4af-6738c3195b98.png)

This **wil not delete** your settings, sensors and commands. So if you want to downgrade, at this point you can install the version you want and be done (note that older versions might not support your config).

If you want to completely clean up, or backup your settings, open explorer and browse here: `%APPDATA%\LAB02 Research\HASS.Agent`

To perform a backup, simply copy the `config` folder. If you want to clean up, completely remove this `HASS.Agent` folder.

Afterwards go here: `%PROGRAMFILES(X86)%\LAB02 Research\HASS.Agent Satellite Service`

Same story, to backup, copy the `config` folder. To clean up, completely remove this `HASS.Agent Satellite Service` folder.
