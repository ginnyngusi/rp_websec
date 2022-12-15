# Information disclosure

## **What is information disclosure?**

Information disclosure, also known as information leakage, 
is when a website unintentionally reveals sensitive information to its 
users. Depending on the context, websites may leak all kinds of 
information to a potential attacker, including:

- Data about other users, such as usernames or financial information
- Sensitive commercial or business data
- Technical details about the website and its infrastructure

## [Lab: **Information disclosure on debug page**](https://portswigger.net/web-security/information-disclosure/exploiting/lab-infoleak-on-debug-page)

### Description:

> This lab contains a debug page that discloses sensitive information about the application. To solve the lab, obtain and submit the `SECRET_KEY` environment variable.
> 

### Solution:

Initially checking the functions, pages as well as params, I didn't see anything suspicious about this site, however, when I looked at the source, I saw this line: 

![Untitled](Information%20disclosure%20d6512bb634c54855a649ef5bdb116ea1/Untitled.png)

When accessing, try the above link: 

![Untitled](Information%20disclosure%20d6512bb634c54855a649ef5bdb116ea1/Untitled%201.png)

Information about the server has been leaked, now just find the SECRET_KEY environment variable, there is a lot of leaked information, including information to debug, information about the server, version, …

![Untitled](Information%20disclosure%20d6512bb634c54855a649ef5bdb116ea1/Untitled%202.png)