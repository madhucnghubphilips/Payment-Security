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
* Adding a negative quantity of an item.<br>
* Repeatedly removing items until the quantity is negative.<br>
* Updating the quantity to a negative value.<br>
* Some sites may also provide a drop-down menu of valid quantities (such as items that must be bought in packs of 10), and it may be possible to tamper these requests to add other quantities of items.<br>

If the full basket details are passed to the payment gateway (rather than simply passing a total value), it may also be possible to tamper the values at that stage.<br>

Finally, if the application is vulnerable to HTTP parameter pollution then it may be possible to cause unexpected behavior by passing a parameter multiple times, such as:<br>

```python
POST /api/basket/add
Host: example.org

item_id=1&quantity=5&quantity=4
```
**Mitigation Strategies** <br>
* Input Validation: Ensure that all quantity fields are validated on the server side.<br>
* Security Controls: Implement checks that compare the quantity requested with inventory levels and expected transaction patterns.<br>
* Audit Logs: Maintain detailed logs of all transactions for post-incident investigation and detection of anomalies.<br>






## <h2 align="left"><font face="Arial">2) Price tampering</font></h2> 
Price tampering in payment security involves the unauthorized manipulation of the price value during a transaction. This can happen when a user intercepts and alters the data being sent from a client to a server, reducing the price of an item or service before the payment is processed. This type of attack can lead to significant financial losses for merchants.<br>

For example, When adding an item to the basket, the application should only include the item and a quantity, such as the example request below:

```python
POST /api/basket/add HTTP/1.1
Host: example.org

item_id=1&quantity=5
```
However, in some cases the application may also include the price, meaning that it may be possible to tamper it:
```python
POST /api/basket/add HTTP/1.1
Host: example.org

item_id=1&quantity=5&price=2.00
```
Different types of items may have different validation rules, so each type needs to be separately tested. Some applications also allow users to add an optional donation to charity as part of their purchase, and this donation can usually be an arbitrary amount. If this amount is not validated, it may be possible to add a negative donation amount, which would then reduce the total value of the basket. <br>

**Mitigation Strategies** <br>
* Server-Side Validation: Always validate prices on the server side before processing payments.
* Use Secure Communication: Implement HTTPS and secure APIs to prevent data interception.
* Integrity Checks: Use cryptographic techniques to ensure the integrity of the data sent between the client and server.






## <h2 align="left"><font face="Arial">3) Tampering at Payment Gateway</font></h2> 
payment gateway tampering refers to malicious activities where attackers manipulate or interfere with the data processed by a payment gateway. This could involve altering payment amounts, intercepting sensitive transaction data like credit card details, or redirecting payments to fraudulent accounts.<br>
The transfer to the gateway may be performed using a cross-domain POST to the gateway, as shown in the HTML example below.
Note: The card details are not included in this request - the user will be prompted for them on the payment gateway:<br>

For example, When adding an item to the basket, the application should only include the item and a quantity, such as the example request below:

```python
<form action="https://example.org/process_payment" method="POST">
	<input type="hidden" id="merchant_id" value="123" />
	<input type="hidden" id="basket_id" value="456" />
	<input type="hidden" id="item_id" value="1" />
	<input type="hidden" id="item_quantity" value="5" />
	<input type="hidden" id="item_total" value="20.00" />
	<input type="hidden" id="shipping_total" value="2.00" />
	<input type="hidden" id="basket_total" value="22.00" />
	<input type="hidden" id="currency" value="GBP" />
	<input type="submit" id="submit" value="submit" />
</form>
```
By modifying the HTML form or intercepting the POST request, it may be possible to modify the prices of items, and to effectively purchase them for less. Note that many payment gateways will reject a transaction with a value of zero, so a total of 0.01 is more likely to succeed. However, some payment gateways may accept negative values (used to process refunds). Where there are multiple values (such as item prices, a shipping cost, and the total basket cost), all of these should be tested.<br>

If the payment gateway uses an IFRAME instead, it may be possible to perform a similar type of attack by modifying the IFRAME URL:

