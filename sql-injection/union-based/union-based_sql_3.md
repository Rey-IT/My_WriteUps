Lab: SQL injection attack, querying the database type and version on Oracle

Link: https://portswigger.net/web-security/sql-injection/examining-the-database/lab-querying-database-version-oracle

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. You can use a UNION attack to retrieve the results from an injected query.
To solve the lab, display the database version string.

2. Analytics

Let's go into the lab and see nothing remarkable.
![Main site](/images/img-un-3/1.jpg)

That means it's worth looking at the product categories and searching for something there.

![Category](/images/img-un-3/2.jpg)

We saw that a category parameter was added to the URL, which is potentially vulnerable to SQL injection. Let's try injecting a payload into the parameter to confirm our suspicions about SQLi.

![SQLi found](/images/img-un-3/3.jpg)

Well, SQLi really does exist. Now we need to figure out how to retrieve the database version.

3. Exploitation

We know that the database is Oracle, so the payload will look like this:

' UNION SELECT NULL FROM dual--

![Find 200status](/images/img-un-3/4.jpg)

FROM dual is fundamentally necessary for Oracle databases because the database requires that data be selected from somewhere. 

The dual table works perfectly for this since it is a system table. 

We are also using NULL to check the number of columns and will wait until the server stops returning errors

![200](/images/img-un-3/5.jpg)

We have determined that there are two columns in the table. Now let's figure out which of them store text.

![Text field](/images/img-un-3/6.jpg)

So we found out that there are 2 columns in the database, and both contain text. Now let's try to retrieve the database version.

![Success](/images/img-un-3/7.jpg)

Well, our payload worked and we were able to retrieve the version. 
That's what the notification about successfully completing the lab tells us.