---
description: Securing file permissions and ownership
---

# Permissions

[https://www.drupal.org/node/244924](https://www.drupal.org/node/244924)

```rust
sites -> 755
sites/default -> 755
sites/default/files -> 775
sites/default/settings.php -> 444
```

#### Check the Permissions Are Writeable <a href="#s-step-2-check-the-permissions-are-writeable" id="s-step-2-check-the-permissions-are-writeable"></a>

By default, the **sites/default** directory and the **settings.php** file should be writeable. You can check that the permissions of **sites/default** and **settings.php** are writeable by issuing the following commands:

```php
ls -al sites/ 
```

Permission on **sites/default** should be 755 \[drwxr-xr-x]:

```php
ls -al sites/default/settings.php 
```

Permission on **settings.php** should be 644 \[-rw-r--r--]:

If they are anything but writeable, you can issue the following commands:

```php
chmod 644 sites/default/settings.php 
```

#### Display octal notation of permissions with -ls

```
stat -c '%A %a %h %U %G %s %y %n' *
```

[https://unix.stackexchange.com/questions/76521/how-can-i-display-octal-notation-of-permissions-with-ls-and-can-octal-represen](https://unix.stackexchange.com/questions/76521/how-can-i-display-octal-notation-of-permissions-with-ls-and-can-octal-represen)
