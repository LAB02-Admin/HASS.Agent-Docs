# Notification Debugging

#### Introduction

The info on this page is focussed on making sure notifications work on the client side: HASS.Agent on your PC. HASS.Agent uses Windows' toast notifications system, which is neatly integrated but sometimes refuses to show them. If everything's working on Home Assistant's side (no errors in the logs), but your notifications still aren't showing, chances are there's some setting in Windows that's blocking them.

Since release 2022.14.0, HASS.Agent attempts to determine whether Windows allows its notifications to be shown. If it suspects that it's blocked, it'll add the following line to the logs:

`[NOTIFIER] Showing notifications might fail, reason: ...`

So if your notification isn't showing, check the logs first (for info on how to do that, check the [integration debugging docs](https://hassagent.readthedocs.io/en/latest/integration/debugging/#hassagent), specifically the HASS.Agent section).

----

#### Windows Notifications Settings

Windows manages which notifications are allowed and when. Usually HASS.Agent is allowed by default. If not, notifications aren't shown, even when you've correctly configured Home Assistant.

For starters, make sure the HASS.Agent icon is shown next to the clock in your taskbar, and not in the overflow (with the hidden icons). After you've done this, try showing a test notification:

![image](https://user-images.githubusercontent.com/81011038/203734681-2996439d-edc4-404e-a4e5-74ba4c9038d5.png)

Did the notification popup? Great! If not, go to Windows' (new) configuration page and make sure you've enabled `Notification access` or `Notifications`, depending on your Windows version:

![image](https://user-images.githubusercontent.com/81011038/160234660-4cf9875b-7961-4e52-882b-2e92cf14bf0a.png)

![image](https://user-images.githubusercontent.com/81011038/160234677-2ec285b7-2236-48a0-808c-1744c7940453.png)

On that same page, make sure HASS.Agent has its checkbox set to `On`:

![image](https://user-images.githubusercontent.com/81011038/160234705-37d0a079-b5e5-472a-9809-8629d5cf0b46.png)

Optionally, click on HASS.Agent instead of the checkbox, and make sure its settings are as follows:

![image](https://user-images.githubusercontent.com/81011038/160234724-db2f6336-d6d2-46c0-9856-52438a115ed6.png)

Depending on your Windows version, you may also need to make sure that the `Action Center` system icon is enabled:

![image](https://user-images.githubusercontent.com/81011038/160234962-8a1df96d-9aef-4771-a686-2b1b2b04bb87.png)

**Important**: if you have focus assist active, you need to add HASS.Agent to the **Priority list** (thanks @jrnhrmn):

[How to Customize Focus Assist Priority List in Windows 10](https://www.tenforums.com/tutorials/102205-customize-focus-assist-priority-list-windows-10-a.html)

When you've done all this, try the `show test notification` button again in HASS.Agent. 

Did the notification show? Great, all done! If not, continue below.

----

#### Obscure Windows configurations

There have been a few reports of users which had to change weird Windows settings before it worked. For starters, if you're using privacy tools like [o&o shutup](https://www.oo-software.com/en/shutup10) or [decrapifier](https://github.com/n1snt/Windows-Decrapifier) (great tools by the way!), make sure they're not set to block notifications.

Sometimes, when telemetry is disabled, it also disables notifications. You can temporarily reenable it with the following Powershell command:

```powershell
Remove-ItemProperty -Path "HKLM:\Software\Policies\Microsoft\Windows\DataCollection" -Name "AllowTelemetry"
```

When everything's working, feel free to disable telemetry once again.

----

#### External links

The following links may (or may not) help in further debugging:

[https://www.surfacetablethelp.com/2017/02/fix-show-app-notifications-option-greyed-out-in-windows-10-settings.html](https://www.surfacetablethelp.com/2017/02/fix-show-app-notifications-option-greyed-out-in-windows-10-settings.html)
[https://www.guidingtech.com/top-ways-to-fix-notifications-not-working-on-windows-11/](https://www.guidingtech.com/top-ways-to-fix-notifications-not-working-on-windows-11/)
