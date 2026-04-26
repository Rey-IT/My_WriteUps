Lab: SQL injection UNION attack, determining the number of columns returned by the query

Difficult: PRACTITIONER

Link:  https://portswigger.net/web-security/sql-injection/union-attacks/lab-determine-number-of-columns

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. 

The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack. 

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values. 

2. Analytics

Let's go into the lab and see nothing remarkable.

![Main site](/images/img-un-7/1.jpg)

That means it's worth looking at the product categories and searching for something there.

![Category](/images/img-un-7/2.jpg)

We saw that a category parameter was added to the URL, which is potentially vulnerable to SQL injection. Let's try injecting a payload into the parameter to confirm our suspicions about SQLi.

![SQLi found](/images/img-un-7/3.jpg)

Well, SQLi really does exist.

3. Exploitation

The task states that when performing a UNION-based SQL injection, we need to determine how many columns are in the table, and we already know how to do this from previous tasks.

![NULL](/images/img-un-7/4.jpg)

if we see an error, we need to keep adding NULL until the site returns a 200 status

![Success](/images/img-un-7/5.jpg)

we received a notification that the lab was solved after entering three NULLs, which means that there are three columns in the database