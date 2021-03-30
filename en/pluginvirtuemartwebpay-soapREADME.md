
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-webpay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> VirtueMart> = 3.x </li>
       <li> PHP> = 5.6 and <= 7.2.1 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your private and public key </li>
       <li> Have the Ecommerce correctly configured </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

---

 <aside class="notice">
You are looking at the old <strong> SOAP </strong> documentation for this plugin. If you want to see reference of the current version
(REST) make [click here] (/ plugin / virtuemart / webpay)
 </aside>

 <h1 class="toc-ignore"> Webpay Virtuemart </h1>
 <h1 style="display: none;"> Webpay </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on Virtuemart.

## Requirements

You must have Virtuemart previously installed.

Enable the following modules / extensions for PHP:

* Soap
* OpenSSL 1.0.1 or higher
* SimpleXML
* DOM 2.7.8 or higher

## Plugin Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-webpay/releases/latestíritu (https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-webpay/releases/latest) and download the latest available version of the plugin.

    Once the plugin is downloaded, go to the VirtueMart administration page (usually at [http://mysite.com/administrator ](http://mysite.com/administrator), [http: // localhost / administrator] ( http: // localhost / administrator)) and go to (Extensions / Manage / Install), indicated below:

     <img src="/images/plug/virtue/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Drag or select the file that you downloaded in the previous step. Upon completion it will appear that it was installed successfully.

   <img src="/images/plug/virtue/webpay/paso2.png" class="rounded mx-auto d-block"/>

   <img src="/images/plug/virtue/webpay/paso3.png" class="rounded mx-auto d-block"/>

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you and you will also be able to generate a diagnostic document in case Transbank requests it.

** IMPORTANT: ** The plugin only works with Chilean currency (CLP). Given this, VirtueMart must be configured with Chilean Peso currency and Chile country so that Webpay can be used.

To access the configuration, you must follow the following steps:

1. Go to the VirtueMart administration page (usually at [http://mysite.com/administrator ](http://mysite.com/administrator), [http: // localhost / administrator] (http: // localhost / administrator)) and enter username and password.

2. Within the administration site go to (VirtueMart / Payment Methods).

     <img src="/images/plug/virtue/webpay/paso4.png" class="rounded mx-auto d-block"/>

3. You must create a new payment method by pressing the [New] button.

     <img src="/images/plug/virtue/webpay/paso5.png" class="rounded mx-auto d-block"/>

4. Enter the details for "Transbank Webpay" as shown in the following image.

    * Payment Name: Transbank Webpay
    * Sef Alias: transbank_webpay
    * Published: Yes
    * Payment Method: Transbank Webpay
    * Currency: Chilean peso

     <img src="/images/plug/virtue/webpay/paso6.png" class="rounded mx-auto d-block"/>

5. Press the [Save] button to save the new payment method. It will be reported that it has been saved.

     <img src="/images/plug/virtue/webpay/paso7.png" class="rounded mx-auto d-block"/>

6. Select the "Configuration" section to configure the plugin.

     <img src="/images/plug/virtue/webpay/paso8.png" class="rounded mx-auto d-block"/>

7. That's it! You are in the plugin configuration screen, you must enter the following information:
   * ** Environment **: Environment where the transaction is carried out.
   * ** Commercial code **: It is what identifies you as a merchant. If the code you have is 8 digits you must prepend `5970`.
   * ** Private Key **: Secret key that authorizes and validates you to make transactions.
   * ** Public Certificate **: Public key that authorizes and validates you to make transactions.
   * ** Transbank Certificate **: Secret webpay key that authorizes and validates you to make transactions.

  The options available for _Ambiente_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade.

### Test Credentials

For the Integration environment, you can use the following credentials for testing:

* Trade code: `597020000540`
* Private Key: It can be found [here - private_key] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
* Public Certificate: It can be found [here - public_cert] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)
* Webpay Certificate: It can be found [here - webpay_cert] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/tbk.pem.crt)

 <aside class="notice">
  Tip: When going to the production environment it will be necessary to generate your own private key, you can see the instructions to create them [here] (https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
 </aside>

1. Save the changes by pressing the [Save] button

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on the "Information" button, there you can download a pdf.

 <img src="/images/plug/virtue/webpay/paso9.png" class="rounded mx-auto d-block"/>

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

 <img src="/images/plug/virtue/webpay/demo1.png" class="rounded mx-auto d-block"/>

* Once logged in, enter any section to add products

 <img src="/images/plug/virtue/webpay/demo2.png" class="rounded mx-auto d-block"/>

* Add a product to the shopping cart, select the shopping cart and select Transbank Webpay, select terms of service and then press the [Confirm Purchase] button:

 <img src="/images/plug/virtue/webpay/demo3.png" class="rounded mx-auto d-block"/>

* Once the button to start the purchase is pressed, the Webpay payment window will be displayed and you must follow the payment process.

For tests you can use the following data:

* Card number: `4051885600446623`
* Ruth: `11,111,111-1`
* Cvv: `123`

 <img src="/images/plug/virtue/webpay/demo4.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/virtue/webpay/demo5.png" class="rounded mx-auto d-block"/>

For tests you can use the following data:

* Ruth: `11,111,111-1`
* Password: `123`

 <img src="/images/plug/virtue/webpay/demo6.png" class="rounded mx-auto d-block"/>

You can accept or reject the transaction

 <img src="/images/plug/virtue/webpay/demo7.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/virtue/webpay/demo8.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/virtue/webpay/demo9.png" class="rounded mx-auto d-block"/>

* You will be redirected to Virtuemart and you will be able to verify that the payment has been successful.

 <img src="/images/plug/virtue/webpay/demo10.png" class="rounded mx-auto d-block"/>

* Also if you access the administration site section (VirtueMart / Orders) you will be able to see the order created and the details of the data delivered by Webpay.

 <img src="/images/plug/virtue/webpay/order1.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/virtue/webpay/order2.png" class="rounded mx-auto d-block"/>
