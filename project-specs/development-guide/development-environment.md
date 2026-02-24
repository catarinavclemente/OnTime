---
description: Local configurations
---

# Development environment

### Accounts

**Dev account**\
Username: <mark style="color:green;">catarina</mark>\
**Email: catarinavclemente@gmail.com**\
Status: Active\
Roles: authenticated



**Tests account**\
Username: n00lox3y\
Full name: [Catarina Clemente](http://localhost:8080/web/user/373?destination=/users)\
**Email: duckfeatherscvc@duck.com**\
Status: Active\
Roles: authenticated[<br>](http://localhost:8080/web/user/373/masquerade?token=aQQlKT-GvUbBOz2GIar5In08rNUosmlZP3xaO4rvN7I)\
once registered, if you want to test any other scenario (for example ETOH registration), you can masquerade as user 1 and delete the test account and re-register the test account in the local application to register for ETOH space

&#x20;

***

##

<br>



[In the case you need to inject something in your settings.php, then you should include it in your .runner.yml file at the "additional\_settings" block.](#user-content-fn-1)[^1]\
\
**Step by step to develop and test with two different users**\
(IMPORTANT: Delete user 1 from the DB)

docker-compose exec -T web drush sql:query "SELECT \* FROM users WHERE uid = 1;"\
\
◦ Check if user 1 exists:

`docker-compose exec -T web drush uinf 1`

* Delete existing user records (if they exist):

\
`docker-compose exec -T web drush sql:query "DELETE FROM users_field_data WHERE uid = 1 ; DELETE FROM users WHERE uid = 1;"`<br>

* Generate password hash:<br>

`docker-compose exec -T web drush php:eval "echo \Drupal::service('password')->hash('123');" | tail -1`<br>

* Insert user into database (replace $HASH with the hash from step 3):

\
`docker-compose exec -T web drush sql:query "INSERT INTO users_field_data (uid, langcode, preferred_langcode, preferred_admin_langcode, name, pass, mail, timezone, status, created, changed, access, login, init, default_langcode) VALUES (1, 'en', 'en', 'en', 'catarina', '$HASH', 'duckfeatherscvc@duck.com', 'UTC', 1, UNIX_TIMESTAMP(), UNIX_TIMESTAMP(), 0, 0, 'duckfeatherscvc@duck.com', 1); INSERT INTO users (uid, uuid, langcode) VALUES (1, UUID(), 'en');"`

• Verify user was created:\
\
`docker-compose exec -T web drush user:information catarina`\
<br>

[^1]: 
