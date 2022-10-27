# Notification Debugging - SSL

If you use the DuckDNS addon, you can only access Home Assistant through SSL. However, you might want to retrieve notification images directly from your HA's local IP. You can solve this by using the **NGINX Home Assistant SSL proxy** addon in addition to DuckDNS.

Configuration steps:

- Install `NGINX Home Assistant SSL proxy` addon
- Configure as follows:

```yaml
certfile: fullchain.pem
cloudflare: false
customize:
  active: false
  default: nginx_proxy_default*.conf
  servers: nginx_proxy/*.conf
domain: your_domain.duckdns.org
hsts: max-age=31536000; includeSubDomains
keyfile: privkey.pem
```

- Start the addon, it'll give errors in the log, don't mind those
- You probably changed the `http` section of `configuration.yaml` for the DuckDNS addon, change it to the following:

```yaml
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 172.30.33.0/24
```

- Reboot Home Assistant
- If you set port forwarding in your router, you need to change the destination port from `8123` to `443`
  - You can of course keep the source port the same, as long as it forwards to `443`

You should now be able to access your Home Assistant instance through both the DuckDNS url and through its local IP, and use the local IP to retrieve notification images.