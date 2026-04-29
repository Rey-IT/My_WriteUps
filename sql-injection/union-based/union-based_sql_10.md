Lab: SQL injection UNION attack, retrieving multiple values in a single column

Difficult: PRACTITIONER

Link:  https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-multiple-values-in-single-column

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables. 
The database contains a different table called users, with columns called username and password. 
To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user. 

2. Analytics

Let's go into the lab and see nothing remarkable.

![MAIN](/images/img-un-10/1.jpg)

That means it's worth looking at the product categories and searching for something there.

![Category](/images/img-un-10/2.jpg)

We saw that a category parameter was added to the URL, which is potentially vulnerable to SQL injection. Let's try injecting a payload into the parameter to confirm our suspicions about SQLi.

![SQLi](/images/img-un-10/3.jpg)

Well, SQLi really does exist.

3. Info for exploitation

Like all UNION-BASED tasks, we'll start by determining the number of columns.
Using the already-known payload,
' UNION SELECT NULL, …--'

![Error](/images/img-un-10/4.jpg)

We got an error, so we add another NULL.

![More NULL](/images/img-un-10/5.jpg)

The site didn't return an error.
Now let's determine which of these fields are responsible for the text.

![Text Field](/images/img-un-10/6.jpg)


4. Explotation

We saw that only one field contains text, which means we can't simply display the login and password in separate fields.
We need to somehow combine them into a single field.
The CONCAT function will help us with this.

![admin](/images/img-un-10/7.jpg)

Having found the administrator's login and password, we log in using his account and successfully complete this lab.