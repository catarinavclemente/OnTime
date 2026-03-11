# Commands

**Find**

```
find . -name "filename"
find . -name "*.js"
```



**Source alias**

```
source ./.bash-aliases
```

**User 1**

```
drush sqlq "SELECT uid, name FROM users_field_data WHERE uid = 1;"
```

```
dcdrush sqlq "UPDATE users_field_data SET name = 'admin-catarina', mail = 'catarinavclemente@gmail.com' WHERE uid = 1;"
```

```
dcdrush user:password admin-catarina '123'
```
