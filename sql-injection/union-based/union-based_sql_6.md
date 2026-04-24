Lab:  SQL injection attack, listing the database contents on Oracle

Difficult: PRACTITIONER

Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-listing-database-contents-non-oracle

1. Lab Description:

his lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables. 

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users. 

To solve the lab, log in as the administrator user. 

2. Analytics

Let's go into the lab and see nothing remarkable.

![Main site](../images/img-un-6/1.jpg)

That means it's worth looking at the product categories and searching for something there.

![Category](../images/img-un-6/2.jpg)

We saw that a category parameter was added to the URL, which is potentially vulnerable to SQL injection. Let's try injecting a payload into the parameter to confirm our suspicions about SQLi.

![SQLi found](../images/img-un-6/3.jpg)

Well, SQLi really does exist.

3. Info before exploitation

We know that the database is Oracle, so the payload will look like this:

' UNION SELECT NULL FROM dual--

![Find 200status](../images/img-un-6/4.jpg)

FROM dual is fundamentally necessary for Oracle databases because the database requires that data be selected from somewhere. 

The dual table works perfectly for this since it is a system table. 

We are also using NULL to check the number of columns and will wait until the server stops returning errors

![200](../images/img-un-6/5.jpg)

We have determined that there are two columns in the table. Now let's figure out which of them store text.

![Text field](../images/img-un-6/6.jpg)

So we found out that there are 2 columns in the database, and both contain text. Now let's try to retrieve the database version.

4. Exploitation

First, we need to find out which table stores information about users

![Table of users](../images/img-un-6/7.jpg)

![Table](../images/img-un-6/8.jpg)

After finding out which table stores information about users, let's find out which fields this table contains

![Analyze users](../images/img-un-6/9.jpg)

We have found out the name of the column responsible for passwords. Now we simply query the database and ask it to output the password for the administrator user.

![Password](../images/img-un-6/10.jpg)

And so we have found out the administrator's password, and after trying to log in as admin, we will receive a notification that the lab has been successfully completed

![Success](../images/img-un-6/11.jpg)