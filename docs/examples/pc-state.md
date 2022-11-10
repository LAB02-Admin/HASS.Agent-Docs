# PC State Cards

Both examples were created by **Rokanishu**.

---

This example allows you to create cards showing the state of your PC in your dashboard. 

![image](https://user-images.githubusercontent.com/81011038/201060163-21512048-0156-431b-8b50-52c35acecf09.png)

![image](https://user-images.githubusercontent.com/81011038/201060198-25c62a77-5b12-43aa-8d2e-d736775c7514.png)

```yaml
type: entities
entities:
  - entity: binary_sensor.office_motion_sensor_ias_zone
    secondary_info: last-changed
    name: Motion Detected
  - entity: binary_sensor.lumi_lumi_sensor_motion_aq2_ias_zone
    name: Desk Motion Sensor
    secondary_info: last-updated
  - entity: binary_sensor.lumi_lumi_sensor_magnet_aq2_on_off
    secondary_info: last-changed
    name: Office Door
  - type: conditional
    conditions:
      - entity: sensor.hass_desktop_sessionstate
        state_not: Unlocked
    row:
      entity: sensor.desktop_lastactive
      icon: mdi:sleep
      state_color: true
      name: PC Last Active (Off)
  - type: conditional
    conditions:
      - entity: sensor.hass_desktop_sessionstate
        state: Unlocked
    row:
      entity: sensor.desktop_lastactive
      icon: mdi:mouse
      state_color: true
      name: PC Last Active
```

---

This example shows specific buttons for when the PC is asleep/off or on.

![image](https://user-images.githubusercontent.com/81011038/201060528-046c3b99-9b32-473c-9379-3b3b627ae17f.png)

![image](https://user-images.githubusercontent.com/81011038/201060551-16c1686d-cae2-4e6e-88f8-8b56d0890e9d.png)

```yaml
square: false
columns: 1
type: grid
cards:
  - type: conditional
    conditions:
      - entity: sensor.hass_desktop_sessionstate
        state_not: Unlocked
    card:
      show_name: true
      show_icon: true
      show_state: false
      type: glance
      entities:
        - entity: script.button_wake_desktop
          name: Desktop WOL
          tap_action:
            action: toggle
          icon: mdi:power
      state_color: false
  - type: conditional
    conditions:
      - entity: sensor.hass_desktop_sessionstate
        state_not: unavailable
      - entity: sensor.hass_desktop_sessionstate
        state_not: unknown
    card:
      show_name: true
      show_icon: true
      show_state: false
      type: glance
      entities:
        - entity: button.desktop_hibernate
          name: Sleep Desktop
          tap_action:
            action: toggle
          icon: mdi:power
        - entity: button.desktop_monitorsoff
          name: Monitors Off
          tap_action:
            action: toggle
          icon: mdi:monitor-off
        - entity: button.desktop_launchhyperion
          name: Run Hyperion
          tap_action:
            action: toggle
          icon: mdi:television-ambient-light
        - entity: media_player.office_tv
          name: Toggle TV
          tap_action:
            action: call-service
            service: media_player.toggle
            target:
              entity_id: media_player.office_tv
      state_color: false
```
