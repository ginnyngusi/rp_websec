# CSRF

## What is CRSF?

> Cross-site request forgery (also known as CSRF) is a web security vulnerability that allows an attacker to induce users to perform actions that they do not intend to perform. It allows an attacker to partly circumvent the same origin policy, which is designed to prevent different websites from interfering with each other.
> 

## [LAB:  **CSRF where token is tied to non-session cookie**](https://portswigger.net/web-security/csrf/lab-token-tied-to-non-session-cookie)

### Description

This lab's email change functionality is vulnerable to CSRF.
 It uses tokens to try to prevent CSRF attacks, but they aren't fully 
integrated into the site's session handling system.

To solve the lab, use your exploit server to host an HTML page that uses a [CSRF attack](https://portswigger.net/web-security/csrf) to change the viewer's email address.

You have two accounts on the application that you can use to
 help design your attack. The credentials are as follows:

- `wiener:peter`
- `carlos:montoya`

### Solution

The CSRF token has been associated with a certain cookie. However, maybe by fate, the developer mistakenly attached the non-session cookie (i.e the cookie does not serve to track – track – user session).

This can happen when the application uses 2 different frameworks (one for session management and one for handling CSRF) and these two have not been integrated to handle user sessions with CSRF tokens.

The point to note here is that the attacker will need a way to install a non-session cookie into the victim's browser so that there is a successful attack.

Same as above, with Burp Suite Interception running, I also logged into the application with the first account (wiener:peter) and then ran the Update email function and blocked the corresponding request.

![Untitled](CSRF%20c75e24b16bcf4ef1aa531d83bfb90b24/Untitled.png)

Here, I notice a new `csrfKey cookie object` (yv7z9SsRUs1LmY6Y0EbATVu2ANh2WZJk in this case) next to the previous `CSRF token` (5muKb3tnKIuGlMCLQLDa7ePjdRbBqCfA). If I push this request to Burp Suite Repeater and try to make some adjustments, I notice:

  - If I change session cookie, I will be logged out, kicked out;

  - If I change the csrfKey cookie, my CSRF token will be rejected, not related to the session;

If I try to get the csrfKey cookie and CSRF token from the wiener account and push it to the respective request of the carlos account, the application runs surprisingly well.

After confirming the related vulnerability, I now go back to my wiener account to find a way to push my csrfKey cookie to the victim's browser.

With the application's Search function, I noticed that the Search term ('test') is 'reflected' into the `Set-Cookie header` of the response as shown below.

![Untitled](CSRF%20c75e24b16bcf4ef1aa531d83bfb90b24/Untitled%201.png)

This Search function is run without CSRF protection so I can take advantage of it to push the csrfKey cookie to the victim's browser. Specifically, I will build a Search term syntax like below (I use csrfKey cookie illustrated Z0xukRp3aYGPaFR0Evh6nRw1ShYhxCiU) with URL encode

**`/?search=test%0d%0aSet-Cookie:%20csrfKey=Z0xukRp3aYGPaFR0Evh6nRw1ShYhxCiU`**

![Untitled](CSRF%20c75e24b16bcf4ef1aa531d83bfb90b24/Untitled%202.png)