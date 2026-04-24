Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data

Link: https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data

1. Lab Description: 

This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following: 
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products. 

2. Analytics

I went to the website and tried to view the details of category of the products.
![view on website](../images/img-un-1/1.jpg)

First, you need to intercept the GET request to understand where to start

![First look at the request](../images/img-un-1/2.jpg)

3. Exploitation

According to the task description, it is known that the parameter responsible for the product category is vulnerable to SQL injection. So, I'll try using a payload to test our theory.
' OR 1=1--

![Payload](../images/img-un-1/3.jpg)

![Success](../images/img-un-1/4.jpg)

Hooray, we were able to log in as admin and received a notification that the lab is solved.