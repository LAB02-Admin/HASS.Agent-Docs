# Configuration

When you first launch HASS.Agent, you'll be taken to the onboarding process. This will guide you through configuring step-by-step, and HASS.Agent will offer to perform some tasks for you. 

If you don't want this, or if you want to change something later on, you can use the configuration screen: 

![image](https://user-images.githubusercontent.com/81011038/198238942-23365c19-5cfa-46dd-8663-f939364cb943.png)

Configuring HASS.Agent is split into different pages:

#### 1. General

Miscellaneous HASS.Agent related settings. 

The `device name` is how your PC advertises itself in Home Assistant. When you change it, all your sensors and command will be removed from Home Assistant, and then reregistered under the new name. Don't worry though, all your automations and scripts will keep working as their IDs don't change.

#### 2. External Tools

Some commands have the ability to invoke external tools; other applications installed on your PC. You can define them here. 

When you don't configure a browser, HASS.Agent will use your default one. If you want, you can specify your own here. As an added bonus, you can set it to launch incognito (HASS.Agent will recognize most browsers for you, and set the required arguments). 

The `custom executor` can be used if you run a lot of Python scripts for instance. That way, you don't have to link the Python binary for every command you configure. Neat!

#### 3. Home Assistant API config

To use quick actions, you have to configure your instance's API. Usually the default URI should work, unless you've changed the port or mdns name. You can get a long-lived API token following [this doc](https://www.home-assistant.io/docs/authentication/).

#### 4. HotKey

This is optional, and can be used to pull up the Quick Actions window at any time.

#### 5. Local API

The local API is an alternative for receiving notifications and using the mediaplayer functionality, if you don't use MQTT. It requires a port reservation and a firewall rule, so if you enable it, make sure to click the `Execute Port Reservation` button afterwards.

#### 6. Local Storage

Manage how local storage is handled. By default HASS.Agent will clear cache older than 7 days.

#### 7. Logging

If you encounter problems while using HASS.Agent, please activate the `enable extended logging` switch. HASS.Agent will restart when you store the configuration, and afterwards try to recreate your problem. Then open this tab again and click `open logs folder`. 

Please attach the latest log when submitting a ticket, or you can mail it to lab02research@outlook.com if you want it handled confidentially (or just because it's easy).

Remember to turn extended logging off afterwards, as it'll clog your disk!

#### 8. Media Player

Make sure the integration has been installed and configured in Home Assistant. 

Requires a patched Windows 10 or Windows 11 to function, *ymmv*. If it appears broken, checks the logs for more info.

#### 9. MQTT

Enter your MQTT broker configuration. This is required if you want to use commands (triggered from Home Assistant) or sensors (sent from your PC). The [new integration](https://github.com/LAB02-Research/HASS.Agent-Integration) also uses MQTT by default, but can use the local API as a fallback.

#### 10. Notifications

Make sure the integration has been installed and configured in Home Assistant.

Sometimes Windows won't allow you to enable notifications for HASS.Agent. This can happen if HASS.Agent hasn't shown a notification yet. To make sure it technically works, click the `show test notification` button in this tab. You'll be presented with a locally generated notification (or not, of course).

#### 11. Satellite Service

This tab provides you with options to manage the technical aspect of the satellite service, like stop/start/disable/reinstall. Usually you won't have to use this. 

If the satellite service doesn't work as expected, please use the `open service logs folder` to locate its logs, and attach the latest one when sending a bug report.

#### 12. Startup

Gives the option to enable/disable running HASS.Agent when you login. 

#### 13. Tray Icon

Allows configuring the (right)mouse click events of HASS.Agent's tray icon.

#### 14. Updates

If you want, HASS.Agent can check for updates in the background. This works by checking the latest release on GitHub. When a new release is found, you'll be notified and given the option to install the update.

You can opt-in on beta updates here. They are generally quite stable, and are mostly used to release new features faster than the regular updates. The regular channel is used roughly once a month, the prevent update fatigue.

To make updating easier, you can enable `offer to download and launch the installer for me`. When enabled, you still have the last say in whether an update will be installed, but you won't have to do anything else. The update is downloaded from github.com, and its cryptographic signature is checked before being executed.
