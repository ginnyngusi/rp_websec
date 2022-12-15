# Command Injection

## What is **OS command injection?**

> OS command injection (also know as shell injection) is a web security vulnerability that allows an attacker to execute arbitrary operating system (OS) commands on the server that is running an application, and typically fully compromise the application and all its data. Very often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, exploiting trust relationships to pivot the attack to other systems within the organization.
> 

## **Blind OS command injection**

Many cases of OS command injection are blind vulnerabilities. This means that the output will not return in the response and the output will not be visible on the screen (aka a stealth hole).

There are several techniques that can be used to determine the blind OS command as follows:

 - **Detecting blind OS command injection using time delays**

 - **Exploiting blind OS command injection by redirecting output**

 - **Exploiting blind OS command injection using out-of-band (OAST) techniques**

## **[Lab: Blind OS command injection with time delays](https://portswigger.net/web-security/os-command-injection/lab-blind-time-delays)**

### Description

> This lab contains a blind [OS command injection](https://portswigger.net/web-security/os-command-injection) vulnerability in the feedback function.
> 
> 
> The application executes a shell command containing the 
> user-supplied details. The output from the command is not returned in 
> the response.
> 
> To solve the lab, exploit the blind OS [command injection](https://portswigger.net/web-security/os-command-injection) vulnerability to cause a 10 second delay.
> 

Command delimiters work on both Windows and Unix systems:

- `&`
- `&&`
- `|`
- `||`

The following command delimiters only work on Unix-based systems:

- `;`
- `Newline (0x0a or`
- `)`
- ```
- `$`

### Solution:

Go to the feedback page and fill in the fields in the form as you like

![                                                                              Submit feedback ](Command%20Injection%2033c15db78f794538a5bda0ab38980289/Untitled.png)

                                                                              Submit feedback 

Request captured from Burp Suite will look like this:

![                                                                     POST Request in Burp Suite](Command%20Injection%2033c15db78f794538a5bda0ab38980289/Untitled%201.png)

                                                                     POST Request in Burp Suite

This time, if you execute the command like the above lab, it will not display anything, we will assume this is a blink OS command and perform a test.

Perform the inject of the delimiters `|`  in turn into each param to know where the code executes the error.

All submit successfully until the **email** is: 

![                                                                             Inject `|` in email param](Command%20Injection%2033c15db78f794538a5bda0ab38980289/Untitled%202.png)

                                                                             Inject `|` in email param

 OK, I know where this **error** is, so I will use the `sleep` command with the `||`  character here to execute the sleep command: 

![                                                                            Execute sleep command ](Command%20Injection%2033c15db78f794538a5bda0ab38980289/Untitled%203.png)

                                                                            Execute sleep command 

![                                                                              Result after 10s delay](Command%20Injection%2033c15db78f794538a5bda0ab38980289/Untitled%204.png)

                                                                              Result after 10s delay