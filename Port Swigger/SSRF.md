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