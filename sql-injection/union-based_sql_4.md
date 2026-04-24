Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft

Difficult: PRACTITIONER

Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-mysql-microsoft

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.

To solve the lab, display the database version string.

2. Analytics

After entering and examining it, we don't see anything interesting on the main page yet. So let's go to some product category and look there.

![main site](../images/img-un-4/1.jpg)

We saw that a category parameter appears in the URL, which is almost certainly vulnerable to SQL injection.

![Parameter](../images/img-un-4/2.jpg)

3. Exploitation

Solving the lab is quite simple if you know all the necessary theory.

We can retrieve the version with a single command, namely:

'%20UNION%20SELECT%20@@version,%20'aaa'%23

This will work because we already know the number of columns in the table by using NULL, and we have also identified the text-based columns. 

Now it's just a matter of small details: knowing that our database is MySQL, we know that the version can be retrieved using @@version, and comments in MySQL use the # symbol. 
We replace spaces with %20 and the # symbol with %23.

![Success](../images/img-un-4/5.jpg)

And we see that we are congratulated on successfully completing the lab.