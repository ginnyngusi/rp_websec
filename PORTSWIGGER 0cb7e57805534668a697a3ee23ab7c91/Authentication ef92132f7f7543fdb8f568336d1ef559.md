# Authentication

## What is authentication?

> Authentication is the process of verifying the identity of given user or client. Because websites  are exposed to anyone who  is connected to the internet by design. Therefore, robust authentication mechanisms are an integral aspect of effective web security
> 

There are three authentication factors into whish different types of authentication can be categorized: 

 - Something you know (password, the answer to a security question) ⇒ knowledge factors

 - Something you have (a physical object like a mobile phone or security token) ⇒ possession factors

 - Something you are or do (your biometrics or patterns of behavior) ⇒ inherence factors

# **[Lab: Password reset poisoning via middleware](https://portswigger.net/web-security/authentication/other-mechanisms/lab-password-reset-poisoning-via-middleware)**

### Description

> This lab is vulnerable to password reset poisoning. The user `carlos` will carelessly click on any links in emails that he receives. To solve the lab, log in to Carlos's account.
You can log in to your own account using the following credentials: `wiener:peter`. Any emails sent to this account can be read via the email client on the exploit server.
> 

In fact, it is not uncommon for users to lose/forget their login password, so most systems have a plan to allow users to reset their password. And some common ways:

 - The system generates a temporary password with a short duration of use and the user is required to change the password as soon as he logs back into the system;

 - The system sends a password reset link to the user's registered email to complete the process of setting up a new password.

### Target exploration

Turn on Burp Suite Intercept and access the target's login location. I tested the password reset function through the ***Forgot password?*** and then enter the username as follows: 

![                                                   Forget password form with user `wiener`
](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled.png)

                                                   Forget password form with user `wiener`

Next, I access the Exploit Server (set up with the Lab to support the testing process), then scroll down to find and access the Email Client. Here, I can see the password reset link containing the related reset token.

![                                                                              Exploit server 

](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%201.png)

                                                                              Exploit server 

![                                                               Reset password link in email client 
](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%202.png)

                                                               Reset password link in email client 

Having confirmed the password reset mechanism, I went back to the ***Burp Suite History***, found and pushed the ***POST /forgot-password*** (corresponding to the request I entered my username and sent to receive the reset link via email) to the ***Burp Suite Repeater*** .

![                                                               Post request in  HTTP History  
](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%203.png)

                                                               Post request in  HTTP History  

![                                                                 Post request in Repeater 
](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%204.png)

                                                                 Post request in Repeater 

With this request, I'm going to test if I can use the ***X-Forwarded-Host header***. After confirming, I will be able to set it up to point reset links to arbitrary domains (specifically, **Exploit Server** here) to entice victims to click and then send tokens back. 

To do this, I need to access the Exploit Server again to record the Exploit Server URL (URL: [https://exploit-0a56000a043a4268c12566e501920054.exploit-server.net](https://exploit-0a56000a043a4268c12566e501920054.exploit-server.net/)[/exploit](https://exploit-ac751f101fd77565c03e344901d10047.web-security-academy.net/exploit)). And then update the corresponding value for X-Forwarded-Host header: **X-Forwarded-Host: [exploit-0a56000a043a4268c12566e501920054.exploit-server.net](https://exploit-0a56000a043a4268c12566e501920054.exploit-server.net/)[.](https://exploit-0a56000a043a4268c12566e501920054.exploit-server.net)**

Already prepared, I went back to the **Burp Suite Repeater**, changed the username to the corresponding value of the victim (**carlos**), inserted the prepared **X-Forwarded-Host** and **Send**.

![                                               POST forgot-password with X-Fowarded-Host](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%205.png)

                                               POST forgot-password with X-Fowarded-Host

At this time, reset password link, will be sent to the email of victim **carlos**. Once the victim innocently clicks on the link, I will be able to access the **Exploit Server** and check the **access log** for the **GET /forgot-password request** (corresponding to a **different IP**) containing the victim's token: `FSnWf4z2hUShWGIKN49moJEzlOFLgbyz`

![                                                                              Access log](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%206.png)

                                                                              Access log

Returning to the Email Client, I get reset link password in the output and reset it by changing the token to the value obtained in the Exploit Server access log: 

[`https://0a330053045e4246c125670400560016.web-security-academy.net/forgot-password?temp-forgot-password-token=FSnWf4z2hUShWGIKN49moJEzlOFLgbyz`](https://0a330053045e4246c125670400560016.web-security-academy.net/forgot-password?temp-forgot-password-token=FSnWf4z2hUShWGIKN49moJEzlOFLgbyz)

Reset URL and chance password of victim 

![                                                                       Reset password form](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%207.png)

                                                                       Reset password form

![                                                               Login form with account carlos ](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%208.png)

                                                               Login form with account carlos 

![                                                                                    Result](Authentication%20ef92132f7f7543fdb8f568336d1ef559/Untitled%209.png)

                                                                                    Result