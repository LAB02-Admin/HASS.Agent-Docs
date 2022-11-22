# MediaPlayer Usage and Examples

### Installation

Refer to the integration's installation and configuration pages on the left.

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
  * This file can be online (`http(s)`) or local (`C:\path\to\file.mp3`)
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

