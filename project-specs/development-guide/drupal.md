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

