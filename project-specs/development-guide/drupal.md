---
description: Project installation
---

# Drupal

Start by making sure you have the correct access rights to the remote GitHub repository.

Then, clone the GIT reference repo:\
`$ git clone git@github.com:ec-europa/<repository-name>.git`

Username and Passord for **Git** repo:\
Edit profile > Access tokens

**Add the 'VIRTUAL\_HOST' variable to your '.env.dist' file.**\
VIRTUAL\_HOST=[http://web:8080/web](http://web:8080/web)

**Set-up and run the environment with Docker Compose**

To run the containerized environment, you can follow these steps to set it up, using Docker Compose.

Run: `docker-compose up -d`

This will set up and run the environment. After spawning, please follow the set of commands specified in the documentation of a given component, site or a project.

**Create a docker-compose.override** file to add settings for existent services (ASDA credentials for web service) or to add entirely new services. **This file is never committed to the repository.**\
\
[https://github.com/ec-europa/just-ejustice-reference#14-installing-the-project](https://github.com/ec-europa/just-ejustice-reference#14-installing-the-project)

**When > 1 environment**

**df -h** - The ‘df‘ command stands for “**disk filesystem**“, it is used to get a full summary of available and used disk space usage of the file system on the Linux system.\
Using ‘`-h`‘ parameter with (df -h) will show the file system disk space statistics in “**human-readable**” format, which means it gives the details in bytes, megabytes, and gigabytes.\
\
**docker volume ls -qf dangling=true -** List only the names of dangling volumes in Docker.Let me break down what this command does:

* `docker volume ls`: Lists Docker volumes
* `-q` or `--quiet`: Only displays volume names (without additional information)
* `-f` or `--filter` with `dangling=true`: Filters to show only dangling volumes

Dangling volumes are those that are not referenced by any containers. These are volumes that were likely created for a container that has since been removed, or were created manually but never attached to any container.\
\
**docker volume prune --all -** Remove all unused local volumes. Unused local volumes are those which are not referenced by any containers. By default, it only removes anonymous volumes, but with option --all, pit removes both unused anonymous and named volumes.\
\
**docker stop $(docker ps -a -q)** - Stops all active containers\
\
**docker ps** - Shows all containners\
\
**docker container prune -f** - Remove all stopped containers and do not prompt for confirmation\
\
**docker image prune -a** - Remove all dangling images. If `-a` is specified, also remove all images not referenced by any container.\
\
**docker volume prune -a** - Remove all unused local volumes. Unused local volumes are those which are not referenced by any containers. With the `--all` flag to prune both unused anonymous and named volumes.

**docker system prune -a --volumes -** By default, volumes aren't removed to prevent important data from being deleted if there is currently no container using the volume. Use the `--volumes` flag when running the command to prune anonymous volumes as well:

```
This will remove:
        - all stopped containers
        - all networks not used by at least one container
        - all anonymous volumes not used by at least one container
        - all images without at least one container associated to them
        - all build cache
```

\
\
**docker network prune -a** - Remove all custom networks not used by at least one container.

\
