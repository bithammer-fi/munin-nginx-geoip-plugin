munin-nginx-geoip-plugin
========================

Munin plugin for monitorin Nginx accesses by country.

# Setup

The plugin requires a custom log format and few settings in munin-node configuration.

## Dependencies

- `ngx_http_geoip_module`
- `bc`

## GeoIP

This plugin depends on nginx GeoIP module. It's configured with `geoip_country   /usr/share/GeoIP/GeoIP.dat;` in `/etc/nginx/nginx.conf`.

## Log Format

Add format `log_format geoip '$remote_addr $remote_user [$time_local] $request $geoip_country_code';` to `/etc/nginx/nginx.conf` or where ever it fits the best and enable access log that uses it `access_log /var/log/nginx/access.log geoip;`.

## Munin-Node Configuration

```
[nginx_request_geoip]
user root
env.TIMESPAN 5
env.ACCESSFILE /var/log/nginx/access.log
```

Plugin needs to be run as root. Each poll reads log entries from time interval specified by TIMESPAN in minutes. Default value is 5 minutes. ACCESSFILE specifies the monitored access log. Default value is `/var/log/nginx/access.log`.

# Disk Usage

Using logrotate is recommended. There is also one indexing variable that is stored on disk between iterations in config loop.
