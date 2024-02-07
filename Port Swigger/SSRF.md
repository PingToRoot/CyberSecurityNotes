# Server Side Request Forgery

SSRF allows an attacker to cause the server-side application to make requests to an unintended location.

Typically an attacker might cause the server to make a connection to internal-only services within the orgainisations infrastructure. In other cases, they may be able to force the server to connect to arbitrary external systems. This could leak sensitive data, such as auth credentials. For example, to provide stockinformation, a query to REST API must be made. It does this by passing the url to the relevent back-end API endpoint via a front-end HTTP request. For example:

```Post /product/stock HTTP/1.0
Content-Type:application/x-www-form
Content-Length:118

StockAPI=http://stock.net.8080/x-www/product/stock/check
```

An attacker can modify this StockAPI variable to:
```
StockAPI=http://localhost/admin
```
This makes the server fetch the contents of the /admin URL and returns it to the attacker.

An attacker can visit the ```/admin``` URL but only requests to the ```/admin``` URL from a local machine will allow access to admin functionality. So with this example, the application grants full acess because the request appears to originate from a trusted location.

There are multiple reasons that applications behave this way;

* The access control check might be implemented in a different component that sites in front of the application servcer. When a connection is made back to the server, the check is bypassed.
* For disaster recovery purposes, the application might allow administrative access without logging in to any user coming from the local machine. This provides a way for an administrator to recover the system if they lose their credentials. This assumes that only a fully trusted user would come directly from the server.
* The admin interface might tisten on a different port number to the main application, and might not be reachable directly by users.

These kind of trust relationships where the requests originating from the local machine are handled differently than ordinary requests, often makes SSRF into a critical vulnerability.

**LAB**

Browse to /admin > Blocked > Visit product > Initiate API > Change API key to ```http://localhost/admin``` > Displays current admin interface > Inspect page > Look at HTML tag for deleting user > Submit URL in API key > User is deleted

SSRF attacks can be made against other back-end systems. These systems often have non-routable private IP addresses.The back-end systems are usually protected by the network topology, so they often have a weaker security posture. In many cases, internal back-end systems contain sensitive functionality that can be accessed without authentification by anyone who is able to interact with the systems. For example, if an admin interface had the back-end URL of ```https://192.168.0.68/admin``` an attacker could submit the following;
```
Post /product/stock http/10
Content-Type: application
Content-length: 118

stockApi=http://192.168.0.68/admin
```


**LAB**

Check stock > Send to intruder > Change stock API parameter to ```http://ipaddress/admin``` > Highlight the final octet > Click "Add §" > Payload tab > Numeric > min:1; max:225; step:1 > Start attack > Look for 200 status code > Send to repeater > Change to ```/admin/delete?username=carlos```