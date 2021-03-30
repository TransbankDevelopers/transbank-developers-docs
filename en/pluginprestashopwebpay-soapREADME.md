
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> Prestashop> = 1.6 </li>
       <li> PHP> = 5.6 and PHP <= 7.2 </li>
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
You are viewing an old <strong> version of the </strong> documentation for this plugin. If you want to see the new version (REST) do [click here] (/ plugin / prestashop / webpay)
 </aside>

 <h1 class="toc-ignore"> Webpay Prestashop </h1>
 <h1 style="display: none;"> Webpay </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on Prestashop.

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/SrBvspBWWtU" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> Prestashop integration video tutorial </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

## Requirements

You must have Prestashop previously installed.

Enable the following modules / extensions for PHP:

* Soap
* OpenSSL 1.0.1 or higher
* SimpleXML
* DOM 2.7.8 or higher

## Plugin Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay/releases/latest-lex.europa.eu(https://github.com/TransbankDevelopers/transbank-plugin-prestashop-webpay/releases/latest), and download the latest available version of the plugin.

   Once the plugin is downloaded, enter the Prestashop administration page (usually at _misite.com_ / admin), and go to Modules, Modules and Services, indicated below:

     <img src="/images/plug/prestashop/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Click on the button "Upload a module":

     <img src="/images/plug/prestashop/webpay/paso2.png" class="rounded mx-auto d-block"/>

3. A box will open to upload the previously downloaded module. Proceed to drag the file, or click on "select the file" and select it from your computer:

     <img src="/images/plug/prestashop/webpay/paso3.png" class="rounded mx-auto d-block"/>

4. Prestashop will proceed to install the module. Once finished, you will be told that the module was installed, and when this happens you must click on the "Configure" button:

     <img src="/images/plug/prestashop/webpay/paso4.png" class="rounded mx-auto d-block"/>

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you, and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Go to the Prestashop administration page (usually at _mysite.com_ / admin), and then go to Modules, Modules and Services.

     <img src="/images/plug/prestashop/webpay/paso1.png" class="rounded mx-auto d-block"/>

2. Search for "Webpay", and press the "Configure" button:

     <img src="/images/plug/prestashop/webpay/paso5.png" class="rounded mx-auto d-block"/>

3. That's it! You are in the plugin configuration screen, you must enter the following information:
   * ** Environment **: Environment where the transaction is carried out.
   * ** Commercial code **: It is what identifies you as a merchant. If the code you have is 8 digits you must prepend `5970`.
   * ** Private Key **: Secret key that authorizes and validates you to make transactions.
   * ** Certificate **: Public key that authorizes and validates you to make transactions.

  The options available for _Ambiente_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade.

 <aside class="notice">
  Tip: It is no longer necessary to configure the ** Transbank Certificate **, in previous versions it was required, currently it is replaced by selecting ** environment **.
 </aside>

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

 <img src="/images/plug/prestashop/webpay/paso6.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/prestashop/webpay/paso7.png" class="rounded mx-auto d-block"/>

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

 <img src="/images/plug/prestashop/webpay/demo1.png" class="rounded mx-auto d-block"/>

* Once logged in, enter any section to add products

 <img src="/images/plug/prestashop/webpay/demo2.png" class="rounded mx-auto d-block"/>

* Add a product to the shopping cart, select the shopping cart and then press the [Checkout] button:

 <img src="/images/plug/prestashop/webpay/demo3.png" class="rounded mx-auto d-block"/>

* Press the [Checkout] button:

 <img src="/images/plug/prestashop/webpay/demo4.png" class="rounded mx-auto d-block"/>

* Select shipping method and press the [Continue] button

  You must make sure that your shipping address is in Chile.

 <img src="/images/plug/prestashop/webpay/demo5.png" class="rounded mx-auto d-block"/>

* Select the method of Payment with Credit Cards or Redcompra, select the "terms of service" and then press the [Order with payment obligation] button and then the [Pay] button

 <img src="/images/plug/prestashop/webpay/demo6.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/prestashop/webpay/demo6.1.png" class="rounded mx-auto d-block"/>

* Once the button to start the purchase is pressed, the Webpay payment window will be displayed and you must follow the payment process.

For tests you can use the following data:

* Card number: `4051885600446623`
* Ruth: `11,111,111-1`
* Cvv: `123`

 <img src="/images/plug/prestashop/webpay/demo7.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/prestashop/webpay/demo8.png" class="rounded mx-auto d-block"/>

For tests you can use the following data:

* Ruth: `11,111,111-1`
* Password: `123`

 <img src="/images/plug/prestashop/webpay/demo9.png" class="rounded mx-auto d-block"/>

You can accept or reject the transaction

 <img src="/images/plug/prestashop/webpay/demo10.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/prestashop/webpay/demo11.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/prestashop/webpay/demo12.png" class="rounded mx-auto d-block"/>

* You will be redirected to Prestashop and you will be able to verify that the payment has been successful.

 <img src="/images/plug/prestashop/webpay/demo13.png" class="rounded mx-auto d-block"/>

* Also if you access the administration site section (Orders / Order) you will be able to see the order created and the details of the data delivered by Webpay.

 <img src="/images/plug/prestashop/webpay/order1.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/prestashop/webpay/order2.png" class="rounded mx-auto d-block"/>
