# Notification Debugging - HA Side

This page is based on the legacy notifier integration. It is (and will remain) functional, but for future updates, it's advisable to switch to the [new integration](https://github.com/LAB02-Research/HASS.Agent-Integration).

----

The info on this page is focussed on making sure notifications work on Home Assistant's side. Use [Notification Debugging - Client Side](https://hassagent.readthedocs.io/en/latest/notifications/notification-debugging-client-side/) to debug on the client (HASS.Agent on your PC) side.

If it still doesn't work after following all below steps, ask for help on [Discord](https://discord.gg/nMvqzwrVBU) or open a [Github Ticket](https://github.com/LAB02-Research/HASS.Agent/issues).

----

#### 1. Configuring

The most important part is that you get the basic configuration right. You need to create a `notify` entity, and use that in your automations and scripts.

Please follow [this guide](https://hassagent.readthedocs.io/en/latest/notifications/notification-usage-and-examples/) to make sure the basics are good.

----

#### 2. Home Assistant Log Debugging

You can use Home Assistant's log to see how the notification-integration's doing. Install the `Log Viewer` addon through HA's add-on store:

![image](https://user-images.githubusercontent.com/81011038/160235220-e5f1c621-9e29-44ea-8342-8df57e2c6c55.png)

Afterwards, add the following snippet to your `configuration.yaml` to enable debug logging for the integration:

```yaml
logger:
  default: warning
  logs:
    custom_components.hass_agent_notifier: debug
```

Reboot Home Assistant. Whenever you send a message, this should show up in your logs:

![Debug Output](https://raw.githubusercontent.com/LAB02-Research/HASS.Agent/main/images/notifier_debug_logging.png)

If not, something's wrong and HA can't reach your PC. Use the [Notification Debugging - HA Side](https://github.com/LAB02-Research/HASS.Agent/wiki/Notification-Debugging-:-Client-Side) manual to debug on the client (HASS.Agent on your PC) side.

----

#### 3. Manual Testing

This is a bit more advanced, but if you use a tool like [Postman](https://www.postman.com), you can manually send a test notification.

Configure as follows:

![image](https://user-images.githubusercontent.com/81011038/160235625-4c4016eb-4b71-42c9-8982-6bdfb30934f7.png)

Use this JSON object:

```json
{
    "Message": "This is a test message",
    "Title": "HASS Agent Test"
}
```

Another way is by using curl:

```
curl -X POST -H "Content-Type: text/json" -d "{\"Message\": \"Test Message\", \"Title\": \"Test Title\"}" http://{device_ip}:5115/notify
```
