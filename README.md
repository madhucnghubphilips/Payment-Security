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
<img align="center" height="400" width="800" src="https://github.com/madhucnghubphilips/Payment-Security/blob/main/resources/Secure%20Payment.png" />
<!-- Header Section -->
<h1 align="center"><font face="Arial">Payment-Security</font></h1>
<h3 align="left"><font face="Arial">Payment security protects financial transactions and sensitive data from unauthorized access, fraud, and cyber threats through encryption, authentication, and compliance with security standards. It ensures that payments are processed safely and securely.</font></h3>

<h3 align="left"><font face="Arial">The payment systems involves;</font></h3>

Three computer systems are involved whenever a customer makes a card/online purchase online. <br>

**The merchant website:** Contains the product catalogue and shopping cart. Always starts the process to collect the customer’s cardholder data when the customer asks to checkout. <br>

**The customer computer:** Running a web browser such as Firefox, Chrome or Internet Explorer. <br>

**The payment service provider (PSP):** A Visa Europe listed validated PCI DSS compliant company that receives the cardholder data and submits it to the payment system. <br>

<h3 align="left"><font face="Arial">The key steps involved in payment;</font></h3>

**Step 1:** CREATE the payment form to collect the customer’s card data and SEND the payment form to the customer's computer. This can be done by the merchant website, or by the PSP website. <br>

**Step 2:** The customer's computer displays the payment form, and the customer enters their card data and presses the OK button (often called ‘submit’ or ‘pay now’) to confirm the payment, which tells the customer computer to SEND the card data to the PSP or to the merchant website. This step always happens on the customer's computer. <br>

**Step 3:** RECEIVE the card data entered by the customer into the payment form and then send it to the payment system for authorization. This can be done by the merchant website, the PSP or both the merchant website and the PSP working together. <br>






<h3 align="left"><font face="Arial">Payment Gateway Integration Methods;</font></h3>

There are several different ways that applications can integrate payment functionality, and the testing approach will vary depending on which one is used. The most common methods are; <br>

1) Redirecting the user to a third-party payment gateway.
2) Loading a third-party payment gateway in an IFRAME on the application.
3) The Direct Post: Accepting the card details directly, and then making a POST from the application backend to the payment gateway’s API.
4) The JavaScript-created form: 



