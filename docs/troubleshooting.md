# Troubleshooting

This page contains references to HASS.Agent problems and solutions that can't easily be fixed code-wise.

---

#### Debugging integration(s)

If an integration isn't working as it should, you can opt to enable debug logging in Home Assistant. This will show you logs for every step the integration's taking, so you (or the dev) can better determine what's going wrong.

Go to your file-editor-of-choice in HA, and open `configuration.yaml`. Add the following snippet:

```yaml
logger:
  default: warning
  logs:
    custom_components.hass_agent: debug
```

This is for the new integration. If you use either of the old ones, you can add these rows:

```yaml
    custom_components.hass_agent_notifier: debug
```


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


#### Running HASS.Agent on x86 OS

Problem: HASS.Agent is compiled as x64 and won't run as-is on the x86 build of Windows.

Fix: [https://github.com/LAB02-Research/HASS.Agent/issues/188#issuecomment-1287777056](https://github.com/LAB02-Research/HASS.Agent/issues/188#issuecomment-1287777056)

Thanks @onexey!


#### The integration isn't publishing through MQTT

Problem: even though everything's configured, the integration isn't autodetecting anything through MQTT

Fix: make sure that the MQTT user of both HASS.Agent and HA have access to the integration topic: `hass.agent/#`

Thanks @DavidD!
