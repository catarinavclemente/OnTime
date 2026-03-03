---
description: Local configurations
---

# Development environment

### Dev settings

[In the case you need to inject something in your settings.php, then you should include it in your .runner.yml file at the "additional\_settings" block.](#user-content-fn-1)[^1]



&#x20;

***

**Step by step to develop and test with two different users**\
(IMPORTANT: Delete user 1 from the DB)\
\
◦ Check if user 1 exists:

`docker-compose exec -T web drush uinf 1`

Or:\
`docker-compose exec -T web drush sql:query "SELECT * FROM users WHERE uid = 1;"`\
\
**If it exists**\
Delete existing user records (if they exist):\
`docker-compose exec -T web drush sql:query "DELETE FROM users_field_data WHERE uid = 1 ; DELETE FROM users WHERE uid = 1;"`<br>



* Generate password hash:<br>

`docker-compose exec -T web drush php:eval "echo \Drupal::service('password')->hash('123');" | tail -1`<br>

* Insert user into database (replace $HASH with the hash from step 3):

```shellscript
dcdrush php:eval '$u = \Drupal\user\Entity\User::load(1); $u->setUsername("catarina"); $u->setEmail("catarinavclemente@gmail.com"); $u->save();'
```

• Verify user was created:\
\
`docker-compose exec -T web drush user:information catarina`\
<br>

[^1]: 
