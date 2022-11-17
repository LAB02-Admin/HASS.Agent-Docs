# Debugging

#### Home Assistant

If an integration isn't working as it should, you can opt to enable debug logging in Home Assistant. This will show you logs for every step the integration's taking, so you (or the dev) can better determine what's going wrong.

Go to your file-editor-of-choice in HA, and open `configuration.yaml`. Add the following snippet:

```yaml
logger:
  default: warning
  logs:
    custom_components.hass_agent: debug
```

This is for the new integration. If you use either of the old ones, you can add these rows:

```yaml
    custom_components.hass_agent_notifier: debug
    custom_components.hass_agent_mediaplayer: debug
```

Reboot HA, and it'll be activated. 

> Tip: install the [Log Viewer](https://github.com/hassio-addons/addon-log-viewer) add-on for easy viewing.

You should see rows like this coming in:

![image](https://user-images.githubusercontent.com/81011038/202460423-721d495d-b160-49e6-a89c-7ab3b66f24ab.png)

----

#### HASS.Agent

If you're sure everything's working from HA's end, you can check HASS.Agent's logs. Open the `Configuration` window, navigate to the `Logging` page and click `Open Logs Folder`:

![image](https://user-images.githubusercontent.com/81011038/202460956-80757e7c-28b4-4fc0-b0e2-27ad99e49b75.png)

In there, sort by date, and open the latest log. You can ignore the files containing `restart` or `update` etc. in their name:

![image](https://user-images.githubusercontent.com/81011038/202461663-8721786b-bdd6-4a32-a0be-62ba74c1e819.png)

Attach the content of the log to a [GitHub ticket](https://github.com/LAB02-Research/HASS.Agent/issues).

If the log's not showing anything interesting, you can enable `Extended Logging`:

![image](https://user-images.githubusercontent.com/81011038/202461944-5fd37baf-998e-407b-836e-0e9ac140a7d0.png)

Restart HASS.Agent and try to reproduce your error.

> Note: disable this afterwards, as it can make the logfiles grow really large!
