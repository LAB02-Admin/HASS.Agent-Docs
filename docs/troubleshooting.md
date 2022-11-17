# Troubleshooting

This page contains references to HASS.Agent problems and solutions that can't easily be fixed code-wise.

----

#### Debugging

If you come across a bug, the best way for the dev to help you if you can provide HASS.Agent's logs. Open the `Configuration` window, navigate to the `Logging` page and click `Open Logs Folder`:

![image](https://user-images.githubusercontent.com/81011038/202460956-80757e7c-28b4-4fc0-b0e2-27ad99e49b75.png)

In there, sort by date, and open the latest log. You can ignore the files containing `restart` or `update` etc. in their name:

![image](https://user-images.githubusercontent.com/81011038/202461663-8721786b-bdd6-4a32-a0be-62ba74c1e819.png)

Attach the content of the log to a [GitHub ticket](https://github.com/LAB02-Research/HASS.Agent/issues).

If the log's not showing anything interesting, you can enable `Extended Logging`:

![image](https://user-images.githubusercontent.com/81011038/202461944-5fd37baf-998e-407b-836e-0e9ac140a7d0.png)

Restart HASS.Agent and try to reproduce your error.

> Note: disable this afterwards, as it can make the logfiles grow really large!

----

#### The application-specific permission settings do not grant Local Activation permission for the COM Server application

Problem: HASS.Agent crashes, showing the following log entry:

```
The application-specific permission settings do not grant Local Activation permission for the COM Server application with CLSID 
{2593F8B9-4EAF-457C-B68A-50F6B8EA6B54}
 and APPID 
{15C20B67-12E7-4BB6-92BB-7AFF07997402}
```

Fix: [https://www.kapilarya.com/fix-event-10016-error-the-application-specific-permission-settings-do-not-grant-local-activation-permission-in-windows-10](https://www.kapilarya.com/fix-event-10016-error-the-application-specific-permission-settings-do-not-grant-local-activation-permission-in-windows-10)

Thanks @hastarin!

----

#### Running HASS.Agent on x86 OS

Problem: HASS.Agent is compiled as x64 and won't run as-is on the x86 build of Windows.

Fix: [https://github.com/LAB02-Research/HASS.Agent/issues/188#issuecomment-1287777056](https://github.com/LAB02-Research/HASS.Agent/issues/188#issuecomment-1287777056)

Thanks @onexey!

----

#### The integration isn't publishing through MQTT

Problem: even though everything's configured, the integration isn't autodetecting anything through MQTT

Fix: make sure that the MQTT user of both HASS.Agent and HA have access to the integration topic: `hass.agent/#`

Thanks @DavidD!
