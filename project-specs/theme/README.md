# Theme

**Compile SCSS**\
cd lib/themes/theme\_name

check npm -v

if npm is not installed, please run 'npm install'\
and then, 'npm run build'\
\
To check all available commands, check inside the theme, the **package.json** file

&#x20;\
dcdrupal site:mode dev

Drush commands

```ini
theme-enable        => dc exec web ./vendor/bin/drush theme-enable (drush then)
theme-disable       => dc exec web ./vendor/bin/drush theme-uninstall (drush thun)
theme-info          => dc exec web ./vendor/bin/drush pm-info
theme-list          => dc exec web ./vendor/bin/drush pm-list --type=theme
theme-list-enabled  => dc exec web ./vendor/bin/drush pm-list --type=theme --status=enabled
theme-set-default   => dc exec web ./vendor/bin/drush config-set system.theme default [theme]
theme-set-admin     => dc exec web ./vendor/bin/drush config-set system.theme admin [theme] 
theme-status        => dc exec web ./vendor/bin/drush status theme
```

### TWIG debugging

#### Enable Twig debugging options

Enabling Twig debugging involves locating the `twig.config[debug]` settings in your _services.yml_ file and changing their values.

These variables can be edited either directly in _sites/default/services.yml_ or added to the _sites/development.services.yml_ file if you followed the steps above to use a _settings.local.php_ file.

#### Edit the following variables under the `twig.config:` section

```php
debug: true
auto_reload: true
cache: false
```

If you're placing this into your _sites/development.services.yml_ file, add the `twig.config` configuration indented under the `parameters:` line. Ensure that your code additions are appropriately indented with 2 spaces, not the tab character (or an error will result).

```php
parameters:
  twig.config:
    debug: true
    auto_reload: true
    cache: false
```

From a fresh install of Drupal and with the `twig.config` additions, the _sites/development.services.yml_ file now looks like this:

```php
# Local development services.
#
# To activate this feature, follow the instructions at the top of the
# 'example.settings.local.php' file, which sits next to this file.
parameters:
  http.response.debug_cacheability_headers: true
  twig.config:
    debug: true
    auto_reload: true
    cache: false
services:
  cache.backend.null:
    class: Drupal\Core\Cache\NullBackendFactory
```
