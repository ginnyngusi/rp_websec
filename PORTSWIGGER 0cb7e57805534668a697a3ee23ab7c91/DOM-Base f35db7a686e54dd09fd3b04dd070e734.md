# DOM-Base

## What is the DOM?

The Document Object Model (DOM) is a web browser's 
hierarchical representation of the elements on the page. Websites can 
use JavaScript to manipulate the nodes and objects of the DOM, as well 
as their properties. DOM manipulation in itself is not a problem. In 
fact, it is an integral part of how modern websites work. However, 
JavaScript that handles data insecurely can enable various attacks. 
DOM-based vulnerabilities arise when a website contains JavaScript that 
takes an attacker-controllable value, known as a source, and passes it 
into a dangerous function, known as a sink.

## **[Lab: DOM-based open redirection](https://portswigger.net/web-security/dom-based/open-redirection/lab-dom-open-redirection)**

### Description:

> This lab contains a DOM-based open-redirection vulnerability. To solve this lab, exploit this vulnerability and redirect the victim to the exploit server.
> 

### Solution

On each of the posts pages, we notice at the end of the source code the following snipped. It is used to return to the home page.

![Untitled](DOM-Base%20f35db7a686e54dd09fd3b04dd070e734/Untitled.png)

 The url parameter is an open redirection. This allows us to change the “Back to Blog” link Let’s host the following payload in the exploit server. To solve the lab, we just have to construct the following payload with the correct links.

```jsx
https://0a23003803cefd75c2bb760e005400d8.web-security-academy.net/post?postId=4&url=https://exploit-0aa900c50389fdd2c243757001cd0099.exploit-server.net/
```

![Untitled](DOM-Base%20f35db7a686e54dd09fd3b04dd070e734/Untitled%201.png)