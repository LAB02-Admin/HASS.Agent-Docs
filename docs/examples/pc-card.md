# PC Card

This example allows you to create a PC card in your dashboard. It was created by **Mouhwahno**.

![image](https://user-images.githubusercontent.com/81011038/190636934-0cb35b78-a466-4dd6-adb9-31b2fc3264fd.png)

Make sure you change all `name` values and other specific references. Feel free to join the Discord for help.

Note: it uses the Powershell screenshot example, [found here](https://hassagent.readthedocs.io/en/latest/sensor-command-automation-and-script-examples/#command-grab-screenshot-using-powershell).

---

WOL switch:

You need to change the entity_id to that of your HASS.Agent `shutdown` command.

```yaml
switch:
  - platform: wake_on_lan
    name: "PC Blanc"
    mac: "AA:AA:AA:AA:AA:AA"
    host: 192.168.0.1
    broadcast_address: 192.168.0.255
    turn_off:
      service: button.press
      data:
        entity_id: button.pc_Blanc_shutdown
```

Screenshot camera:

Use [this example](https://hassagent.readthedocs.io/en/latest/sensor-command-automation-and-script-examples/#command-grab-screenshot-using-powershell).

```yaml
camera:
  - platform: local_file
    file_path: /media/PC_Blanc/PC-BLANC-screenshot.png
    name: "PC Blanc"
```

Uptime sensor:

**Mouhwahno** uses a PowershellSensor to get the uptime.
 
 ```
 ((Get-Date) - (Get-CimInstance Win32_OperatingSystem).LastBootUpTime).ToString('dd\.hh\:mm\:ss')
 ```

YAML layout code:

```yaml
type: custom:layout-card
layout_type: custom:vertical-layout
layout: {}
cards:
  - type: markdown
    content: |
      # <img height="40" src='/local/logo/pcmr.png' /> PC-BLANC 
    card_mod:
      style:
        ha-markdown $: |
          h1 img {
            vertical-align: bottom;
          }
        .: |
          ha-card {
            background-color: transparent !important;
            box-shadow: none !important;
            margin-bottom: -15px;
            margin-top: -15px;
            text-align: center;
          }
  - type: custom:stack-in-card
    mode: vertical
    cards:
      - type: custom:stack-in-card
        mode: horizontal
        card_mod:
          style: |
            ha-card {
              background: none;
              --ha-card-box-shadow: 0px;
            }
        cards:
          - type: custom:mushroom-template-card
            entity: switch.pc_blanc
            primary: PC Cube
            secondary: |-
              {% set st = states('switch.pc_blanc') %}
              {% set up = 'Uptime: ' + states(sensor.pc_blanc_uptime) %}
              {% if st == 'on' %}
                {{ up }}
              {% else %}
              {% endif %}
            icon: mdi:desktop-tower-monitor
            badge_icon: |-
              {% set temp = states('sensor.pc_blanc_cputemp') %}
              {% if temp == 'unavailable' %}
              {% elif temp > '65' %}
                mdi:thermometer
              {% endif %}
            fill_container: false
            multiline_secondary: false
            badge_color: |-
              {% set temp = states('sensor.pc_blanc_cputemp') %}
              {% if temp < '40' %} 
                green
              {% elif temp < '50' %}
                light-green
              {% elif temp < '55' %}
                lime
              {% elif temp < '60' %}
                yellow
              {% elif temp < '65' %}
                amber
              {% elif temp < '68' %}
                orange
              {% elif temp < '70' %}
                deep-orange
              {% else %}
                red
              {% endif %}
            icon_color: |-
              {% set st = states(entity) %}
              {% if st == 'on' %}
                blue
              {% else %}
                grey
              {% endif %} 
            tap_action:
              action: call-service
              service: switch.toggle
              data: {}
              target:
                entity_id: switch.pc_blanc
          - type: custom:state-switch
            entity: switch.pc_blanc
            states:
              'on':
                type: custom:mushroom-chips-card
                alignment: end
                chips:
                  - type: template
                    entity: sensor.pc_blanc_cputemp
                    content: '{{states(entity) | round(0) }} °C'
                    icon_color: |-
                      {% set temp = states(entity) %}
                      {% if temp < '40' %} 
                        green
                      {% elif temp < '50' %}
                        light-green
                      {% elif temp < '55' %}
                        lime
                      {% elif temp < '60' %}
                        yellow
                      {% elif temp < '65' %}
                        amber
                      {% elif temp < '68' %}
                        orange
                      {% elif temp < '70' %}
                        deep-orange
                      {% else %}
                        red
                      {% endif %} 
                    icon: mdi:thermometer
                    card_mod:
                      style: |
                        ha-card {
                          --chip-box-shadow: none;
                          --chip-background: none;
                          --chip-padding: 0 0.3em;
                          margin-top: 0.4em;
                          margin-right: 0.4em;
                          --chip-font-size: 0.4em;
                          --chip-icon-size: 0.6em;
                        }
      - type: custom:state-switch
        entity: switch.pc_blanc
        states:
          'on':
            type: picture-glance
            camera_view: auto
            entities: []
            camera_image: camera.pc_blanc
            theme: Mushroom
```
