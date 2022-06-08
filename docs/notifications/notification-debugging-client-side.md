# Notification Debugging - Client Side

The info on this page is focussed on making sure notifications work on the client side: HASS.Agent on your PC. Use [Notification Debugging - HA Side](https://hassagent.readthedocs.io/en/latest/notifications/notification-debugging-ha-side/) to debug on Home Assistant's side.

If it still doesn't work after following all below steps, ask for help on [Discord](https://discord.gg/nMvqzwrVBU) or open a [Github Ticket](https://github.com/LAB02-Research/HASS.Agent/issues).

----

#### 1. Firewall Rule and Port Reservation

Your PC needs to be able to accept messages from Home Assistant. Normally, your PC's closed for the 'outside world', so we need to allow add an exception. 

This is done for you during onboarding, however, you can always execute these steps again by going to **Configuration** -> **Notifications** and click `execute port reservation`.

You can check whether HASS.Agent can be reached by going to this URL on another device on the same network (for instance your smartphone, when it's connected to WiFi):

`http://{hass_agent_ip}:5115` 

Make sure to change `{hass_agent_ip}` to the IP of the PC where HASS.Agent's installed. 

If HASS.Agent is configured and the firewall rule's active, you'll see: `HASS.Agent Active`. If not, something is blocking access to HASS.Agent. Perhaps your using a 3rd party firewall (most paid antivirus have them these days). If that's the case, consult its documentation to see how you can allow an exception for port `5115` and/or `HASS.Agent.exe`.

----

#### 2. Windows Notifications Settings

Windows manages which notifications are allowed and when. Usually HASS.Agent is allowed by default. If not, notifications aren't shown, even when you've correctly configured Home Assistant.

For starters, make sure the HASS.Agent icon is shown next to the clock in your taskbar, and not in the overflow message. After you've done this, you can show a test notification by going to **Configuration** -> **Notifications** and click `show test notification`.

Did the notification popup? Great! If not, go to Windows' (new) configuration page and make sure you've enabled `Notification access` or `Notifications`, depending on your Windows version:

![image](https://user-images.githubusercontent.com/81011038/160234660-4cf9875b-7961-4e52-882b-2e92cf14bf0a.png)

![image](https://user-images.githubusercontent.com/81011038/160234677-2ec285b7-2236-48a0-808c-1744c7940453.png)

On that same page, make sure HASS.Agent has its checkbox set to `On`:

![image](https://user-images.githubusercontent.com/81011038/160234705-37d0a079-b5e5-472a-9809-8629d5cf0b46.png)

Optionally, click on HASS.Agent instead of the checkbox, and make sure its settings are as follows:

![image](https://user-images.githubusercontent.com/81011038/160234724-db2f6336-d6d2-46c0-9856-52438a115ed6.png)

Depending on your Windows version, you may also need to make sure that the `Action Center` system icon is enabled:

![image](https://user-images.githubusercontent.com/81011038/160234962-8a1df96d-9aef-4771-a686-2b1b2b04bb87.png)

When you've done all this, try the `show test notification` button again in HASS.Agent. 

Did the notification show? Great, you can now finish setting up in Home Assistant. If not, continue below.

----

#### 3. Obscure Windows configurations

There have been a few reports of users which had to change weird Windows settings before it worked. For starters, if you're using privacy tools like [o&o shutup](https://www.oo-software.com/en/shutup10) or [decrapifier](https://github.com/n1snt/Windows-Decrapifier) (great tools by the way!), make sure they're not set to block notifications.

Sometimes, when telemetry is disabled, it also disables notifications. You can temporarily reenable it with the following Powershell command:

```powershell
Remove-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\DataCollection" -Name "AllowTelemetry"
```

When everything's working, feel free to disable telemetry once again.

----

#### 4. External links

The following links may (or may not) help in further debugging:

https://www.surfacetablethelp.com/2017/02/fix-show-app-notifications-option-greyed-out-in-windows-10-settings.html
https://www.guidingtech.com/top-ways-to-fix-notifications-not-working-on-windows-11/
