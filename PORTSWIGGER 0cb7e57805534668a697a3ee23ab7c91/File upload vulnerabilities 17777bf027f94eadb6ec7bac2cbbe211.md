# File upload vulnerabilities

## What are file upload vulnerabilities?

File upload vulnerabilities are when a web server allows users to upload files to its filesystem without sufficiently validating things like their name, type, contents, or size. Failing to properly 
enforce restrictions on these could mean that even a basic image upload function can be used to upload arbitrary and potentially dangerous files instead. This could even include server-side script files that enable remote code execution.

In some cases, the act of uploading the file is in itself enough to cause damage. Other attacks may involve a follow-up HTTP request for the file, typically to trigger its execution by the server.

# **[Lab: Web shell upload via obfuscated file extension](https://portswigger.net/web-security/file-upload/lab-file-upload-web-shell-upload-via-obfuscated-file-extension)**

### Description

> This lab contains a vulnerable image upload function. 
Certain file extensions are blacklisted, but this defense can be bypassed using a classic obfuscation technique.
> 
> 
> To solve the lab, upload a basic PHP web shell, then use it to exfiltrate the contents of the file `/home/carlos/secret`. Submit this secret using the button provided in the lab banner.
> 
> You can log in to your own account using the following credentials: `wiener:peter`
> 

![                                                                           Logon with credentials](File%20upload%20vulnerabilities%2017777bf027f94eadb6ec7bac2cbbe211/Untitled.png)

                                                                           Logon with credentials

First, trying up a shell PHP via upload avatar feature 

```php
<?php echo system($_GET['command']); ?>
```

![                                                                              Error message](File%20upload%20vulnerabilities%2017777bf027f94eadb6ec7bac2cbbe211/Untitled%201.png)

                                                                              Error message

This time, the whitelist option was used (only **JPG** and **PNG** allowed) so obviously I was outright rejected. 

However, it should be noted that the **whitelist** tactic will face another painful problem that is the popular camouflage tricks as follows:

 - Use **multiple extensions** like “**php.jpg**” to disguise the image file extension;

 - Use **trailing characters** like **whitespace (' ')**, **dot ('.')** like ***'`php`.'*** to mask the identity (`php`) and show the original form only when the application handles the removal of the trailing character;

 - Use **URL encoding** (or **double URL encoding**) for **`dot` (’.’)**, **`forward slash` (’/’), `backward slashe` (”\”)** 

 - Use a **semicolon (';')** or **URL encoded null** **byte character** in front of the file extension to exploit the inconsistency of file extension verification and subsequent server processing (i.e. verification and processing steps). then 'read' two different file extensions from the same input).

I jumped over to **Burp Suite Proxy History** to get the corresponding request and push it through the **Repeater**.

![                                                                             File upload whitelist request](File%20upload%20vulnerabilities%2017777bf027f94eadb6ec7bac2cbbe211/Untitled%202.png)

                                                                             File upload whitelist request

Then, I found the `shell.php` file location and changed the **filename** to something like: **filename='exploit.php%00.jpg'** (using the URL encode null byte **%00** as I said above).

At this point, the `.jpg` helps me break through the whitelist protection to push the goods to the server. After going through the verification step with the `whitelist`, in the next processing, when I hit the `null byte encode URL`, the server immediately cut the `null byte` and the `.jpg` behind and 'left' for me the file `shell.php` as expected.

![                                                                    Obfuscated file upload request
](File%20upload%20vulnerabilities%2017777bf027f94eadb6ec7bac2cbbe211/Untitled%203.png)

                                                                    Obfuscated file upload request

![                                                                                    Exploit](File%20upload%20vulnerabilities%2017777bf027f94eadb6ec7bac2cbbe211/Untitled%204.png)

                                                                                    Exploit