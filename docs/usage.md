# Usage

HASS.Agent resides in the system tray. Make sure it's always visible and not hidden in the overflow, otherwise notifications may not get shown. As a tip, you could drag the icon right next to your clock.

You can double-click to open the Home screen:

![image](https://user-images.githubusercontent.com/81011038/198242015-a46a60d6-a555-4049-8c7d-bb67dee21e72.png)

From here, you can easily use & configure the various parts of HASS.Agent:

#### 1. Configuration

Shows the various application configuration options (as discussed above).

#### 2. Quick Actions

Manage your quick action buttons. Use the `add new` button to create your first action. All entities are dynamically acquired from your Home Aassistant instance. If they won't show up, make sure you've configured 'HASS api' in the configuration screen. Use the 'preview' button to check if it's working for you.

#### 3. Satellite Service

From here, you can configure there service's inner workings (MQTT, commands, sensors ..). Since 2022.13, the MQTT configuration you set in HASS.Agent will get applied to the service as well (minus the client ID). 

#### 4. Local Sensors

Manage which sensors you want to publish to your Home Assistant instance. There are some ready-to-use sensors available, but you can also use your own WMI query.
*This requires MQTT to be configured*.

Most sensors are single value, but some are **multi-value sensors**. These sensors are configured as one entity, but will create multiple entities in Home Assistant. These sensors in turn can have *attributes* configured to provide even more information.

**Note: WMI can be a pain, and also make sure you don't update your queries too often. Keep an eye on your CPU load.**

#### 5 Commands

Manage which commands should be accepted from your Home Assistant instance. Aside from the builtin commands, you can use your own custom command (you can test your command by typing it into a console), or a key command which will emulate key presses. *This requires MQTT to be configured.*

Example configuration of a shutdown command in Home Assistant, used in combination with <a href="https://www.home-assistant.io/integrations/wake_on_lan/" target="_blank">wake-on-lan</a>:

```yaml
- platform: wake_on_lan
  name: "TEST_W10_x64_01"
  mac: "00-00-00-00-00-00"
  host: 10.0.0.5
  broadcast_address: 10.0.0.255
  turn_off:
    service: switch.turn_on
    data:
      entity_id: switch.test_w10_x64_01_cmd_shutdown
```


#### 6 Commands - Actions

Some commands have support for **actions**. You can use automations to send extra variables along with the command, for instance the exact level to which the volume has to be set, or an URL to open.

There's a page dedicated to these: [Command Actions Usage & Examples](https://hassagent.readthedocs.io/en/latest/commands/actions-usage-and-examples/).

#### 7 Commands - Custom Executor

Using the `Custom Executor` command, you can use any application to extend HASS.Agent even more. For instance, `python.exe` or a custom tool you built.

Another powerful tool (tip provided by **SankaGrandi**) is [NirCmd](https://www.nirsoft.net/utils/nircmd.html). Just set `nircmdc.exe` as the `custom executor binary` in Configuration and you're ready:

![image](https://user-images.githubusercontent.com/81011038/198242777-45cc94f9-cff3-47f6-8661-00e51d44e8ef.png)

