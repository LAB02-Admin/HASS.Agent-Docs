# MediaPlayer Usage and Examples

Note: this only applies to release 2022.12.0-beta1 and up!

---

### Home Assistant Configuration

The [HASS.Agent-MediaPlayer integration](https://github.com/LAB02-Research/HASS.Agent-MediaPlayer) exposes itself as a <a href="https://www.home-assistant.io/integrations/media_player/" target="_blank">media_player integration</a>, and has to be configured as such:

```yaml
media_player: 
  name: "hass agent test mediaplayer"
  platform: hass_agent_mediaplayer
  host: 10.0.0.5
  port: 5115 [optional]
```

Replace the `host` value with the IP of the device that has an HASS.Agent instance running. To find your IP, run `ipconfig` in a command prompt on your PC. Look for the value after `IPv4 Address`. Optionally replace `5115` if you've configured a different port, normally you shouldn't have to.

Restart Home Assistant to load your configuration.

### General

You can add the mediaplayer to your dashboard by using a `Media Control Card`, and selecting your entity (for the above example, `media_player.hass_agent_test_mediaplayer`).

It'll show up as follows:

![image](https://user-images.githubusercontent.com/81011038/165727899-a6a5484b-fccc-4ad2-96a6-e38815b7c112.png)

### Media Control

The entity allows you to control your PC as if it were a regular media player, regardless of what application is actually playing. 

For the 'what's playing' functionality, the playing application needs to use Windows' media API. An example application that does this, is Spotify. An example application that doesn't do this, is VLC. 

If your application supports it, your current playing media will show up in Home Assistant:

![image](https://user-images.githubusercontent.com/81011038/165730694-ceae1f9a-87d5-4d0e-a5ef-c2116bd2439a.png)

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
  * Currently running audio will be aborted when a new file is received
  * There are no controls, so you can't pause or stop (other than perhaps play an empty file)
* You can't control the volume, but you can use Windows' Volume Mixer:

![image](https://user-images.githubusercontent.com/81011038/165731564-d870fe88-1836-4d58-ab79-22d94c401486.png)

If you're not getting your current-song information from Spotify, make sure this setting is enabled:

![image](https://user-images.githubusercontent.com/81011038/171420676-111115c0-ff15-4dad-978d-b5f3e7b6397c.png)

