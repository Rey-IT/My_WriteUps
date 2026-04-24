Lab: SQL injection vulnerability allowing login bypass

Link: https://portswigger.net/web-security/sql-injection/lab-login-bypass

1. Lab Description:

This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the administrator user.

2. Exploitation

Let's take a look at the initial data entry window.

![LogIn](../images/img-un-2/1.jpg)

It is enough to go to the login and password entry window and inject our payload into the SQL query in order to trick the site and log in as an admin. 
That is, we modify the query so that the password is not checked.

![Modify](../images/img-un-2/2.jpg)

The -- symbol allows commenting out code in SQL, so the password check simply gets commented out. This allows us to enter anything in the password field and still log in as an admin.

administrator'--

![Success](../images/img-un-2/3.jpg)

Hooray, we were able to log in as admin and received a notification that the lab is solved.