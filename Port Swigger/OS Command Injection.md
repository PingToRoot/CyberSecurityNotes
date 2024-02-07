# OS Command Injection

Also known as shell injection allows an attacker to execute OS commands on the server that is running an application, and typically fully compromise the application and its data. Often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure and exploit trust relationships to pivot the attack to other systems within the organisations.

## Userful Commands
```
Purpose - Linux/Windows
Name of user - whoami/whoami
OS - uname -a/ver
Network Config - ifconfig/ipconfig /all
Network Connection - netstat -an/netstat -an
Running Processes ps -ef/tasklist
``````

For example a URL to check stock may be ```https://webiste.com/stockStatus?ID=381&stockID=29``` would be a command line of ```stock report.pl 381 29``` so if you were to input the following ```& echo test &``` them command line would execute ```stockreport.pl & echo test & 29``` so the output would to the user would be;
```
Error - product ID was not provided
test
29:command not found
```

**LAB**

Modify storeID parameter to ```1|whoami```