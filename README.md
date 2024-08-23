![platform](https://img.shields.io/badge/PCI-DSS-blue)
![platform](https://img.shields.io/badge/Payment-Gateway-purple)
![](https://img.shields.io/badge/Threat%20Model-STRIDE,%20IriusRisk-yellow)
![platform](https://img.shields.io/badge/Proxy-OWASP%20ZAP,%20Burpsuite-green)
![platform](https://img.shields.io/badge/EKS-kubescape,%20kubebench-orange)
![platform](https://img.shields.io/badge/SAST-Fortify,%20Coverity,%20BlackDuck,%20GitSec,%20Checkov-orange)
![Infrascan](https://img.shields.io/badge/InfraScan-Nessus-orange)
![platform](https://img.shields.io/static/v1?label=Platform&message=OS:%20Windows%20/%20Linux&color=yellow)
![python](https://img.shields.io/badge/python-green.svg?logo=python&labelColor=yellow)
![python](https://img.shields.io/badge/Java-orange.svg?logo=oracle&labelColor=green)
![.NET](https://img.shields.io/badge/.NET-green.svg?logo=.NET&labelColor=yellow)
![NodeJS](https://img.shields.io/badge/NodeJS-blue.svg?logo=Node.JS&labelColor=orange) 

<!-- GIF -->
<img align="center" height="400" width="800" src="https://github.com/madhucnghubphilips/Payment-Security/blob/main/resources/Secure%20Payment.png"/>
<!-- Header Section -->
<h1 align="center"><font face="Arial">Payment-Security</font></h1>
<h3 align="left"><font face="Arial">Payment security protects financial transactions and sensitive data from unauthorized access, fraud, and cyber threats through encryption, authentication, and compliance with security standards. It ensures that payments are processed safely and securely.</font></h3>

<h2 align="left"><font face="Arial">The payment systems involves;</font></h2> 
Three computer systems are involved whenever a customer makes a card/online purchase online. <br>

**The merchant website** 
Contains the product catalogue and shopping cart. Always starts the process to collect the customer’s cardholder data when the customer asks to checkout. <br>
**The customer computer:**
Running a web browser such as Firefox, Chrome or Internet Explorer. <br>
**The payment service provider (PSP):**
A Visa Europe listed validated PCI DSS compliant company that receives the cardholder data and submits it to the payment system. <br>

<h2 align="left"><font face="Arial">The key steps involved in payment;</font></h2>

**Step 1:** CREATE the payment form to collect the customer’s card data and SEND the payment form to the customer's computer. This can be done by the merchant website, or by the PSP website. <br>
**Step 2:** The customer's computer displays the payment form, and the customer enters their card data and presses the OK button (often called ‘submit’ or ‘pay now’) to confirm the payment, which tells the customer computer to SEND the card data to the PSP or to the merchant website. This step always happens on the customer's computer. <br>
**Step 3:** RECEIVE the card data entered by the customer into the payment form and then send it to the payment system for authorization. This can be done by the merchant website, the PSP or both the merchant website and the PSP working together. <br>






<h2 align="left"><font face="Arial">Payment Gateway Integration Methods;</font></h2>
There are several different ways that applications can integrate payment functionality, and the testing approach will vary depending on which one is used. The most common methods are; <br>
<h3 align="left"><font face="Arial">1) Redirecting the user to a third-party payment gateway.</font></h3> <br>
   <img align="center" src="https://github.com/madhucnghubphilips/Payment-Security/blob/main/resources/1%20The%20Redirect%20Process.JPG" /> <br>
<h3 align="left"><font face="Arial">2) Loading a third-party payment gateway in an IFRAME on the application.</font></h3> <br>
   <img align="center" src="https://github.com/madhucnghubphilips/Payment-Security/blob/main/resources/2%20The%20IFRAME.JPG" /> <br>
<h3 align="left"><font face="Arial">3) The Direct Post: Accepting the card details directly, and then making a POST from the application backend to the payment gateway’s API.</font></h3> <br>
   <img align="center" src="https://github.com/madhucnghubphilips/Payment-Security/blob/main/resources/3%20The%20Direct%20Post.JPG" /> <br>
<h3 align="left"><font face="Arial">4) The JavaScript-created form:</font></h3> <br>
   <img align="center" src="https://github.com/madhucnghubphilips/Payment-Security/blob/main/resources/4%20The%20JavaScript%20Created%20Form.JPG" /> <br>


## <h2 align="left"><font face="Arial">1) Quantity tampering:</font></h2> 
Quantity tampering in payment security refers to the manipulation of the quantity field in a transaction to alter the number of items or units being purchased. This can result in undercharging or overcharging for a transaction. 
For example, the quantity should normally be a positive integer, but if the site does not properly validate this then it may be possible to specify a decimal quantity of an item (such as 0.1), or a negative quantity (such as -1). If malicious actor might tamper with the quantity value before a payment is processed, depending on the backend processing, adding negative quantities of an item may result in a negative value, reducing the overall cost of the basket.<br>
**There are usually multiple ways to modify the contents of the basket that should be tested, such as:** <br>
Adding a negative quantity of an item.<br>
Repeatedly removing items until the quantity is negative.<br>
Updating the quantity to a negative value.<br>
Some sites may also provide a drop-down menu of valid quantities (such as items that must be bought in packs of 10), and it may be possible to tamper these requests to add other quantities of items.<br>

If the full basket details are passed to the payment gateway (rather than simply passing a total value), it may also be possible to tamper the values at that stage.<br>

Finally, if the application is vulnerable to HTTP parameter pollution then it may be possible to cause unexpected behavior by passing a parameter multiple times, such as:<br>

```python
POST /api/basket/add
Host: example.org

item_id=1&quantity=5&quantity=4

```
**Mitigation Strategies** <br>
Input Validation: Ensure that all quantity fields are validated on the server side.<br>
Security Controls: Implement checks that compare the quantity requested with inventory levels and expected transaction patterns.<br>
Audit Logs: Maintain detailed logs of all transactions for post-incident investigation and detection of anomalies.<br>

