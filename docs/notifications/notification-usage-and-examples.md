# Notification Usage and Examples

### Home Assistant Configuration

The [HASS.Agent-Notifier integration](https://github.com/LAB02-Research/HASS.Agent-Notifier) exposes itself as a <a href="https://www.home-assistant.io/integrations/notify/" target="_blank">notifications integration</a>, and can be configured by adding this snippet in your `configuration.yaml` file:

```yaml
notify: 
  name: "hass agent test device"
  platform: hass_agent_notifier
  resource: http://{device_ip}:5115/notify
```

Replace `{device_ip}` with the IP of the device that has an HASS.Agent instance running. To find your IP, run `ipconfig` in a command prompt on your PC. Look for the value after `IPv4 Address`. Optionally replace `5115` if you've configured a different port, normally you shouldn't have to.

Restart Home Assistant to load your configuration.

### General

Currently, there are four variables you can set:

 * `message`: the message you want to show
 * `title`: the title of your popup [optional]
 * `image`: http(s) url containing the location of an image [optional]
 * `duration`: duration (in seconds) for which the popup will be shown [optional]

### Text notification

```yaml
  action:
    - service: notify.hass_agent_test_device
      data:
        message: "This is a test message."
```

### Text notification with title and duration

```yaml
  action:
    - service: notify.hass_agent_test_device
      data:
        message: "This is a test message with title and 3 sec duration."
        title: "HASS.Agent Test"
        data:
          duration: 3
```

### Image notification

```yaml
  action:
    - service: notify.hass_agent_test_device
      data:
        message: "This is a test message with an image."
        data:
          image: "http://10.0.0.6:1234/jpeg/image.jpg"
```

### Camera Proxy

You can also use Home Assistant's camera proxy. This way you don't have to share the credentials etc. of your camera. Home Assistant will provide a token that's valid for 5 minutes, so it's safe to use.

Example script:

```yaml
notification_test:
  alias: Notification Test
  variables:
    image: |
      {%- set image = "http://hass.local:8123" + state_attr("camera.garden","entity_picture") %}
      {{ image }}
  sequence:
  - service: notify.hass_agent_test
    data:
      title: Test
      message: "This is a test message with an image."
      data:
        image: "{{ image }}"
  mode: single
  icon: mdi:bell
```

Optionally change `hass.local` to the mDNS/IP of your Home Assistant instance, and change `garden` to the name of your camera - or use another variable.

### Multiple Receivers

You can configure multiple HASS.Agent receivers:

```yaml
notify:
  - name: device_one
    platform: hass_agent_notifier
    resource: http://{device_ip}:5115/notify
  - name: device_two
    platform: hass_agent_notifier
    resource: http://{device_ip}:5115/notify
```

And optionally combine them as a group:

```yaml
notify:
  name: hassagent_group
  platform: group
  services:
    - service: device_one
    - service: device_two
```

### Script GUI examples

This is the sequence part of a test script to send a text-only message, created through the Home Assistant GUI:

![Script Test Notification](https://raw.githubusercontent.com/LAB02-Research/HASS.Agent/main/images/notifier_script_example.png)

This is the same script, but with an image added to the notification:

![Script Test Image Notification](https://raw.githubusercontent.com/LAB02-Research/HASS.Agent/main/images/notifier_script_image_example.png)

You can use the new <a href="https://www.home-assistant.io/lovelace/button/" target="_blank">Button Card</a> to trigger your test scripts.
