Lab: SQL injection attack, listing the database contents on non-Oracle databases

Difficult: PRACTITIONER

Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

2. Analytics

First, let's go into the lab and examine the page

![First view](../images/img-un-5/1.jpg)

After not finding anything interesting, we'll go to the product category and see that a category parameter is added to the URL, which is almost certainly vulnerable to SQL injection.

![Parameter](../images/img-un-5/2.jpg)

After determining that our parameter is vulnerable, let's try to perform a UNION-based SQL injection

To do this, let's find out the number of columns in the table:

' UNION SELECT NULL--

![UNION](../images/img-un-5/3.jpg)

It returned an error, so let's test for a larger number of columns:
' UNION SELECT NULL, NULL--

![NULL NULL](../images/img-un-5/4.jpg)

Well, now we know that there are two columns. We need to find out which of them are text-based.

![Text field](../images/img-un-5/5.jpg)

Both fields contain text. Now that we have all this information, we can start searching for the password.

3. Exploitation

First, we need to find out which table stores information about users

![Table of users](../images/img-un-5/6.jpg)

![Table](../images/img-un-5/7.jpg)

After finding out which table stores information about users, let's find out which fields this table contains

![Analyze users](../images/img-un-5/8.jpg)

We have found out the name of the column responsible for passwords. Now we simply query the database and ask it to output the password for the administrator user.

![Password](../images/img-un-5/9.jpg)

And so we have found out the administrator's password, and after trying to log in as admin, we will receive a notification that the lab has been successfully completed

![Success](../images/img-un-5/10.jpg)

