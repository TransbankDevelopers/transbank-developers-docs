
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-opencart-webpay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> OpenCart> = 3.x </li>
       <li> PHP> = 5.6 and PHP <= 7.1 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your private and public key </li>
       <li> Have Prestashop installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

---

 <aside class="warning">
You are viewing an old <strong> version of the </strong> documentation for this plugin. If you want to see the new version (REST) do [click here] (/ plugin / opencart / webpay)
 </aside>

 <h1 class="toc-ignore"> Webpay Opencart </h1>
 <h1 style="display: none;"> Webpay </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on Opencart.

## Requirements

You must have previously installed Opencart.

Enable the following modules / extensions for PHP:

* Soap
* OpenSSL 1.0.1 or higher
* SimpleXML
* DOM 2.7.8 or higher

## Plugin Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-opencart-webpay/releases/latestíritu (https://github.com/TransbankDevelopers/transbank-plugin-opencart-webpay/releases/latest), and download the latest available version of the plugin.

    Once the plugin is downloaded, go to the Opencart administration page (usually at _mysite.com_ / admin), and go to (Extensions / Installer) indicated below:

     <img src="/images/plug/open/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Click on the [Upload] button, select the plugin file and Opencart will proceed to install the plugin. Once finished, you will be told that the module was installed:

     <img src="/images/plug/open/webpay/paso2.png" class="rounded mx-auto d-block"/>

### Refresh the OpenCart mods system

1. Go to (Extensions / Modifications) and select the "Transbank Webpay" plugin indicated below:

    <img src="/images/plug/open/webpay/paso3.png" class="rounded mx-auto d-block"/>

2. With the plugin "Transbank Webpay" selected, press the button "Refresh"! [Save] <img src="/images/plug/open/webpay/mod_refresh.png" class="rounded mx-auto d-block"/> at the top right.

    OpenCart will indicate that the modifications have been successful on the plugin:

     <img src="/images/plug/open/webpay/paso4.png" class="rounded mx-auto d-block"/>

3. Within the administration site go to (Extensions / Extensions) and filter by "Payments".

    <img src="/images/plug/open/webpay/paso5.png" class="rounded mx-auto d-block"/>

4. Look down for the "Webpay Plus" plugin.

     <img src="/images/plug/open/webpay/paso6.png" class="rounded mx-auto d-block"/>

5. Press the green "+" button to install the plugin.

     <img src="/images/plug/open/webpay/paso7.png" class="rounded mx-auto d-block"/>

6. It will turn red.

   <img src="/images/plug/open/webpay/paso8.png" class="rounded mx-auto d-block"/>

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you, and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Go to the Opencart administration page (usually at _mysite.com_ / admin), and then go to (Extensions / Extensions), filter for "Payments", search for "Webpay Plus" and press the "Edit" button of the plugin:

    <img src="/images/plug/open/webpay/paso8.png" class="rounded mx-auto d-block"/>

2. That's it! You are in the plugin configuration screen, you must enter the following information:
   * ** Status **: Enable or disable the plugin, you must enable it to work as a means of payment in your business.
   * ** Environment **: Environment where the transaction is carried out.
   * ** Commercial code **: It is what identifies you as a merchant. If the code you have is 8 digits you must prepend `5970`.
   * ** Private Key **: Secret key that authorizes and validates you to make transactions.
   * ** Certificate **: Public key that authorizes and validates you to make transactions.

  The options available for _Ambiente_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade.

 <aside class="notice">
  Tip: It is no longer necessary to configure the ** Transbank Certificate **, in previous versions it was required, currently it is replaced by selecting ** environment **.
 </aside>

### Order statuses configuration

Make sure to correctly configure the order statuses as appropriate for your business:

* ** Completed status **: Status of a successful order.
* ** Rejected status **: Status of a rejected order.
* ** Canceled status **: Status of a canceled order.

 <img src="/images/plug/open/webpay/paso9.png" class="rounded mx-auto d-block"/>

### Test Credentials

For the Integration environment, you can use the following credentials for testing:

* Trade code: `597020000540`
* Private Key: It can be found [here - private_key] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
* Public Certificate: It can be found [here - public_cert] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)

 <aside class="notice">
  Tip: When going to the production environment it will be necessary to generate your own private key, you can see the instructions to create them [here] (https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
 </aside>

1. Save the changes by pressing the [Save] button

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on the "Information" button, there you can download a pdf.

   <img src="/images/plug/open/webpay/paso10.png" class="rounded mx-auto d-block"/>

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the store and with the session started, enter any section to add products

   <img src="/images/plug/open/webpay/demo1.png" class="rounded mx-auto d-block"/>

* Add a product to the shopping cart, select the shopping cart and then press the [Checkout] button:

   <img src="/images/plug/open/webpay/demo2.png" class="rounded mx-auto d-block"/>

* Enter all the required data:

   <img src="/images/plug/open/webpay/demo3.png" class="rounded mx-auto d-block"/>

* Select payment method "Payment with Credit Cards or Redcompra":

   <img src="/images/plug/open/webpay/demo4.png" class="rounded mx-auto d-block"/>

* Press the [Continue to Webpay] button

   <img src="/images/plug/open/webpay/demo5.png" class="rounded mx-auto d-block"/>

* Once the button to start the purchase is pressed, the Webpay payment window will be displayed and you must follow the payment process.

For tests you can use the following data:

* Card number: `4051885600446623`
* Ruth: `11,111,111-1`
* Cvv: `123`

 <img src="/images/plug/open/webpay/demo6.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/open/webpay/demo7.png" class="rounded mx-auto d-block"/>

For tests you can use the following data:

* Ruth: `11,111,111-1`
* Password: `123`

 <img src="/images/plug/open/webpay/demo8.png" class="rounded mx-auto d-block"/>

You can accept or reject the transaction

 <img src="/images/plug/open/webpay/demo9.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/open/webpay/demo10.png" class="rounded mx-auto d-block"/>

* You will be redirected to Opencart and you will be able to verify that the payment has been successful.

 <img src="/images/plug/open/webpay/demo11.png" class="rounded mx-auto d-block"/>

* Also if you access the administration site section (Sales / Orders) you can see the order created and the details of the data delivered by Webpay.

 <img src="/images/plug/open/webpay/order1.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/open/webpay/order2.png" class="rounded mx-auto d-block"/>