```python
<iframe src="https://example.org/payment_iframe?merchant_id=123&basket_total=22.00" />
```
Different types of items may have different validation rules, so each type needs to be separately tested. Some applications also allow users to add an optional donation to charity as part of their purchase, and this donation can usually be an arbitrary amount. If this amount is not validated, it may be possible to add a negative donation amount, which would then reduce the total value of the basket. <br>

**Mitigation Strategies** <br>
* Encryption: Ensuring all data transmitted between the user and the payment gateway is encrypted (e.g., using SSL/TLS).
* Integrity Checks: Verifying that data has not been altered during transmission.
* Tokenization: Replacing sensitive data with a secure token that cannot be tampered with.
* Secure Coding Practices: Implementing strong validation and security controls within the payment gateway to detect and prevent unauthorized modifications.










## <h2 align="left"><font face="Arial">4) Tampering Encrypted Transaction Details</font></h2> 
In order to prevent the transaction being tampered with, some payment gateways will encrypt the details of the request that is made to them. For example, PayPal does this using public key cryptography.
The first thing to try is making an unencrypted request, as some payment gateways allow insecure transactions unless they have been specifically configured to reject them.<br>
If this doesn’t work, then you need to find the public key that is used to encrypt the transaction details, which could be exposed in a backup of the application, or if you can find a directory traversal vulnerability.
Alternatively, it’s possible that the application re-uses the same public/private key pair for the payment gateway and its digital certificate. You can obtain the public key from the server with the following command:<br>

```python
echo -e '\0' | openssl s_client -connect example.org:443 2>/dev/null | openssl x509 -pubkey -noout
```
Once you have this key, you can then try and create an encrypted request (based on the payment gateway’s documentation), and submit it to the gateway to see if it’s accepted.<br>

**Mitigation Strategies** <br>
* Strong Encryption Protocols: Use strong, up-to-date encryption protocols (e.g., AES-256).
* Secure Key Management: Ensure keys are generated, stored, and rotated securely.
* Message Integrity Checks: Implement message authentication codes (MACs) or digital signatures to ensure the integrity and authenticity of the transaction data.
* End-to-End Encryption: Ensure that the encryption is maintained throughout the entire transaction process, from the client to the payment gateway, without any breaks.
* Tamper-Detection Mechanisms: Incorporate tamper detection methods in the payment gateway that can identify and reject altered encrypted data.







## <h2 align="left"><font face="Arial">5) Tampering Secure Hashes</font></h2> 
Some of payment gateways use a secure hash (or a HMAC) of the transaction details to prevent tampering. The exact details of how this is done will vary between providers (for example, Adyen uses HMAC-SHA256), but it will normally include the details of the transaction and a secret value. For example, a hash may be calculated as:<br>

```python
$secure_hash = md5($merchant_id . $transaction_id . $items . $total_value . $secret)
```
This value is then added to the POST request that is sent to the payment gateway, and verified to ensure that the transaction hasn’t been tampered with.<br>

**Mitigation Strategies** <br>
* Try to removing the secure hash, as some payment gateways allow insecure transactions unless a specific configuration option has been set.
* The POST request should contain all of the values required to calculate this hash, other than the secret key. This means that if you know how the hash is calculated (which should be included in the payment gateway’s documentation), then you can attempt to brute-force the secret.
* Alternatively, if the site is running an off-the-shelf application, there may be a default secret in the configuration files or source code. Finally, if you can find a backup of the site, or otherwise gain access to the configuration files, you may be able to find the secret there.
* Collision Attacks: Finding two different inputs that produce the same hash.
Note: If we use strong secret key, collision attack not possible.
* Length Extension Attacks: Exploiting vulnerabilities in certain hashing algorithms to manipulate data.
   Note: If we use strong secret key, Length Extension Attacks not possible.





## <h2 align="left"><font face="Arial">6) Currency tampering</font></h2> 
Currency tampering in payment security refers to the unauthorized manipulation of the currency used in a transaction, potentially leading to incorrect charges or fraudulent transactions. This could involve altering the currency code, changing exchange rates, or misrepresenting the currency value during a transaction.<br>

**Mitigation Strategies** <br>
* Server-Side Validation: Ensure all currency and amount data are validated and processed securely on the server side.
* Use Trusted Payment Gateways: Rely on well-established payment gateways that enforce currency checks.
* Audit Trails: Implement logging and monitoring to detect any anomalies in transaction details.







