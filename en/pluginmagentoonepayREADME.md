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
       <li> Have your Api Key and Shared Secret </li>
       <li> Have Magento2 installed on your site </li>
       <li> Have a secure 'https' site </li>
     </ol>
   </div>
 </div>

 <h1 class="toc-ignore"> Onepay Magento </h1>
 <h1 style="display: none;"> Onepay </h1>

## Description

This official plugin has been created so that you can easily integrate Onepay into your business, based on Magento2.

## Requirements

1. You must have Magento2 previously installed and make sure you have [Composer] (https://getcomposer.org) installed.
2. Your Magento Market credentials at hand. If you don't know what your credentials are, you can check this guide: [https://devdocs.magento.com/guides/v2.2/install-gde/prereq/connect-auth.html-lex.europa.eu(https://devdocs.magento. com / guides / v2.2 / install-gde / prereq / connect-auth.html)

## Plugin installation

** Note: ** At this point composer might ask you if it requires your magento2 credentials.

** Versions available ** [Here] (https://packagist.org/packages/transbank/onepay-magento2)

In your Magento2 directory, run the following command to install the latest version:

    composer require transbank / onepay-magento2

  ! [Step 7] (/ images / plug / mage / onepay / step7.png)

When done, run the command:

    magento module: enable Transbank_Onepay --clear-static-content

  ! [Step 8] (/ images / plug / mage / onepay / step8.png)

When done, run the command:

    magento setup: upgrade && magento setup: di: compile && magento setup: static-content: deploy

  ! [Step 9] (/ images / plug / mage / onepay / step9.png)

Once the above process is done, Magento2 must have installed the Onepay plugin. When finished, you must activate the plugin in the Magento2 administrator.

## Setting

This plugin has a configuration site that will allow you to enter credentials that Transbank will grant you and you will also be able to generate a diagnostic document in case Transbank requests it.

To access the configuration, you must follow the following steps:

1. Go to the Magento2 administration page (usually at [http://mysite.com/admin ](http://mysite.com/admin), [http: // localhost / admin] (http: // localhost / admin)) and enter username and password.

    ! [Step 10] (/ images / plug / mage / onepay / step10.png)

2. Within the administration site go to (Stores / Configuration).

    ! [Step 11] (/ images / plug / mage / onepay / step11.png)

3. Then to section (Sales / Payments Methods).

    ! [Step 12] (/ images / plug / mage / onepay / step12.png)

4. Choose the country Chile

    ! [Step 13] (/ images / plug / mage / onepay / step13.png)

5. Going down to the list of payment methods you will see Onepay

    ! [Step 14] (/ images / plug / mage / onepay / step14.png)

6. That's it! You are in the plugin configuration screen, you must enter the following information:

   * ** Enable **: When activated, Onepay will be available as a means of payment. Take care that this option is checked when you want users to pay with Onepay.
   * ** Endpoint **: Environment where the transaction is carried out.
   * ** APIKey **: It is what identifies you as a business.
   * ** Shared Secret **: Secret key that authorizes and validates you to make transactions.

  The options available for _Endpoint_ are: "Integration" to carry out tests and certify the installation with Transbank, and "Production" to carry out real transactions once Transbank has approved the trade. Depending on which Endpoint has been selected, the plugin will use one of the two sets of APIKey and Shared Secret as appropriate.

### Test Credentials

For the Integration environment, you can use the following credentials for testing:

* APIKey: `dKVhq1WGt_XapIYirTXNyUKoWTDFfxaEV63-O5jcsdw`
* Shared Secret: `? XW # WOLG ## FBAGEAYSNQ5APD # JF @ $ AYZ`

1. Save the changes by pressing the [Save Config] button

   ! [Step 15] (/ images / plug / mage / onepay / step15.png)

2. In addition, you can generate a diagnostic document in case Transbank requests it. To do this, click on "Generate Diagnostic PDF", and this document will automatically download.

  ! [Step 17] (/ images / plug / mage / onepay / step17.png)

## Magento2 configuration for Chile CLP

The plugin only works with Chilean currency CLP since magento2 must be correctly configured so that Onepay can be used.

1. Go to the administration section (Stores / General / Country Option) and choose Chile as shown in the following image, then save the changes.

    ! [Step 1] (/ images / plug / mage / onepay / clp1.png)

2. Go to the administration section (Stores / Currency Setup / Country Option) and choose Chile as shown in the following image, then save the changes.

    ! [Step 2] (/ images / plug / mage / onepay / clp2.png)

3. Go to the administration section (Stores / Currency) and verify in the two sections (Currency Rates and Currency Symbols) that CLP is active.

  ! [Step 3] (/ images / plug / mage / onepay / clp3.png)

  ! [Step 4] (/ images / plug / mage / onepay / clp4.png)

  ! [Step 5] (/ images / plug / mage / onepay / clp5.png)

## Installation test with transaction

In an integration environment it is possible to perform a transaction test using an online payment emulator.

* Enter the trade

  ! [Step 1] (/ images / plug / mage / onepay / step18.png)

* Once logged in, enter any section to add products

  ! [Step 4] (/ images / plug / mage / onepay / step19.png)

* Add a product to the shopping cart:

  ! [Step 5] (/ images / plug / mage / onepay / step20.png)

* Select the shopping cart and then press the [Proceed to Checkout] button:

  ! [Step 6] (/ images / plug / mage / onepay / step21.png)

* Select shipping method and press the [Next] button

  You must make sure that your shipping address is in Chile.

  ! [Step 7] (/ images / plug / mage / onepay / step22.png)

* Select the Transbank Onepay payment method, then press the [Place Order] button

  ! [Step 8] (/ images / plug / mage / onepay / step23.png)

* Once the button is pressed to start the purchase, the Onepay payment window will be displayed, as seen in the image. Take note of the number that appears as "Purchase Code", as you will need it to emulate the payment in the next step:

  ! [Step 9] (/ images / plug / mage / onepay / step24.png)

* In another browser window, enter the payment emulator from [https://onepay.ionix.cl/mobile-payment-emulator/)(https://onepay.ionix.cl/mobile-payment-emulator/), use test@onepay.cl as an email, and the purchase code obtained from the previous screen. Once you have entered the requested data, press the "Start Payment" button:

  ! [Step 10] (/ images / plug / mage / onepay / step25.png)

* If all goes well, the emulator will show options to simulate different situations. To simulate a successful payment, press the `PRE_AUTHORIZED` button. In case you want to simulate a failed payment, press the `REJECTED` button. We will simulate a successful payment by pressing the `PRE_AUTHORIZED` button.

  ! [Step 11] (/ images / plug / mage / onepay / step26.png)

* Go back to the browser window where Magento2 is located and you will be able to verify that the payment has been successful.

 ! [Step 12] (/ images / plug / mage / onepay / step27.png)

* Also if you access the administration site section (Sales / Orders) you will be able to see the order created and the details of the data delivered by Onepay.

 ! [Step 13] (/ images / plug / mage / onepay / step28.png)

 ! [Step 14] (/ images / plug / mage / onepay / step29.png)

 ! [Step 15] (/ images / plug / mage / onepay / step30.png)
