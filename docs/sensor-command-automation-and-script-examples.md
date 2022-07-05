#Sensor, Command, Automation and Script Examples

Below is a list of examples, provided by the community.

---

#### Template trigger: LastActive

```yaml
alias: example
trigger:
- platform: template
  value_template: "{{ (now() - states('sensor.testvm_lastactive')|as_datetime).total_seconds() < 60 }}"
action:
   ... etc ...
```

Alternative version:

```yaml
  value_template: "{{ now() - states('sensor.testvm_lastactive')|as_datetime < timedelta(minutes=1) }}"
```

#### Condition: TTS only when microphone's not active

```yaml
tts_test:
  alias: Test TTS
  mode: single
  sequence:
    - condition: state
      entity_id: binary_sensor.test_vm_mic_active
      state: "off"
    - service: notify.mycroft_tts
      data_template:
        message: >-
          {{ message }}
```

#### Switch: Wake-On-LAN

```yaml
- platform: wake_on_lan
  name: "your_pc_name"
  mac: "00-00-00-00-00-00"
  host: 10.0.0.5
  broadcast_address: 10.0.0.255
  turn_off:
    service: switch.turn_on
    data:
      entity_id: switch.your_pc_hass_agent_shutdown_switch
```

Alternative version: use a `Hibernate Command` to put your PC in hibernation instead.

You may need to enable a rule in Windows Firewall to allow ICMP (ping) packets. Open **Windows Defender Firewall with Advanced Security** by running `wf.msc` and go to **Inbound Rules**. Then look up the rules called `File and Printer Sharing (Echo Request - ICMPv*-In)`, and enable the ICMPv4 variants.

To manually add a rule, consult these docs (thanks @ArekkusuDesu):

https://docs.microsoft.com/en-us/windows/security/threat-protection/windows-firewall/create-an-inbound-icmp-rule


#### Sensor: GeoLocation

```yaml
alias: GPS voiture
description: ''
trigger:
  - platform: state
    entity_id: sensor.voiture_geolocation
condition: []
action:
  - service: device_tracker.see
    data:
      dev_id: Voiture
      gps:
        - '{{ states(''sensor.voiture_geolocation'').split(",")[0] }}'
        - '{{ states(''sensor.voiture_geolocation'').split(",")[1] }}'
mode: single
```

This places the GeoLocation sensor as a `device_tracker` on the map (thanks **Neilge**).

#### Sensor: RAM from Percent to Usage/Total

WMI Query Sensor:

Query: `SELECT Capacity FROM Win32_PhysicalMemory`

Scope: `\\.\Root\CIMV2`

Home Assistant Template Sensor:

```yaml
- sensor:
  - name: "RAM Usage"
    state: >
      {% set ram = states('sensor.laptop_memoryusage') | float %}
      {% set ramTotal = states('sensor.laptop_ramtotal') | int * 2 / 1024 / 1024 / 1024 %}
      {% set usage = ramTotal | float / 100 * ram | round(0, default=0) %}
      {% set state = usage | string + "GB" + "/" + ramTotal | string + "GB" %}
      
      {{state}}
```

The sensor makes the assumption that you have 2 identical sticks of RAM installed, which you should have for performance, but if you only have 1 stick, simply remove the * 2. `sensor.laptop_memoryusage` is my percentage sensor reported by HASS Agent by default. I simply changed my entity in the card from the HASS.Agent entity to my own sensor.

Thanks **Celisuis** ([forum post](https://community.home-assistant.io/t/hass-agent-windows-client-to-receive-notifications-use-commands-sensors-quick-actions-and-more/369094/198?u=samkr)).
