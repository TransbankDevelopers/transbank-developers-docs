
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank"  href="https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> Woocommerce> = 3.2 </li>
       <li> PHP> = 5.6 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your private and public key </li>
       <li> Have WooCommerce installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

---

 <aside class="warning">
You are viewing an old <strong> version of the </strong> documentation for this plugin. If you want to see the new version (REST) do [click here] (/ plugin / woocommerce / webpay)
 </aside>

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube.com/embed/9--NHgh07Fw" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> How to go from SOAP to REST </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

 <h1 class="toc-ignore"> Webpay WooCommerce </h1>
 <h1 style="display: none;"> Webpay </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on Woocommerce.

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/NrvBtGFgA-8" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> WooCommerce integration video tutorial </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

## Requirements

You must have Woocommerce previously installed.

Enable the following modules / extensions for PHP:

* Soap
* OpenSSL 1.0.1 or higher
* SimpleXML
* DOM 2.7.8 or higher

## Plugin Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay/releases/latest-lex.europa.eu(https://github.com/TransbankDevelopers/transbank-plugin-woocommerce-webpay/releases/latest), and download the latest available version of the plugin.

    Once the plugin is downloaded, go to the Woocommerce administration page (usually <http://misitio.com/wp-admin>, <http://localhost/wp-admi>) and go to (Plugins / Add New), indicated below:

     <img src="/images/plug/woo/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Select the file you downloaded in the previous step. Upon completion it will appear that it was installed successfully.

     <img src="/images/plug/woo/webpay/paso2.png" class="rounded mx-auto d-block"/>

3. You must also `Activate Plugin`.

 <img src="/images/plug/woo/webpay/paso3.png" class="rounded mx-auto d-block"/>

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you and you will also be able to generate a diagnostic document in case Transbank requests it.

** IMPORTANT: ** The plugin only works with Chilean currency (CLP). Given this, Woocommerce must be configured with Chilean Peso currency and Chile country so that Webpay can be used.

To access the configuration, you must follow the following steps:

1. Go to the Woocommerce administration page (usually <http://misitio.com/wp-admin>, <http://localhost/wp-admin>) and enter username and password.

2. Within the administration site go to (Plugins / Installed Plugins) and search for `Transbank Webpay Plus`.

     <img src="/images/plug/woo/webpay/paso4.png" class="rounded mx-auto d-block"/>

3. Press the `Settings` link of the plugin and it will take you to the plugin settings page.

     <img src="/images/plug/woo/webpay/paso5.png" class="rounded mx-auto d-block"/>

4. That's it! You are in the plugin configuration screen, you must enter the following information:
   * ** Environment **: Environment where the transaction is carried out.
   * ** Commercial code **: It is what identifies you as a merchant. If the code you have is 8 digits you must prepend `5970`.
   * ** Private key **: Secret key that authorizes and validates you to make transactions.
   * ** Certificate **: Public key that authorizes and validates you to make transactions.
   * ** Order status after payment **: By default, it is `Processing`.

  The options available for _Ambiente_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade.

 <aside class="notice">
  Tip: It is no longer necessary to configure the ** Transbank Certificate **, in previous versions it was required, currently it is replaced by selecting ** environment **.
 </aside>

### Test Credentials

For the Integration environment, you can use the following credentials for testing:

* Trade code: `597020000540`
* Private Key: It can be found [here - private_key] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.key)
* Public certificate: Can be found [here - public_cert] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/blob/master/integracion/Webpay%20Plus%20-%20CLP/597020000540.crt)

 <aside class="notice">
  Tip: When going to the production environment it will be necessary to generate your own private key, you can see the instructions to create them [here] (https://www.transbankdevelopers.cl/documentacion/como_empezar#credenciales-en-webpay)
 </aside>

1. Save the changes by pressing the [Save changes] button

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on the "Information" button, there you can download a pdf.

 <img src="/images/plug/woo/webpay/paso6.png" class="rounded mx-auto d-block"/>

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

 <img src="/images/plug/woo/webpay/demo1.png" class="rounded mx-auto d-block"/>

* Enter any section to add products

 <img src="/images/plug/woo/webpay/demo2.png" class="rounded mx-auto d-block"/>

* Add a product to the shopping cart, select the shopping cart and then press the [Proceed to checkout] button:

 <img src="/images/plug/woo/webpay/demo3.png" class="rounded mx-auto d-block"/>

* Add the requested information and select `Transbank Webpay` then press the [Place order] button

 <img src="/images/plug/woo/webpay/demo4.png" class="rounded mx-auto d-block"/>

* Once the button to start the purchase is pressed, the Webpay payment window will be displayed and you must follow the payment process.

For tests you can use the following data:

* Card number: `4051885600446623`
* Ruth: `11,111,111-1`
* Cvv: `123`

 <img src="/images/plug/woo/webpay/demo5.png" class="rounded mx-auto d-block"/>

For tests you can use the following data:

* Ruth: `11,111,111-1`
* Password: `123`

 <img src="/images/plug/woo/webpay/demo6.png" class="rounded mx-auto d-block"/>

You can accept or reject the transaction

 <img src="/images/plug/woo/webpay/demo7.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/woo/webpay/demo8.png" class="rounded mx-auto d-block"/>

* You will be redirected to Woocommerce and you will be able to verify that the payment has been successful.

 <img src="/images/plug/woo/webpay/demo9.png" class="rounded mx-auto d-block"/>

* Also if you access the administration site section (Woocommerce / Orders) you will be able to see the order created and the details of the data delivered by Webpay.

 <img src="/images/plug/woo/webpay/order1.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/woo/webpay/order2.png" class="rounded mx-auto d-block"/>
