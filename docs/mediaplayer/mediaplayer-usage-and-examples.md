# MediaPlayer Usage and Examples

Note: this only applies to release 2022.12.0-beta1 and up!

---

### Home Assistant Configuration

Make sure MQTT is configured (or, alternatively, the local API) and install the [new integration](https://github.com/LAB02-Research/HASS.Agent-Integration). Your HASS.Agent will get discovered and show in the integrations page:

![image](https://user-images.githubusercontent.com/81011038/198243976-ea1322ec-74da-4558-b261-58b690b4d563.png)

Just hit `CONFIGURE`, click `SUBMIT`, pick an area (optionally) and you're done!

You'll now have a list of HASS.Agent instances:

![image](https://user-images.githubusercontent.com/81011038/198244279-9a6804bb-c103-4c75-ad72-b6cdb07e8048.png)

Each instance will have a `media_player` and `notifiy` entity.

### General

You can add the mediaplayer to your dashboard by using a `Media Control Card`, and selecting your entity (for the above example, `media_player.hass_agent_test_mediaplayer`).

It'll show up as follows:

![image](https://user-images.githubusercontent.com/81011038/165727899-a6a5484b-fccc-4ad2-96a6-e38815b7c112.png)

### Media Control

The entity allows you to control your PC as if it were a regular media player, regardless of what application is actually playing. 

For the 'what's playing' functionality, the playing application needs to use Windows' media API. An example application that does this, is Spotify. An example application that doesn't do this, is VLC. 

If your application supports it, your current playing media will show up in Home Assistant:

![image](https://user-images.githubusercontent.com/81011038/198244995-e962c2b4-b8a7-488e-b4a4-b7279600326e.png)

### Text-to-Speech

By using this integration, you can use your PC as a text-to-speech target. You can easily test the functionality by using the developer tools in Home Assistant:

![image](https://user-images.githubusercontent.com/81011038/165731015-140b67f5-cd52-4f45-a994-d4a81dd293f2.png)

Or through YAML:

```yaml
service: tts.google_translate_say
data:
  entity_id: media_player.hass_agent_test_mediaplayer
  message: this is a test message
```

Replace `google_translate_say` if you're using any other TTS service.

### Notes

Some extra info:

* Besides TTS, you can play any .mp3 file on HASS.Agent directly
  * This file can be online (http) or local
  * The `media_content_type` must be `music`
  * Currently running audio will be aborted when a new file is received

YAML to play a .mp3 file on the agent:

```yaml
service: media_player.play_media
data:
  # Hosted on Home Assistant (/config/www folder)
  media_content_id: media-source://media_source/local/Alarm1.mp3
  # Or streamed from another server
  # media_content_id: https://download.samplelib.com/mp3/sample-3s.mp3
  media_content_type: music
target:
  entity_id: media_player.hass_agent_test_mediaplayer
```

If you're not getting your current-song information from Spotify, make sure this setting is enabled:

![image](https://user-images.githubusercontent.com/81011038/171420676-111115c0-ff15-4dad-978d-b5f3e7b6397c.png)

