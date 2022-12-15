# Web Socket

## WebSockets

`WebSockets` are widely used in modern web applications. They are initiated over HTTP and provide long-lived connections with asynchronous communication in both directions.

`WebSockets` are used for all kinds of purposes, including performing user actions and transmitting sensitive information. 
Virtually any web security vulnerability that arises with regular HTTP can also arise in relation to WebSockets communications.

[WebSockets](https://portswigger.net/web-security/websockets) are a bi-directional, full duplex communications protocol initiated over HTTP. They are commonly used in modern web applications for streaming data and other asynchronous traffic.

In this section, we'll explain the difference between HTTP and `WebSockets`, describe how WebSocket connections are established, and outline what WebSocket messages look like.

## **[Lab: Manipulating WebSocket messages to exploit vulnerabilities](https://portswigger.net/web-security/websockets/lab-manipulating-messages-to-exploit-vulnerabilities)**

### Description

> 
> 
> 
> This online shop has a live chat feature implemented using [WebSockets](https://portswigger.net/web-security/websockets).
> 
> Chat messages that you submit are viewed by a support agent in real time.
> 
> To solve the lab, use a WebSocket message to trigger an `alert()` popup in the support agent's browser.
> 

Go to lick "Live chat" and send a chat message.

![Untitled](Web%20Socket%207e4a250a00db4390a3be77b0e8a6f9f8/Untitled.png)

In the WebSockets history tab, and observe that the chat message has been sent via a WebSocket message.

![Untitled](Web%20Socket%207e4a250a00db4390a3be77b0e8a6f9f8/Untitled%201.png)