## <h2 align="left"><font face="Arial">7) Time Delayed Requests</font></h2> 
If the value of items on the site changes over time (for example on a currency exchange), then it may be possible to buy or sell at an old price by intercepting requests using a local proxy and delaying them. In order for this to be exploitable, the price would need to either be included in the request, or linked to something in the request (such as session or transaction ID). The example below shows how this could potentially be exploited on a application that allows users to buy and sell gold<br>

* View the current price of gold on the site.
* Initiate a buy request for 1oz of gold.
* Intercept and freeze the request.
* Wait one minutes to check the price of gold again:
* If it increases, allow the transaction to complete, and buy the gold for less than it’s current value.
* If it decreases, drop the request request.
* If the site allows the user to make payments using cryptocurrencies (which are usually far more volatile), it may be possible to exploit this by obtaining a fixed price in that cryptocurrency, and then waiting to see if the value rises or falls compared to the main currency used by the site.


   
   
   
   
   
   
   
## <h2 align="left"><font face="Arial">8) Discount Codes</font></h2> 
If the application supports discount codes, then there are various checks that should be carried out <br>

* Are the codes easily guessable (TEST, TEST10, SORRY, SORRY10, company name, etc)?
* If a code has a number in, can more codes be found by increasing the number?
* Is there any brute-force protection?
* Can multiple discount codes be applied at once?
* Can discount codes be applied multiple times?
* Can you inject wildcard characters such as % or *?
* Are discount codes exposed in the HTML source or hidden <input> fields anywhere on the application?
* In addition to these, the usual vulnerabilities such as SQL injection should be tested for.
   
   
   
   

## <h2 align="left"><font face="Arial">9) Breaking Payment Flows</font></h2> 
If the checkout or payment process on an application involves multiple stages (such as adding items to a basket, entering discount codes, entering shipping details, and entering billing information), then it may be possible to cause unintended behavior by performing these steps outside of the expected sequence. For example, you could try<br>

* Modifying the shipping address after the billing details have been entered to reduce shipping costs.
* Removing items after entering shipping details, to avoid a minimum basket value.
* Modifying the contents of the basket after applying a discount code (apply discount code through brute force, if rate limit is not implemented).
* Modifying the contents of a basket after completing the checkout process.

It may also be possible to skip the entire payment process for the transaction. For example, if the application redirects to a third-party payment gateway, the payment flow may be<br>

* The user enters details on the application.
* The user is redirected to the third-party payment gateway.
* The user enters their card details.
* If the payment is successful, they are redirected to success.php on the application.
* If the payment is unsuccessful, they are redirected to failure.php on the application
* The application updates its order database, and processes the order if it was successful.
* Depending on whether the application actually validates that the payment on the gateway was successful, it may be possible to force-browse to the success.php page (possibly including a transaction ID if one is required), which would cause the site to process the order as though the payment was successful. Additionally, it may be possible to make repeated requests to the success.php page to cause an order to be processed multiple times.







## <h2 align="left"><font face="Arial">10) Exploiting Transaction Processing Fees</font></h2> 
Merchants normally have to pay fees for every transaction processed, which are typically made up of a small fixed fee, and a percentage of the total value. This means that receiving very small payments (such as $0.01) may result in the merchant actually losing money, as the transaction processing fees are greater than the total value of the transaction.<br>

This issue is rarely exploitable on e-commerce sites, as the price of the cheapest item is usually high enough to prevent it. However, if the site allows customers to make payments with arbitrary amounts (such as donations), check that it enforces a sensible minimum value.<br>






## <h2 align="left"><font face="Arial">11) Usage of Tokenization for payment security</font></h2> 
Tokenization in payment data is a security technique used to protect sensitive payment information, such as credit card numbers, by replacing it with a unique identifier known as a "token." This token has no exploitable value if breached, as it cannot be used outside the specific context for which it was created. The original payment information is securely stored in a tokenization vault, and only authorized systems can map the token back to the original data.<br>

## Step-by-Step Example of Tokenization

