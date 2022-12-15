# Directory Traversal

# **What is directory traversal?**

> Directory traversal (also known as file path traversal) is a web security vulnerability that allows an attacker to read arbitrary files on the server that is running an application. This might include application code and data, credentials for back-end systems, and sensitive operating system files. In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.
> 

# **[Lab: File path traversal, validation of start of path](https://portswigger.net/web-security/file-path-traversal/lab-validate-start-of-path)**

### Description

> This lab contains a [file path traversal](https://portswigger.net/web-security/file-path-traversal) vulnerability in the display of product images.
> 
> 
> The application transmits the full file path via a request 
> parameter, and validates that the supplied path starts with the expected
> folder.
> 
> To solve the lab, retrieve the contents of the `/etc/passwd` file.
> 

![                                            GET request in Repeater with productID parameter ](Directory%20Traversal%20f7bbc05cd55f4c83807590dabaf22347/Untitled.png)

                                            GET request in Repeater with productID parameter 

![                                            GET request in Repeater with filename parameter ](Directory%20Traversal%20f7bbc05cd55f4c83807590dabaf22347/Untitled%201.png)

                                            GET request in Repeater with filename parameter 

The filename parameter in this case is a full path.

Server only accepts filename parameter values that begin with `/var/www/images/`, otherwise an error message will be returned:

![                                                  Error message  "Missing parameter 'filename'”](Directory%20Traversal%20f7bbc05cd55f4c83807590dabaf22347/Untitled%202.png)

                                                  Error message  "Missing parameter 'filename'”

But, when the filename parameter begins with `/var/www/images/`, the system searches for the corresponding file:

![                                                          Error message  "No such file”](Directory%20Traversal%20f7bbc05cd55f4c83807590dabaf22347/Untitled%203.png)

                                                          Error message  "No such file”

So, we just need to build the payload starting with `/var/www/images/`, then use the directory traversal technique to point to the `/etc/passwd` file:

`filename=/var/www/images/../../../etc/passwd`

![                                                      Directory traversal technique with full path](Directory%20Traversal%20f7bbc05cd55f4c83807590dabaf22347/Untitled%204.png)

                                                      Directory traversal technique with full path