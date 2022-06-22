# Adjust Screen Brightness

This example allows you to adjust your screen brightness, by using the [Monitorian](https://github.com/emoacht/Monitorian) tool and a CustomCommand. It was created by [@DivanX10](https://github.com/DivanX10) ([#86](https://github.com/LAB02-Research/HASS.Agent/issues/86)).

Note: your screen has to support DDC/CI, check the Monitorian link for more info.

---

Create a custom command:

![image](https://user-images.githubusercontent.com/64090632/174493496-4b3bc583-46ec-40cf-bcdd-57aec22d09a8.png)

We create a [mqtt light](https://www.home-assistant.io/integrations/light.mqtt) entity and configure it as below. With it, you can control the brightness of the monitor screen.
Make sure you change the `brightness_command_template` if you've placed Monitorian somewhere else.

```yaml
mqtt:
  light:
    - name: "Living room: Monitor. Screen Brightness"
      unique_id: "pc_livingroom_monitor_brightness"
      object_id: "pc_livingroom_monitor_brightness"
      command_topic: "homeassistant/light/LIVINGROOM/pc_livingroom_screen_brightness/action"
      brightness_command_topic: "homeassistant/light/LIVINGROOM/pc_livingroom_screen_brightness/action"
      brightness_command_template: "%LOCALAPPDATA%\\Microsoft\\WindowsApps\\Monitorian.exe /set {{ value }}"
      brightness_scale: 100
      on_command_type: 'brightness'
      icon: mdi:monitor-shimmer
```

To keep the brightness level in the auxiliary element number, and for the values to be equal, then you need to use this code:

```
{{ state_attr("light.pc_livingroom_monitor_brightness", "brightness")|multiply(0.392)|round(0) }}
```
