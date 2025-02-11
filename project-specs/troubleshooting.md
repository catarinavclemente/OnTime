# 🐞 Troubleshooting

## <mark style="color:red;">No space left on device when using docker-compose</mark> <a href="#id-73b0" id="id-73b0"></a>

[https://medium.com/@wlarch/no-space-left-on-device-when-using-docker-compose-why-c4a2c783c6f6](https://medium.com/@wlarch/no-space-left-on-device-when-using-docker-compose-why-c4a2c783c6f6)

#### <mark style="color:red;">Failed loading /usr/lib64/php/modules/xdebug.so: /usr/lib64/php/modules/xdebug.so: undefined symbol: gc\_globals</mark>

In GNU/Linux systems derived from modern Red Hat releases (such as RHEL, Fedora, and CentOS), the PHP distribution may be divided into the primary .ini file residing at `/etc/php.ini` as well as a collection of other .ini files specific to extensions or packages installed as RPMs residing in `/etc/php.d`. At initialization, PHP will read `/etc/php.d/*.ini` after loading the main `php.ini` file.

It would seem that you have a stale file containing Xdebug settings residing in `/etc/php.d`. Grep for Xdebug in `/etc/php.d` to find the offender and remove it or comment out the relevant lines.

```bash
grep xdebug /etc/php.d/*.ini
```

If Xdebug was installed separately by a manual process and you manually modified the main `/etc/php.ini` to load the extension and configure its settings, that would explain why Xdebug otherwise works while you still see errors about its nonexistence at `/usr/lib/php/modules/xdebug.so`. This also could have happened if configuration files from `/etc` were copied from an old 32bit system to a 64bit system wherein modules reside at `/usr/lib64/php/modules`

\`\`

<figure><img src="../.gitbook/assets/Screenshot 2022-12-09 140432.png" alt=""><figcaption><p>Can't download database dump </p></figcaption></figure>

Run docker-compose exec web ./vendor/bin/run toolkit:download-dump --define toolkit.clone.asda\_type=default

## EasyRdf\Exception: Unsupported URI scheme&#x20;

If someone come across this issue it is due to DRUPAL\_SPARQL\_HOSTNAME in your config file returning null.\
Add to .env:\
DRUPAL\_SPARQL\_HOSTNAME=sparql DRUPAL\_SPARQL\_PORT=8890 DRUPAL\_ACCOUNT\_USERNAME=admin DRUPAL\_ACCOUNT\_PASSWORD=admin

\
