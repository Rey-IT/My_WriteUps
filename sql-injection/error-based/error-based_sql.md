Lab: Visible error-based SQL injection

1. Lab Description:

This lab contains a SQL injection vulnerability. The application uses a tracking cookie for analytics, and performs a SQL query containing the value of the submitted cookie. The results of the SQL query are not returned. 

The database contains a different table called users, with columns called username and password. To solve the lab, find a way to leak the password for the administrator user, then log in to their account. 


2. Analytics

I went to the website and tried to view the details of one of the products.
![view on website](/images/-1.jpg)

In Burp Suite, I intercepted a GET request and noticed some interesting request parameters that are potentially vulnerable to SQL injection. Specifically, I was interested in parameters such as productId and TrackingId in the cookies.

Before the session cookies, I assume this is a custom cookie that might be vulnerable.

![First look at the request](/images/0.jpg)

First, I'll try to check the trackingId.

3. Initialization of the vulnerability

I'll put a single quote at the end of the cookie to confirm that there is an SQL injection.

![Found parameter](/images/1.jpg)

And yes, we saw an error message that displayed the full SQL query for us:

Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = 'unRTfIWlLhSmV6hX''. Expected char

If after our single quote we put a --, we see that the server returns a 200 status. 

![Status 200](/images/2.jpg)

This means that the SQL injection works, and since the full error text is displayed to us, we can artificially create errors to view its content.

4. Start of exploitation

Let's try using the CAST() function to generate an error. The goal is to create errors on our own in order to obtain the full error text.

![Create error](/images/3.jpg)

Now it is clear that we can create errors on our own using CAST(). This is evident because the server returned a full error explanation stating that the argument must be a boolean, not an integer.

Now let's try to retrieve the list of users.

![Limit of symbols](/images/4.jpg)

It returned an unterminated string error. Our query was simply truncated, which means we need to figure out how to fit our query within this character limit. 

The idea immediately comes to mind that we might be able to remove the custom cookie from our query since it doesn't carry any importance.

![Delete cookies](/images/5.jpg)

Well, we saw that after removing the custom cookie from the TrackingId parameter, the error text changed. By reading it, we understand that our query returns more than one row. 

That means we should limit the output to a single row (retrieve one record from the table)

![First user](/images/6.jpg)

In the error text, we saw the very first record from the users table with the value 'administrator'. 
This suggests that in the same way, we can now also retrieve the password for the admin account.

![Admin info](/images/7.jpg)

As expected, we were able to find out the admin's password. Thanks to the fact that our expression cannot be converted to an int type.

Let's try to log in as admin with the credentials we now know.

![Success](/images/8.jpg)

And yes, we received a notification that the lab is solved.