# Installation

## Environment

{% hint style="info" %}
**Good to know:** Before following the instructions provided on toolkit, be sure to have composer installed in your environment
{% endhint %}

### Creating the AWS Cloud 9 environment

Request a new Cloud 9 instance\
Create your Cloud 9 instance:

* Name your environment with your IAM username.
* Instance type is limited : "t3.medium"
* Your environment must be created in "eu-west-1" region (Ireland)
* Platform must be "Amazon Linux 2"
* Be careful, "m4.medium" is listed, but not available, for "medium" instance type you have to select "Other instance type"

#### Initialize

After creating the environment, it is compulsory to initialise it with the script provided by DevOps:\
`aws s3 cp s3://c9-install-scripts/install-salt.sh - | bash`

{% hint style="info" %}
You need to have the following software installed on your local development environment: [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git), [Docker](https://docs.docker.com/install/) and [Docker Compose](https://docs.docker.com/compose/install/)
{% endhint %}

Source: [https://webgate.ec.europa.eu/fpfis/wikis/display/MULTISITE/AWS+Cloud9](https://webgate.ec.europa.eu/fpfis/wikis/display/MULTISITE/AWS+Cloud9)

### Git, Docker and Docker Compose <a href="#id-4.configurec9-dockerprofile" id="id-4.configurec9-dockerprofile"></a>

#### Configure Docker profile <a href="#id-4.configurec9-dockerprofile" id="id-4.configurec9-dockerprofile"></a>

sudo salt-call state.apply profiles.docker

This [page](https://webgate.ec.europa.eu/fpfis/wikis/display/MULTISITE/AWS+Cloud9+-+Docker) explains how to use docker for running web server and all needed services for website development.

```
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
php -r "if (hash_file('sha384', 'composer-setup.php') === '906a84df04cea2aa72f40b5f787e49f22d4c2f19492ac310e8cba5b96ac8b64115ac402c8cd292b8a03482574915d1a8') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
php composer-setup.php --version=1.9.0
sudo mv composer.phar /usr/local/bin/composer
```

Source: [https://webgate.ec.europa.eu/fpfis/wikis/display/MULTISITE/4.+Configure+C9](https://webgate.ec.europa.eu/fpfis/wikis/display/MULTISITE/4.+Configure+C9)



### Setting up a project

To install locally a project running Toolkit 4 you should run the following commands:

Start by cloning GitLab dev repo:

#### Setting up the environment

By default, docker-compose reads two files, a `docker-compose.yml` and an optional `docker-compose.override.yml` file. **By convention, the `docker-compose.yml` contains your base configuration and it is committed to the repository.** This file contains a webserver, a mysql server and a selenium server. It very closely matches the environment the website is deployed on.

**Create a docker-compose.override** file to add settings for existent services (ASDA credentials for web service) or to add entirely new services. **This file is never committed to the repository.**

Check if composer.json has the correct requirements.\
And then:

```
docker-compose up -d
docker-compose exec web composer install
docker-compose exec web ./vendor/bin/run toolkit:build-dev
```

If it's a fresh install:

```
docker-compose exec web ./vendor/bin/run toolkit:download-dump
```

Then:

```
docker-compose exec web ./vendor/bin/run toolkit:install-clone
```

### Git configuration

#### Aliases

Aliases are stored in \~/.gitconfig.\
\
Now you’ll learn a few of the more interesting options that you can set in this manner to customize your Git usage.

First, a quick review: Git uses a series of configuration files to determine non-default behavior that you may want. The first place Git looks for these values is in the system-wide `[path]/etc/gitconfig` file, which contains settings that are applied to every user on the system and all of their repositories. If you pass the option `--system` to `git config`, it reads and writes from this file specifically.

The next place Git looks is the `~/.gitconfig` (or `~/.config/git/config`) file, which is specific to each user. You can make Git read and write to this file by passing the `--global` option.

Finally, Git looks for configuration values in the configuration file in the Git directory (`.git/config`) of whatever repository you’re currently using. These values are specific to that single repository, and represent passing the `--local` option to `git config`. If you don’t specify which level you want to work with, this is the default.

Each of these “levels” (system, global, local) overwrites values in the previous level, so values in `.git/config` trump those in `[path]/etc/gitconfig`, for instance.\\

### Deployment in GitLab

### Routine

`docker-compose up -d`\
Starts the containers in the background and leaves them running.\
\
Check the state of the working directory and the staging area\
Check configurations status:\
`./vendor/bin/drush config:status`\\
