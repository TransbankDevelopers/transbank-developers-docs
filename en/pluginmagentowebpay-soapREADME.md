
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Compatibility </h4>
     <ul>
       <li> Magento> = 2.0 </li>
       <li> PHP> = 5.6 and PHP <= 7.2 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your private and public key </li>
       <li> Have Magento2 installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

---

 <aside class="warning">
You are viewing an old <strong> version of the </strong> documentation for this plugin. If you want to see the new version (REST) do [click here] (/ plugin / magento / webpay)
 </aside>

 <h1 class="toc-ignore"> Webpay Magento2 </h1>
 <h1 style="display: none;"> Webpay </h1>

## Description

This official plugin has been created so that you can easily integrate Webpay into your business, based on Magento2.

 <div class='url-modal-embed' data-toggle-embedYT="modal" data-src="https://www.youtube-nocookie.com/embed/TMbLGFJQX44" >
   <div class="container-embed">
     <div class="data-info-url">
       <b> Magento2 integration video tutorial </b>
     </div>
     <img class="icon-video-YT td_img-night" src="{{dir}}/images/yt_icon.png" alt="Youtube">
   </div>
 </div>

## Requirements

1. You must have Magento2 previously installed and make sure you have [Composer] (https://getcomposer.org) installed.
2. Your Magento Market credentials at hand. If you don't know what your credentials are, you can check this guide: [https://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html-lex.europa.eu(https://devdocs.magento. com / guides / v2.2 / install-gde / prereq / connect-auth.html)

Enable the following modules / extensions for PHP:

* Soap
* OpenSSL 1.0.1 or higher
* SimpleXML
* DOM 2.7.8 or higher

## Plugin installation

** Note: ** At this point composer might ask you if it requires your magento2 credentials.

** Available versions ** [Here] (https://packagist.org/packages/transbank/webpay-magento2)

In your Magento2 directory, run the following command to install the latest version:

    composer require transbank / webpay-magento2

 <img src="/images/plug/mage/webpay/paso7.png" class="rounded mx-auto d-block"/>

When done, run the command:

    magento module: enable Transbank_Webpay --clear-static-content

 <img src="/images/plug/mage/webpay/paso8.png" class="rounded mx-auto d-block"/>

When done, run the command:

    magento setup: upgrade && magento setup: di: compile && magento setup: static-content: deploy

 <img src="/images/plug/mage/webpay/paso9.png" class="rounded mx-auto d-block"/>

Once the above process is done, Magento2 must have installed the Webpay plugin. When finished, you must activate the plugin in the Magento2 administrator.

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Go to the Magento2 administration page (usually <http://misitio.com/admin>, <http://localhost/admin>) and enter username and password.

     <img src="/images/plug/mage/webpay/paso10.png" class="rounded mx-auto d-block"/>

2. Within the administration site go to (Stores / Configuration).

     <img src="/images/plug/mage/webpay/paso11.png" class="rounded mx-auto d-block"/>

3. Then to section (Sales / Payments Methods).

     <img src="/images/plug/mage/webpay/paso12.png" class="rounded mx-auto d-block"/>

4. Choose the country Chile

     <img src="/images/plug/mage/webpay/paso13.png" class="rounded mx-auto d-block"/>

5. Going down to the list of payment methods you will see Webpay

     <img src="/images/plug/mage/webpay/paso14.png" class="rounded mx-auto d-block"/>

6. That's it! You are in the plugin configuration screen, you must enter the following information:

    * ** Enable **: When activated, Webpay will be available as a means of payment. Take care that this option is checked when you want users to pay with Webpay.
    * ** Environment **: Environment where the transaction is carried out.
    * ** Commercial code **: It is what identifies you as a merchant. If the code you have is 8 digits you must prepend `5970`.
    * ** Private Key **: Secret key that authorizes and validates you to make transactions.
    * ** Public Certificate **: Public key that authorizes and validates you to make transactions.

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

1. Save the changes by pressing the [Save Config] button

     <img src="/images/plug/mage/webpay/paso15.png" class="rounded mx-auto d-block"/>

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on "Main Parameters" button "Information" there you can download a pdf.

 <img src="/images/plug/mage/webpay/paso17.png" class="rounded mx-auto d-block"/>

## Magento2 configuration for Chile CLP

The plugin only works with Chilean CLP currency, since magento2 must be correctly configured so that Webpay can be used.

1. Go to the administration section (Stores / General / Country Option) and choose Chile as shown in the following image, then save the changes.

     <img src="/images/plug/mage/webpay/clp1.png" class="rounded mx-auto d-block"/>

2. Go to the administration section (Stores / Currency Setup / Country Option) and choose Chile as shown in the following image, then save the changes.

     <img src="/images/plug/mage/webpay/clp2.png" class="rounded mx-auto d-block"/>

3. Go to the administration section (Stores / Currency) and verify in the two sections (Currency Rates and Currency Symbols) that CLP is active.

 <img src="/images/plug/mage/webpay/clp3.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/clp4.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/clp5.png" class="rounded mx-auto d-block"/>

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

 <img src="/images/plug/mage/webpay/demo1.png" class="rounded mx-auto d-block"/>

* Once logged in, enter any section to add products

 <img src="/images/plug/mage/webpay/demo2.png" class="rounded mx-auto d-block"/>

* Add a product to the shopping cart:

 <img src="/images/plug/mage/webpay/demo3.png" class="rounded mx-auto d-block"/>

* Select the shopping cart and then press the [Proceed to Checkout] button:

 <img src="/images/plug/mage/webpay/demo4.png" class="rounded mx-auto d-block"/>

* Select shipping method and press the [Next] button

  You must make sure that your shipping address is in Chile.

 <img src="/images/plug/mage/webpay/demo5.png" class="rounded mx-auto d-block"/>

* Select Transbank Webpay payment method, then press the [Place Order] button

 <img src="/images/plug/mage/webpay/demo6.png" class="rounded mx-auto d-block"/>

* Once the button to start the purchase is pressed, the Webpay payment window will be displayed and you must follow the payment process.

For tests you can use the following data:

* Card number: `4051885600446623`
* Ruth: `11,111,111-1`
* Cvv: `123`

 <img src="/images/plug/mage/webpay/demo7.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/demo8.png" class="rounded mx-auto d-block"/>

For tests you can use the following data:

* Ruth: `11,111,111-1`
* Password: `123`

 <img src="/images/plug/mage/webpay/demo9.png" class="rounded mx-auto d-block"/>

You can accept or reject the transaction

 <img src="/images/plug/mage/webpay/demo10.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/demo11.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/demo12.png" class="rounded mx-auto d-block"/>

* You will be redirected to Magento2 and you will be able to verify that the payment has been successful.

 <img src="/images/plug/mage/webpay/demo13.png" class="rounded mx-auto d-block"/>

* Also if you access the administration site section (Sales / Orders) you can see the order created and the details of the data delivered by Webpay.

 <img src="/images/plug/mage/webpay/order1.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/order2.png" class="rounded mx-auto d-block"/>

 <img src="/images/plug/mage/webpay/order3.png" class="rounded mx-auto d-block"/>