**Customer Enters Payment Information:**
```python
Customer visits an e-commerce website and enters her credit card information:
Card Number: 4111 1111 1111 1111
Expiration Date: 12/24
CVV: 123
```

**Payment Information Sent to Tokenization Service:**
* Instead of storing Customer's credit card information directly, the e-commerce website sends her payment details to a tokenization service provider or payment processor.

**Token Generation:**
* The tokenization service receives Customer's payment data and generates a unique token. For example, the service might create the following token:
* Token: f8e1c9ab7d334f4dbdf1a1c7f8f5e2ab
* This token is a random string and does not reveal any of Customer's actual credit card details.

**Token Returned to the Merchant:**
* The tokenization service returns the token f8e1c9ab7d334f4dbdf1a1c7f8f5e2ab to the e-commerce website, where it is stored in place of Customer's original credit card number.

**Token Used for Transactions:**
* The e-commerce website uses the token to process the transaction. The token is sent to the payment processor, which maps the token back to the original credit card information stored in the secure tokenization vault.
* The payment processor completes the transaction by charging Customer's actual credit card.

**Future Transactions:**
* If Customer returns to the e-commerce website to make another purchase, the website can use the stored token to process the payment without asking for Customer's credit card information again.
* Customer's original credit card details are never exposed during the transaction.

**Security in Case of  Breach:**
* If the e-commerce website's database is breached and the tokens are stolen, the attackers would only have the tokens (f8e1c9ab7d334f4dbdf1a1c7f8f5e2ab), which are useless outside the context of the tokenization service. The original credit card details are securely stored elsewhere and are not compromised.

**Benefits of Tokenization:**
* Enhanced Security: Reduces the risk of sensitive payment data being exposed in the event of a data breach.
* Compliance: Helps businesses comply with industry regulations, such as PCI DSS, by minimizing the scope of systems that handle sensitive data.
* Reduced Risk: If tokens are stolen, they are useless to attackers, as they cannot be reverse-engineered to retrieve the original payment information.
* Flexibility: Tokens can be used across different systems and processes without exposing the original sensitive data.








## <h2 align="left"><font face="Arial">12) 3-D Secure (3DS)</font></h2> 
3-D Secure (3DS) is an authentication protocol designed to enhance the security of online credit and debit card transactions. It adds an additional layer of verification during the payment process, helping to reduce fraud and provide greater protection for both merchants and consumers.<br>

## How 3-D Secure Works:

**Initiation of Payment:**
* When a customer makes an online purchase and enters their credit or debit card information, the transaction process begins.

**Redirection to Authentication Page:**
* The 3DS protocol redirects the customer to a page hosted by their card issuer (e.g., Visa, Mastercard, or another financial institution). This is done seamlessly within the checkout process.

**Authentication Process:**
The card issuer prompts the customer for additional authentication. This might involve:<br>
* Password/PIN: Entering a password or PIN previously set up with the bank.
* One-Time Passcode (OTP): Receiving a one-time passcode via SMS or email that needs to be entered to proceed with the transaction.
* Biometric Verification: Using fingerprint or facial recognition via a banking app on the customer’s smartphone.

**Completion of Payment:**
* Once the customer successfully authenticates, they are redirected back to the merchant’s site, and the payment is completed.
* If authentication fails, the transaction is declined, and the customer is typically notified to try again or use a different payment method.

**Transaction Processing:**
* The payment processor completes the transaction, and the merchant receives confirmation of payment.

**Benefits:**
* Enhanced Security: Reduces the risk of card-not-present (CNP) fraud.
* Compliance: Helps merchants comply with regulations like the Payment Services Directive 2 (PSD2) in the European Union, which mandates strong customer authentication.
* Global Acceptance: Supported by major card networks such as Visa (Verified by Visa), Mastercard (Mastercard SecureCode), and American Express (SafeKey).
* Reduced Fraud: By adding this layer of authentication, the likelihood of unauthorized transactions is significantly reduced.
* Liability Shift: In many cases, when 3DS is used, the liability for fraudulent transactions shifts from the merchant to the card issuer, providing more protection for the merchant.
* Consumer Confidence: Customers feel more secure knowing their transactions are protected by additional security measures.
