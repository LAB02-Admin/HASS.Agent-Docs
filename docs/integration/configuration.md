# Configuration

#### MQTT

By default, the integration uses MQTT to autodetect your devices. With MQTT setup, it'll show up in your `Integrations` page:

![image](https://user-images.githubusercontent.com/81011038/201068314-6f38ce3e-cc9d-46f5-9d45-f4fb2a99ae18.png)

Just hit `CONFIGURE`, click `SUBMIT`, pick an area (optionally) and you're done!

You'll now have one (or a list of) HASS.Agent instance(s):

![image](https://user-images.githubusercontent.com/81011038/201071780-b4f31075-71a9-4f93-b3d1-b0423514aeec.png)

Each instance will have a `media_player` and `notifiy` entity, depending on what you've configured in HASS.Agent.

#### Legacy

If you don't use MQTT, you can still use HASS.Agent's local API to connect. Click `ADD INTEGRATION` in HA's `Integrations` page and search for `HASS.Agent`. Enter you device's IP and click `SUBMIT`:

![image](https://user-images.githubusercontent.com/81011038/201068870-9e5c8b9b-4ce2-480f-b0f1-c7c51511ca40.png)

You'll also need to open the configured port in the firewall of the receiving PC (default `5115`). You can do that through the configuration window:

![image](https://user-images.githubusercontent.com/81011038/201073759-8afc6477-61e3-4d87-95d1-8288f8e658fd.png)

Use the legacy integration troubleshooting on the left if something's not working.

> Note: the local API doesn't support all functionality of the MQTT variant.
