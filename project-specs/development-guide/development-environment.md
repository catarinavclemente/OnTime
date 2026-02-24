---
description: Local configurations
---

# Development environment

### Configure your development environment



In the case you need to inject something in your settings.php, then you should include it in your .runner.yml file at the "additional\_settings" block.\
\
**Step by step to develop and test with two different users**\
(IMPORTANT: Delete user 1 from the DB)

docker-compose exec -T web drush sql:query "SELECT \* FROM users WHERE uid = 1;"\
\
◦ Check if user 1 exists:\
docker-compose exec -T web drush uinf 1<br>

* Delete existing user records (if they exist):\
  docker-compose exec -T web drush sql:query "DELETE FROM users\_field\_data WHERE uid = 1 ; DELETE FROM users WHERE uid = 1;"<br>
* Generate password hash:\
  docker-compose exec -T web drush php:eval "echo \Drupal::service('password')->hash('123');" | tail -1<br>
* Insert user into database (replace $HASH with the hash from step 3):\
  docker-compose exec -T web drush sql:query "INSERT INTO users\_field\_data (uid, langcode, preferred\_langcode, preferred\_admin\_langcode, name, pass, mail, timezone, status, created, changed, access, login, init, default\_langcode) VALUES (1, 'en', 'en', 'en', 'catarina', '$HASH', 'duckfeatherscvc@duck.com', 'UTC', 1, UNIX\_TIMESTAMP(), UNIX\_TIMESTAMP(), 0, 0, 'duckfeatherscvc@duck.com', 1); INSERT INTO users (uid, uuid, langcode) VALUES (1, UUID(), 'en');"<br>

• Verify user was created:\
docker-compose exec -T web drush user:information catarina\
<br>
