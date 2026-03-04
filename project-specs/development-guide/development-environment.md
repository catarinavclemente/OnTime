---
description: Local configurations
---

# Development environment

### Dev settings

[In the case you need to inject something in your settings.php, then you should include it in your .runner.yml file at the "additional\_settings" block.](#user-content-fn-1)[^1]



&#x20;

***

#### **Step by step to develop and test with two different users**<br>

**Create a new user**

`docker compose exec web drush user:create "Operational Tester" --mail="catarinavclemente@gmail.com" --password="123"`

**Update user 1**

`docker compose exec web drush sqlq "SELECT uid, name, mail FROM users_field_data WHERE uid = 1"`

[^1]: 
