# Business Logic Vulnerabilities

## **[Lab: Insufficient workflow validation](https://portswigger.net/web-security/logic-flaws/examples/lab-logic-flaws-insufficient-workflow-validation)**

### Description:

> This lab makes flawed assumptions about the sequence of 
events in the purchasing workflow. To solve the lab, exploit this flaw 
to buy a "Lightweight l33t leather jacket".
> 
> 
> You can log in to your own account using the following credentials: `wiener:peter`
> 

### Solution:

In this lab, there is no error when adding products to the cart to cause negative money leading to errors at checkout, so it is imperative to check the functions related to the purchase.

Tried all the features but still couldn't find anything exploitable, so I checked the HTTP History, see if there were any exploitable Requests, and found a request that seemed interesting.

![                                                                 POST Request in HTTP History 
](Business%20Logic%20Vulnerabilities%20c92b153590bd45aabf563bfb1e98d13c/Untitled.png)

                                                                 POST Request in HTTP History 

This request is sent when I pay for an order whose total value is included in the payment, the response is a location: `/cart/order-confirmation?order-confirmed=true` , which is another request:

![                                                                              GET Request](Business%20Logic%20Vulnerabilities%20c92b153590bd45aabf563bfb1e98d13c/Untitled%201.png)

                                                                              GET Request

This GET Request is sent to confirm the payment is valid and does not include any other conditions, after sending this request successfully, the server will confirm the successful payment of the products in the cart. let's say, what if after adding a product with a value greater than the amount currently in the account, then resubmit this request?

I went back, added the product to the cart, then `sent this request again`

 

![Untitled](Business%20Logic%20Vulnerabilities%20c92b153590bd45aabf563bfb1e98d13c/Untitled%202.png)