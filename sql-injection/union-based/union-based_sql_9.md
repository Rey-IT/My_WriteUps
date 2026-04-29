Lab: SQL injection UNION attack, retrieving data from other tables

Difficult: PRACTITIONER

Link:  https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs. 

The database contains a different table called users, with columns called username and password. 

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user. 

2. Analytics

Let's go into the lab and look at the main page

![MAIN](/images/img-un-9/1.jpg)

We don't see anything interesting, so let's go to the product category and look there.

![Category](/images/img-un-9/2.jpg)

We notice that a category parameter appears in the URL, which is potentially vulnerable to SQLi.

![SQLi](/images/img-un-9/3.jpg)

Well, the category parameter is vulnerable, which means our task is to first determine the number of columns.

3. Info for exploitation

![Error](/images/img-un-9/4.jpg)

We checked using payload(' UNION SELECT NULL--) on 1 column and got an error, which means we will continue adding NULLs.

![More NULL](/images/img-un-9/5.jpg)

The site didn't return an error.

Now let's determine which of these fields are responsible for the text.

![Text Field](/images/img-un-9/6.jpg)

We found out that the 2nd parameter is text.

4. Explotation

According to the assignment, we already know that we need to find the administrator's username and password from the users table.
And this won't be difficult for us to do.

![admin](/images/img-un-9/7.jpg)

Well, we found out the admin password and let's try to log in.

![Success](/images/img-un-9/8.jpg)

We received a notification about the successful completion of the lab.