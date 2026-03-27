# TEST

TESTER login\
\
user: n00f54j3\
email: catarinavclemente@gmail.com\
pw: 123\
\
\
ADMIN login

**1. Find user 1**

dcdrush sqlq "SELECT uid, name, mail, status FROM users\_field\_data WHERE uid = 1"

**2. Rename to admin**

dcdrush sqlq "UPDATE users\_field\_data SET name = 'admin' WHERE uid = 1"

**3. Unblock**

dcdrush sqlq "UPDATE users\_field\_data SET status = 1 WHERE uid = 1"

**4. Set password**

dcdrush user:password admin 123

**5. Clear cache**

dcdrush cr
