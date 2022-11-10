#Sensor, Command, Automation and Script Examples

Below is a list of examples, provided by the community.

---


#### Template sensor: Storage Capacity

Starting with 2022.13.0-beta2, multivalue sensors are using attributes. That means that a lot of values aren't reachable as regular sensors anymore. You can use `state_attr` to fetch the value, or even to create another sensor with just that specific value.

Here's an example to get the AvailableSpaceMB attribute from a Storage multivalue sensor:

```yaml
template:
  sensor:
  - storage_capacity_c:
      friendly_name: "Storage Capacity Drive C"
      unit_of_measurement: "MB"
      value_template: "{{ state_attr('sensor.hass_agent_storage_c', 'AvailableSpaceMB') | float(0) }}"
```

Make sure to change `sensor.hass_agent_storage_c` to the drive's sensor, and optionally `AvailableSpaceMB` to the attribute you want to use. The `| float(0)` part converts it to a number-based value, you can drop that if you want it as a text value.

Reboot HA, and it'll show up as `sensor.storage_capacity_c`.


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

Alternative version (binary template sensor so you can use the true/false anywhere):

```yaml
template:
  - binary_sensor:
      - name: "User Active"
        state: "{{ as_timestamp(now(),0) - as_timestamp(states('sensor.staging_lastactive'),0) < 60 }}"
```

This will also not error when the sensor's unavailable (thanks [dbrunt](https://community.home-assistant.io/t/how-to-handle-unknown-or-unavailable-states-in-template-entities-or-automations/411632/14)!).

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

To manually add a rule, consult these docs (thanks @ArekkusuDesu!):

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

This places the GeoLocation sensor as a `device_tracker` on the map (thanks **Neilge**!).

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

