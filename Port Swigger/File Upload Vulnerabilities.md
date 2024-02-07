# File Upload Vulnerabilities

File upload vulnerabilities is when a webserver allows a user to upload files to its file system without sufficiently validating things like their name, type, contents, or size. Failing to properly enforce restrictions on these could mean that even a basic image upload duncrion can be used to upload arbitary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution. In some cases the act of uploading the file is in itself enough damage caused. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server. Though, it is rare for websites in the wild to have no restrictions e.g. fail to account for parsing discrepancies when checking the file extensions or the website may attempt to check the file type by verifying properties that can be easily manipulated by an attacker using Burp Suite Proxy/Repeater.

Exploiting this can be useful to get a web shell. This means you are able to read/write files or even use it to pivot attacks against both internal infrastructure and other servers outside the network. For example ```<?php echo file_get_contents('/path/of/file');?>``` will read arbitary files from the servers file system. Once uploaded, sending a request for this malicious file will return the targets files contents in the response. A more versatile web shell may look like ```<?php echo system($_Get['command'];)?>``` this script enables you to pass an arbitrary system command via a query parameter with ```GET /example/exploit.php?command=id HTTP/1.1```

**LAB**

Burp Suite > Proxy > Login > Upload avatar > Avatar is shown so there is a get request > Go to http history > Filter settings > Enable image > Send GET request to repeater > Upload file exploit.php containing ```<?php echo file_get_contents('/home/carlos/secret');?>``` > Change the image location to exploit.php > Retreive response

This is unlikey to be seen in the wild but just because there are some defences in place does not mean that it is not exploitable.

### Flawed file type validation

This is when submitting HTML forms, the browser typically sends the provided data in a POST request with the content type ```application/x-www-form-encoded``` this is fine for sending simple text like your name/address. However, it isn't suitable for sending large amounts of binary data. Such as an entire image file or a PDF document.

In this case, the content type ```multipart/form-data``` is preferred. Consider a form containing fields for uploading an image, providing a description, and entering your username. The body of the message will be split into seperate parts for each of the forms inputs, these will be using a ```Content-Disposition``` header which provides basic information. These individual parts may also contain their own ```Content-Type``` header which tells the server the MIME type of the data that was submitted using the inputs.

One way websites may attempt to validate file uploads is to check that this input-specific ```Content-Type``` header matches an expected MIME type. If the server is only expecting imagine type, problems can arise when the value of this header is implicitly trusted by the server. If no further validation is performed to check whether the contents of the file actually match the supposed MIME type, this defence can be easily bypassed using tools like Burp Repeater.

**LAB**

Follow the previous lab but change the content-type of exploit to ```image/png```