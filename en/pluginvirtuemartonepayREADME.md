
 <div class="data-menu-side-right">
   <div class="btn-side-right"> <span> <img src="/images/navbar.png"> </span> </div>
   <div class="block-cantainer">
     <h4> Download our plugin. </h4>
     <a class="td_btn-more" target="_blank" href="https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-onepay/releases/latest"> Download Plugin </a>
     <br>
     <h4> Compatibility </h4>
     <ul>
       <li> VirtueMart> = 3.x </li>
       <li> PHP> = 5.6 and <= 7.2.1 </li>
     </ul>
     <h4> Remember </h4>
     <ol>
       <li> Have your Api Key and Shared Secret </li>
       <li> Have VirtueMart installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

 <h1 class="toc-ignore"> Onepay VirtueMart </h1>
 <h1 style="display: none;"> Onepay </h1>

## Description

This official plugin has been created so that you can easily integrate Onepay into your business, based on Virtuemart.

## Installation

1. Go to [https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-onepay/releasesiexpress(https://github.com/TransbankDevelopers/transbank-plugin-virtuemart-onepay/releases) and download the latest version available of the plugin.

    Once the plugin is downloaded, go to the VirtueMart administration page (usually at [http://mysite.com/administrator ](http://mysite.com/administrator), [http: // localhost / administrator] ( http: // localhost / administrator)) and go to (Extensions / Manage / Install), indicated below:

    ! [Step 1] (/ images / plug / virtue / onepay / step1.png)

2. Drag or select the file that you downloaded in the previous step. Upon completion it will appear that it was installed successfully.

  ! [Step 2] (/ images / plug / virtue / onepay / step2.png)

  ! [Step 3] (/ images / plug / virtue / onepay / step3.png)

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you and you will also be able to generate a diagnostic document in case Transbank requests it.

** IMPORTANT: ** The plugin only works with Chilean currency (CLP). Given this, VirtueMart must be configured with Chilean Peso currency and Chile country so that Onepay can be used.

To access the configuration, you must follow the following steps:

1. Go to the VirtueMart administration page (usually at [http://mysite.com/administrator ](http://mysite.com/administrator), [http: // localhost / administrator] (http: // localhost / administrator)) and enter username and password.

2. Within the administration site go to (VirtueMart / Payment Methods).

   ! [Step 4] (/ images / plug / virtue / onepay / step4.png)

3. You must create a new payment method by pressing the [New] button.

   ! [Step 5] (/ images / plug / virtue / onepay / step5.png)

4. Enter the details for "Transbank Onepay" as shown in the following image.

    * Payment Name: Transbank Onepay
    * Sef Alias: transbank_onepay
    * Published: Yes
    * Payment Method: Transbank Onepay
    * Currency: Chilean peso

   ! [Step 6] (/ images / plug / virtue / onepay / step6.png)

5. Press the [Save] button to save the new payment method. It will be reported that it has been saved.

    ! [Step 7] (/ images / plug / virtue / onepay / step7.png)

6. Select the "Configuration" section to configure the plugin.

   ! [Step 8] (/ images / plug / virtue / onepay / step8.png)

7. That's it! You are in the plugin configuration screen, you must enter the following information:

   * ** Endpoint **: Environment where the transaction is carried out.
   * ** APIKey **: It is what identifies you as a business.
   * ** Shared Secret **: Secret key that authorizes and validates you to make transactions.

  The options available for _Endpoint_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade. Depending on which Endpoint has been selected, the plugin will use one of the two sets of APIKey and Shared Secret as appropriate.

  You can also change the values of the pre-established status of the orders according to your trade.

### Test Credentials

For the Integration environment, you can use the following credentials for testing:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `? XW # WOLG ## FBAGEAYSNQ5APD # JF @ $ AYZ`

1. Save the changes by pressing the [Save] button

   ! [step7] (/ images / plug / virtue / onepay / step7.png)

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on "Generate Diagnostic PDF" and this document will automatically download.

  ! [Step 8] (/ images / plug / virtue / onepay / step8.png)

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

  ! [demo 1] (/ images / plug / virtue / onepay / demo1.png)

* Add a product to the shopping cart

  ! [demo 2] (/ images / plug / virtue / onepay / demo2.png)

* Select the shopping cart, enter the requested information such as address, shipping method, select Transbank Onepay payment method, then press the [Confirm Purchase] button

  ! [demo 3] (/ images / plug / virtue / onepay / demo3.png)

* Once the button is pressed to start the purchase, the Onepay payment window will be displayed, as seen in the image. Take note of the number that appears as "Purchase code", as you will need it to emulate the payment in the next step (In this example 8430 - 7696):

  ! [demo 4] (/ images / plug / virtue / onepay / demo4.png)

* In another browser window, enter the payment emulator from [https://onepay.ionix.cl/mobile-payment-emulator/)(https://onepay.ionix.cl/mobile-payment-emulator/), use test@onepay.cl as an email, and the purchase code obtained from the previous screen. Once you have entered the requested data, press the "Start Payment" button:

  ! [demo 5] (/ images / plug / virtue / onepay / demo5.png)

* If all goes well, the emulator will show options to simulate different situations. To simulate a successful payment, press the `PRE_AUTHORIZED` button. In case you want to simulate a failed payment, press the `REJECTED` button. We will simulate a successful payment by pressing the `PRE_AUTHORIZED` button.

  ! [demo 6] (/ images / plug / virtue / onepay / demo6.png)

* Go back to the browser window where VirtueMart is located and you will be able to verify that the payment has been successful.

 ! [demo 7] (/ images / plug / virtue / onepay / demo7.png)

* In addition, if you access the administration site section (VirtueMart / Orders) you will be able to see the order created and the detail of the data delivered by Onepay.

 ! [demo 8] (/ images / plug / virtue / onepay / demo8.png)

 ! [demo 9] (/ images / plug / virtue / onepay / demo9.png)
