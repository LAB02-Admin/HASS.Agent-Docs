# Installation

Click [here](https://github.com/LAB02-Research/HASS.Agent/releases/latest/download/HASS.Agent.Installer.exe) to download the latest installer.

If you want to install HASS.Agent on multiple accounts on the same PC, first install using the installer, then use the .zip package for the remaining accounts: click [here](https://github.com/LAB02-Research/HASS.Agent/releases/latest/download/HASS.Agent.zip) to get it. Note: you only have to install HASS.Agent on those accounts, the service is once-per-machine.


***

#### 1. Installer (recommended)

You have the option to download an installer or a .zip package. The installer is the easier and recommended option: it'll help you install .NET 6 if you don't have it yet, install the service and launch HASS.Agent. However, because the installer requires elevated rights, you can use the .zip package to install HASS.Agent on other accounts on the same PC (since the service only needs to be installed once).

HASS.Agent can use an entry in your user account's registry to launch on login. It's disabled by default, but you'll be offered to enable this during onboarding, or you can always disable/enable using the Configuration window.

To use notifications, you'll need to install the [HASS.Agent Notifier integration](https://github.com/LAB02-Research/HASS.Agent-Notifier). This can be done through [HACS](https://hacs.xyz) or manually. 

You'll also need to open the configured port in the firewall of the receiving PC (default `5115`). During the onboarding process (or when using the Configuration window), HASS.Agent will offer to do it for you. You can always execute these steps later on through the configuration window:

![Configuration screen](https://raw.githubusercontent.com/LAB02-Research/HASS.Agent/main/images/hass_agent_notifications_portreservation.png)

***

#### 2. Manual

If you don't want to (or can't) use the installer, you can also install manually - it's not that hard. 

Make sure you have dotnet 6 desktop runtime installed, you can [get that here](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-6.0.4-windows-x64-installer).

There are two zip files to download with every release:

Click [here](https://github.com/LAB02-Research/HASS.Agent/releases/latest/download/HASS.Agent.zip) to download the latest HASS.Agent package.

Click [here](https://github.com/LAB02-Research/HASS.Agent/releases/latest/download/HASS.Agent.Satellite.Service.zip) to download the latest Satellite Service package.

HASS.Agent needs to be placed in a folder with write access. The default is `%appdata%\LAB02 Research\HASS.Agent`, but anywhere will do.

The Satellite Service's default folder is `C:\Program Files (x86)\LAB02 Research\HASS.Agent Satellite Service`. You can place it anywhere you want, however, if that's the case you need to manually start `HASS.Agent.Satellite.Service.exe`, wait for it to start, then close it again. This way, it'll have configured a local registry setting telling HASS.Agent where it's stored.

When both are done, you can start the configuration process by opening an **elevated** command prompt in HASS.Agent's directory. Then, execute the following command:

`HASS.Agent.exe update`

HASS.Agent will now launch, install and configure the service and firewall rules, then relaunch itself without elevation.

All done!

***

#### 3. Build From Scratch

If you want, you can also build everything yourself. You'll need to have Visual Studio 2022 installed. Also, make sure you have dotnet 6 desktop runtime installed, you can [get that here](https://dotnet.microsoft.com/en-us/download/dotnet/thank-you/runtime-desktop-6.0.4-windows-x64-installer).

There are two main projects:

[HASS.Agent](https://github.com/LAB02-Research/HASS.Agent/tree/main/src)

[HASS.Agent Satellite Service](https://github.com/LAB02-Research/HASS.Agent.Satellite.Service/tree/main/src)

Just get the source through git or GitHub's zip download, launch the solution files, make sure you set `x64` as its output type and build.

If you want to control the entire process: both projects use `HASS.Agent.Shared`, a library which contains models and code mainly for the commands and sensors. It's added as a [nuget](https://www.nuget.org/packages/HASS.Agent.Shared).

You can download its source here:

[HASS.Agent.Shared](https://github.com/LAB02-Research/HASS.Agent.Shared)

Just like before, make sure you set `x64` as its output type and build. Then, remove the nuget reference from both projects, add your own library and rebuild them.

When all this is done, you can follow step 2 'manual' on this page to resume installation.


