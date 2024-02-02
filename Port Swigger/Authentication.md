# Authentication

Can allow attackers to gain access to sensitive data and functionality. They expose additional attack surface for further exploits

### Brute forcing

This is not just about brute forcing passwords but also usernames. Admin/Administrator are very common but you can find usernames without logging in by checking the HTTP responses for any disclosed emails.

Username enumeration is to observe the changes in the website behaivour to identify whether a given username is valid such as status code, error messages or repons time.

### Lab Notes
Submit invalid username + password > HTTP history > POST/login request > highlight the value of username in the request > send to burp intruder > replace the user name parameter with §test§ > payloads > paste word list > start attack > check status code/length for correct username > change password to static and add §test§ to password > repeat for password

### Bypassing 2FA

You can sometimes skip from a logged in state with a 2FA block to a logged in only page