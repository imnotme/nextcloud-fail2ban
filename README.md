# NOTE:

Even if Nextcloud now natively supports bruteforce protection :

https://docs.nextcloud.com/server/12/admin_manual/configuration_server/config_sample_php_parameters.html?highlight=bruteforce

I prefer using network-level filtering instead of PHP-level filtering :
 * Less load on the webserver
 * More secure (block network access, packet will never reach application level)

## Fail2ban

Add the following to Nextcloud's config files. Remember to restart `fail2ban`
after adding the below. With Debian/Ubuntu this is done with
`/etc/init.d/fail2ban reload`

### filter.d/nextcloud.conf

Add the following file to your `fail2ban` filters directory. Note: At the
moment,Nextcloud still uses the ownCloud log type. The author will update the
examples upon release of the 'nextcloud' log type is announced.

```
[INCLUDES]
before = common.conf

[Definition]
failregex = Login failed.*Remote IP.*'<HOST>'
ignoreregex =
```

### jail.d/nextcloud.conf

Add the following text to your jail.d/nexcloud.conf file. Note: do not edit your jail.conf
file as changes may be discarded on updates.

```
[nextcloud]

enabled  = true
port     = http,https
filter   = nextcloud
logpath  = /var/www/nextcloud/nextcloud.log
```
