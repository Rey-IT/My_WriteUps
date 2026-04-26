Lab: SQL injection UNION attack, finding a column containing text

Difficult: PRACTITIONER

Link:  https://portswigger.net/web-security/sql-injection/union-attacks/lab-find-column-containing-text

1. Lab Description:

This lab contains a SQL injection vulnerability in the product category filter. 

The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you first need to determine the number of columns returned by the query. You can do this using a technique you learned in a previous lab. The next step is to identify a column that is compatible with string data.

The lab will provide a random value that you need to make appear within the query results. To solve the lab, perform a SQL injection UNION attack that returns an additional row containing the value provided. This technique helps you determine which columns are compatible with string data.

2. Analytics

Let's go into the lab and look at the main page

![MAIN](/images/img-un-8/1.jpg)

We don't see anything interesting, so let's go to the product category and look there.

![Category](/images/img-un-8/2.jpg)

We notice that a category parameter appears in the URL, which is potentially vulnerable to SQLi.

![SQLi](/images/img-un-8/3.jpg)

Well, the category parameter is vulnerable, which means our task is to first determine the number of columns.

3. Exploitation

![Error](/images/img-un-8/4.jpg)

We checked using payload(' UNION SELECT NULL--) on 1 column and got an error, which means we will continue adding NULLs.

![More NULL](/images/img-un-8/5.jpg)

We see the same error for two Nulls, so we check further.

![NULLNULLNULL](/images/img-un-8/6.jpg)

Three NULLs worked, and the site didn't show us an error.

According to the assignment, we need to find where we can substitute text instead of NULL and display a specific string.

Let's start by determining which parameter is responsible for this.

![Text Field](/images/img-un-8/7.jpg)

We found out that the 2nd parameter is text, now we will simply output the specified string.

![Success](/images/img-un-8/8.jpg)

We output the given string and received a notification about the successful completion of the lab.