Thanks **Celisuis** ([forum post](https://community.home-assistant.io/t/hass-agent-windows-client-to-receive-notifications-use-commands-sensors-quick-actions-and-more/369094/198?u=samkr))!

#### Automation: Change LCD brightness based on lux and session state

Uses a CustomCommand with Actions to trigger Monitorian, and a LastSystemStateChangeSensor to check whether the user's active.

```yaml
alias: Change LCDs brightness based on lux
mode: queued
variables:
  # 25 - min value to set on first LCD (my LCD is hard to view below this value)
  # 30 - the difference between max value and min value that should be set, which gives us number of steps LCD can be set on (above 55 my LCD is already to bright for normal viewing)
  # 220 - the max lux value I've observed where the LCDs are standing throughout normal day of work
  # states("sensor.xiaomi_lumi_sen_ill_mgl01_illuminance") | int(0) - make sure to get int value, not text
  lux_lcd01: '{{ 25 + (( (states("sensor.xiaomi_lumi_sen_ill_mgl01_illuminance") | int(0)) / 220) * 30) | int(0) }}'
trigger:
  - platform: state
    entity_id: sensor.xiaomi_lumi_sen_ill_mgl01_illuminance
    to: ~ 
condition:
  # set brightness only if the PC is up by looking for specific values from sensor set in HASS.Agent
  - condition: template
    value_template: "{{ states('sensor.earth_lastsystemstatechange') in ['ApplicationStarted', 'Resume', 'SessionLogon', 'SessionUnlock'] }}"
  # don't update brightness if the last trigger was less the 10s ago but do if there was no trigger at all (like after restarting HA)
  - condition: template
    value_template: "{{ state_attr('automation.lcd_change_brightness', 'last_triggered') != 'None' or ((as_timestamp(utcnow()) - as_timestamp(state_attr('automation.lcd_change_brightness', 'last_triggered')) |int(0) ) > 10) }}"
  - condition: template
    value_template: '{{ states("input_number.lux_value_for_automation") | int(0) != lux_lcd01 }}'
action:
  - service: mqtt.publish
    data:
      topic: "homeassistant/light/EARTH/set_lcd01_brightness/action"
      payload_template: '%LOCALAPPDATA%\\Microsoft\\WindowsApps\\Monitorian.exe /set "DISPLAY\AOC2713\5&3f30a18&0&UID4357" {{ lux_lcd01 }}'
  - service: mqtt.publish
    data:
      topic: "homeassistant/light/EARTH/set_lcd01_brightness/action"
      # this is the same as for lux_lcd01 but second LCD has different min and max (zero and 7 - yeah, that's how bright it is...)
      payload_template: '%LOCALAPPDATA%\\Microsoft\\WindowsApps\\Monitorian.exe /set "DISPLAY\PHL08E9\5&3f30a18&0&UID4355" {{ (( (states("sensor.xiaomi_lumi_sen_ill_mgl01_illuminance") | int(0)) / 220) * 7) | int(0) }}'
  # I'm saving this to use later on when checking if the brightness changes even if the lux changed (automation was triggered)
  - service: input_number.set_value
    target:
      entity_id: input_number.lux_value_for_automation
    data:
      value: '{{ lux_lcd01 }}'
```

Thanks @Draghmar! Awesome detailed writeup :)

#### Light: Create a light entity for your monitor's powerstate

```yaml
- platform: template
  lights:
    pc_monitor:
      friendly_name: "PC Monitor"
      unique_id: pc_monitor
      availability_template: '{{ not is_state("binary_sensor.my_pc_is_active", "unknown") }}'
      icon_template: >-
        hass: monitor
      value_template: >-
        {{ is_state("sensor.windows_11_monitorpowerstate", "PowerOn") }}
      turn_on:
        service: button.press
        entity_id: button.windows_11_monitorwake
      turn_off:
        service: button.press
        entity_id: button.windows_11_monitorsleep
```

Thanks @danielbrunt57!

#### Automation: Toggle light based on motion and pc activity

```yaml
alias: Office Lights
description: ""
trigger:
  - platform: state
    entity_id:
      - binary_sensor.my_pc_is_active
    id: PC Active
    to: "on"
  - platform: state
    entity_id:
      - binary_sensor.office_motion
    id: Motion on
    to: "on"
  - platform: state
    entity_id:
      - binary_sensor.my_pc_is_active
    id: PC Idle
    to: "off"
    for:
      hours: 0
      minutes: 5
      seconds: 0
  - platform: state
    entity_id:
      - binary_sensor.office_motion
    to: "off"
    id: Motion off
    for:
      hours: 0
      minutes: "{{ states('input_number.office_lights_auto_off_time') }}"
      seconds: 0
condition: []
action:
  - choose:
      - conditions:
          - condition: or
            conditions:
              - condition: trigger
                id: PC Active
              - condition: trigger
                id: Motion on
        sequence:
          - if:
              - condition: state
                entity_id: sensor.windows_11_monitorpowerstate
                state: "PowerOff"
            then:
              - service: light.turn_on
                data: {}
                target:
                  entity_id: light.pc_monitor
          - if:
              - condition: state
                entity_id: light.office_lights
                state: "off"
            then:
              - service: light.turn_on
                data: {}
                target:
                  entity_id: light.office_lights
      - conditions:
          - condition: and
            conditions:
              - condition: state
                entity_id: binary_sensor.my_pc_is_active
                state: "off"
              - condition: state
                entity_id: binary_sensor.office_motion
                state: "off"
        sequence:
          - service: light.turn_off
            data: {}
            target:
              area_id: office
    default: []
mode: restart
```

Thanks @danielbrunt57!

#### Command: Grab screenshot using Powershell

A small Powershell script to save the screen and write it to the www folder on home assistant, then use the standard Generic Camera to use the image.
This is the powershell to grab the shot. Then simply schedule a standard automation to update every two minutes.

```powershell
$Path = "\\HOMEASSISTANT\config\www"

# Make sure that the directory to keep screenshots has been created, otherwise create it
If (!(test-path $path)) {
  New-Item -ItemType Directory -Force -Path $path
}

Add-Type -AssemblyName System.Windows.Forms

$screen = [System.Windows.Forms.Screen]::PrimaryScreen.Bounds

# Get the current screen resolution
$image = New-Object System.Drawing.Bitmap($screen.Width, $screen.Height)

# Create a graphic object
$graphic = [System.Drawing.Graphics]::FromImage($image)

$point = New-Object System.Drawing.Point(0, 0)

$graphic.CopyFromScreen($point, $point, $image.Size);

$cursorBounds = New-Object System.Drawing.Rectangle([System.Windows.Forms.Cursor]::Position, [System.Windows.Forms.Cursor]::Current.Size)

# Get a screenshot
[System.Windows.Forms.Cursors]::Default.Draw($graphic, $cursorBounds)

# $screen_file = "$Path\" + $env:computername + "_" + $env:username + "_" + "$((get-date).tostring('yyyy.MM.dd-HH.mm.ss')).png"
$screen_file = "$Path\" + $env:computername + "-screenshot.png"

# Save the screenshot as a PNG file
$image.Save($screen_file, [System.Drawing.Imaging.ImageFormat]::Png)
```

Thanks kind [stranger](https://lab02research.youtrack.cloud/issue/hassagent-85/Sensor-Screenshot#focus=Comments-4-19.0-0)!

Note: make sure your PS execution policy [allows execution](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/set-executionpolicy?view=powershell-7.2) (to allow remote unsigned, but require remote scripts to be signed, use `Set-ExecutionPolicy RemoteSigned`). Thanks **drjjr**!

#### Automation: Send a notification when Spotify starts playing

```yaml
alias: HASS.Agent - Spotify is playing audio
description: ""
trigger:
  - platform: template
    value_template: >-
      {%- if state_attr('sensor.hass_desktop_audio_audio_sessions',
      'AudioSessions') | selectattr('Application', 'match', 'Spotify') | list 
      %}
      true
      {%- else %}
      false
      {%- endif %}
condition: []
action:
  - service: notify.hass_agent_desktop
    data:
      message: Spotify is playing music
      data:
        image: none
mode: single
```

Thanks **Rokanishu**!

#### Sensor: Battery percentage to Bluetooth device

Based on: https://stackoverflow.com/questions/71736070/how-to-get-bluetooth-device-battery-percentage-using-powershell-on-windows

Create a `PowershellSensor`:

```powershell
(Get-PnpDevice -Class 'Bluetooth' -FriendlyName ''Xbox Wireless Controller' | Get-PnpDeviceProperty -KeyName '{104EA319-6EE2-4701-BD47-8DDBF425BBE5} 2').Data
```

[Customize](https://www.home-assistant.io/docs/configuration/customizing-devices/#manual-customization) the sensor:

```yaml 
sensor.desktop_windows_xboxbattery:
  device_class: battery
```

Thanks @bkbilly!

#### Command: Change default audio playback device

Uses nircmd: https://www.nirsoft.net/utils/nircmd.html

Create a `CustomCommand` (button type):

`nircmd setdefaultsounddevice <your device>`

Optionally use `input_select` in HA: https://github.com/LAB02-Research/HASS.Agent/issues/97#issuecomment-1288040508

Thanks @tachibanayui!
