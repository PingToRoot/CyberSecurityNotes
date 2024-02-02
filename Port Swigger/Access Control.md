# Access Control

Storing admins URLs within the JavaScript of a webpage, in /admin or in robots.txt.

Parameter based access is where the some applications determine the users access rights or roles at login, then store this in a user-controllable location e.g.

* A hidden field
* A cookie
* A preset query string parameter

For example: ```https://insecurewebsite.com/login/home.jsp?admin=true``` using burp, turn on intercept and enable response interception. Submit login page, change Admin=False to Admin=True in the http request.

Horizontal privilege escaltion occurs if a user is able to gain access to resources belonging to another user, instead of their own of that type e.g. ```http://insecurewebsite.com/account?id=123```

Horizontal to vertical privilege escalation occurs when a horizontal privilege attack is turned into a vertical privilege escalation.