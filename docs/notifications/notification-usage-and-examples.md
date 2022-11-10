# Notification Usage and Examples

### Home Assistant Configuration

By default, the integration uses MQTT to autodetect your devices. With MQTT setup, it'll show up in your `Integrations` page:

![image](https://user-images.githubusercontent.com/81011038/201068314-6f38ce3e-cc9d-46f5-9d45-f4fb2a99ae18.png)

Just hit `configure` and you'll be good to go.

If you don't use MQTT, you can still use HASS.Agent's local API to connect. Click `ADD INTEGRATION` in HA's `Integrations` page and search for `HASS.Agent`. Enter you device's IP and click `SUBMIT`:

![image](https://user-images.githubusercontent.com/81011038/201068870-9e5c8b9b-4ce2-480f-b0f1-c7c51511ca40.png)

Use the legacy integration troubleshooting on the left if something's not working.

### General

Currently, there are four variables you can set:

 * `message`: the message you want to show
 * `title`: the title of your popup [optional]
 * `image`: http(s) url containing the location of an image [optional]
 * `duration`: duration (in seconds) for which the popup will be shown [optional]

You can also configure actionable notifications, refer to the [actionable notifications](https://hassagent.readthedocs.io/en/latest/notifications/new/notification-actionable/) docs for more info.

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

Example automation that sends an image when the doorbell's pressed (thanks @bharvey88):

```yaml
alias: Send image notification on doorbell
trigger:
  - platform: state
    entity_id:
      - binary_sensor.doorbell
    to: "on"
condition: []
action:
  - service: notify.hassagent_desktop
    data:
      message: Someone is at the door
      title: Doorbell
      data:
        image: /api/camera_proxy/camera.doorbell
        duration: 3
mode: single
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
