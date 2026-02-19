---
description: >-
  First, we'll define configuration in Drupal's context, what it is used for and
  what options we have for managing it in Drupal, both as a site builder and as
  a developer.
---

# Configuration system

### What is configuration?

Configuration, in Drupal, is the data that the proper functioning of an application relies upon.\
In versions prior to Drupal 8, data was simply stored in the database and the lack of consistency of Features or Ctools exportables meant big risks and headaches for Drupal developers.

The [State API](https://api.drupal.org/api/drupal/core!core.api.php/group/state_api) is a simple system for the storage of information about the system's state. The information is stored in the database and will be lost when the database is dropped or the site is re-installed from configuration. For storing data which needs to be edited by humans or needs to be shared between environments use the[ Configuration API](https://www.drupal.org/developing/api/8/configuration). In Drupal 7 and earlier the variables system was used to store state information.

State information is stored in the database and has the following characteristics:

* It is not meant to be exported.
* It is specific to an individual environment.
* It is not to be deployed to another environment.
* All state information is lost when you reset the database.

Since Drupal 8, the configuration API is, therefore, an indispensable tool.

The configuration API comes in two flavors - the (simple) Config API and the Configuration Entity API. The key difference is that the Config API is the singleton use case. A singleton is where there can be only a single instance of this configuration. A good example would be the site's name.

The **Configuration Entity API**, however, is used to store multiple sets of configuration - for example node types, views, vocabularies, and fields.\
In this [article](https://www.drupal.org/node/2120523#s-simple-configuration-vs-configuration-entities), you can have a more deep analysis of these differences.\
And if you want to [create a configuration entity type](https://www.drupal.org/node/1809494), check this page on drupal.org.\\

### What is configuration for?

Configuration is used for storing everything that has to be synchronized between different environments.

The typical flow is for the active site configuration to be synchronized with the one in the YAML files. This means importing all the configurations which are new or different in the YAML files into the database. These YAML files are inside the configuration sync folder, which should be committed to Git (you can configure in the `settings.php`, which directory should be the`sync` folder) and the opposite is to export the active configuration to the YAML files in order to commit them into code.

The UI allows you to compare the configuration uploaded to your sync directory with the active one on the database by providing you with a nice Diff interface.\
\
Then, you synchronize either in the UI as we've seen, or through Drush.\
\
You should check the configuration status regularly with the commands, bellow:\
`docker-compose exec web ./vendor/bin/drush config:status`

#### To export to sync directory

`docker-compose exec web ./vendor/bin/drush cex`<br>

#### To import from sync directory

`docker-compose exec web ./vendor/bin/drush cim`

#### To check differences

`docker-compose exec web ./vendor/bin/drupal config:diff`

### How is configuration stored

Configuration can be stored either in YAML files (the sync storage) or in the database (the active storage).

If a configuration is mandatory for our code to work, we can create it upon module installation.\
There are two ways this can be done:\
**Statically (most common way):** including YAML files in a **config/install** folder of the module, so that they get imported when the module is installed.\
**Dynamically (if the values we need to set in the configuration must be retrieved dynamically):** implementing a hook\_install(), so that we can try to get our value and create the configuration object containing it.

You can also provide configuration files with the module in a **config/optional** folder. In this case, these should only be imported if their dependencies are met. Meaning that if the dependencies of the configurations are not met (modules, themes and other configurations), the module will install correctly but without those configurations.

### How is configuration updated

If your module is making a [data model change related to configuration](https://www.drupal.org/node/2535316), then you need to properly update your data model. The four steps are:

1. Update your configuration schema YML file so that it reflects your new data model. This will make sure that people who install your module after you made the change will get the correct schema. See the [Configuration Schema documentation](https://www.drupal.org/node/1905070) for details on that.
2. Make sure any configuration items provided by your module in the config/install and config/optional directories have been updated, so that people installing your module will get correct configuration.
3. Write a hook\_update\_N() function. This will update configuration items for existing users of your module who already had it installed before you made the change so that they can continue to function. This is described below.
4. Write tests to ensure the code in your hook\_update\_N() correctly modifies the configuration. This will make sure that people who have an older version of your module installed can successfully update to the newer version. See [https://www.drupal.org/docs/8/api/update-api/writing-automated-update-tests-for-drupal-8](https://www.drupal.org/docs/8/api/update-api/writing-automated-update-tests-for-drupal-8) for details.

### Schema

Schema allows various systems to interact properly with the configuration items. They are a way to define the configuration items and specify what kind of data they store (strings, Booleans, integers and so on).\
The **schema** or structure for the image style configuration entity is defined in _core/modules/image/config/schema/image.schema.yml_.\
\
There are three main reasons why configuration needs a schema definition:\
\- Multilingual support\
\- Configuration entities\
\- Typecasting

More about configuration schema/metadata on this Drupal documentation [page](https://www.drupal.org/docs/drupal-apis/configuration-api/configuration-schemametadata).

### Configuration in code or database? Or?

The configuration is stored in the database, but to be organized and well described, it encoded in the YAML files. **These YAML files need to be imported, in order of its configuration to be used, because until then it's the database that holds the active configuration.**

So, to make things more dynamic, the configuration API also provides an override system by which you can override the active configuration, at various levels. There are three layers at which these overrides can be used: global, module and language.

**Global**\
\*\*\*\*The global override happens via the global $config variable. It's available in the settings.php file for site-wide overrides<mark style="color:red;">.</mark>\ \ **Module**\
\*\*\*\*With the modules override, we can create a service with the config.factory.override tag.\
In this service we, among other related things, handle overrides.\
\
**Language**\
\*\*\*\*If we enable configuration translation and add languages to our site, we can translate whatever configuration items that are described as translated by their schema. By doing this, we override the default configuration for a particular language. This override will be stored in the configuration storage and can be exported to YAML files.

We have three layers for configuration overrides by this exact order of priority: global, modules, and languages. Global overrides take precedence over everything else, while module overrides take precedence over language ones. This is why if we override a system configuration in a module, we cannot use language override in our code. So, this is important to keep in mind.

\
\
**Sources**\
Sipos, D. (2020). **Drupal 9 Module Development**: Get up and running with building powerful Drupal modules and applications (3rd Edition). Packt Publishing Ltd.\\\
[What-are-configuration-entities](https://drupalize.me/tutorial/what-are-configuration-entitie)\
[Simple ](https://www.drupal.org/node/2120523#s-simple-configuration-vs-configuration-entities)[configuration - vs - configuration-entities](https://www.drupal.org/node/2120523#s-simple-configuration-vs-configuration-entities)\
[Updating-configuration-in-drupal-8](https://www.drupal.org/docs/drupal-apis/update-api/updating-configuration-in-drupal-8)\
[Configuration override system](https://www.drupal.org/docs/drupal-apis/configuration-api/configuration-override-system)\
[Create a configuration entity type](https://www.drupal.org/node/1809494)
