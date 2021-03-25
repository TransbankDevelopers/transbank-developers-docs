 <aside class="warning">
This documentation refers to SOAP services. Find the new [REST documentation here] (/ documentation / how to start)
 </aside>

# How to start

To start integrating Transbank products, we recommend using
our SDKs, available for multiple programming languages and
platforms. In general, there is only one Transbank SDK for the _backend_
of your e-commerce, which allows you to operate with all our products.

Additionally, there are specialized SDKs for some products and
platforms that require integrations beyond the _backend_, as in the case of Onepay.

In this section we will see the steps to get started with the SDK that corresponds to the
programming language that you use in your backend. For SDKs
specific to Onepay, visit the section dedicated to that product.

## Integration Flow

Initially the merchant will have some necessary tasks to perform while the integration process occurs. You can see the complete flow below.

 <img class="td_img-night" src="/images/documentacion/flujo-integracion.svg" alt="Flujo de integración">

## SDK installation

To install the SDK, you must add it to the dependency manager for your language:

[In
** Java **] (https://github.com/TransbankDevelopers/transbank-sdk-java#instalaci%C3%B3n)
You must add this entry in your Maven `pom.xml` file:

xml
 <dependency>
     <groupId> com.github.transbankdevelopers </groupId>
     <artifactId> transbank-sdk-java </artifactId>
     <version> {see-on-github-the-latest-version-available} </version>
 </dependency>
`` ''

We recommend reading [the detailed installation instructions for the Java SDK] (https://github.com/TransbankDevelopers/transbank-sdk-java#instalaci%C3%B3n) for more options and information on the latest version available.

[In
** PHP **] (https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n)
you can use composer (if you don't have it, you can install it from [here] (https://getcomposer.org/)) to download the latest version of the SDK,
running this on the command line when you are at the root of your project:

bash
composer require transbank / transbank-sdk
`` ''

We recommend reading [the detailed installation instructions for the PHP SDK] (https://github.com/TransbankDevelopers/transbank-sdk-php#instalaci%C3%B3n) for more installation options.

[In
** Node.js **] (https://github.com/TransbankDevelopers/transbank-sdk-nodejs#instalaci%C3%B3n)
It must be installed by NPM or Yarn, running this on the command line when you are at the root of your project:

NPM

bash
npm install transbank-sdk --save
`` ''

also, need to have openssl installed globally

bash
npm install -g openssl
`` ''

Yarn

bash
yarn add transbank-sdk
`` ''

We recommend reading [the detailed installation instructions for the Node.js SDK] (https://github.com/TransbankDevelopers/transbank-sdk-nodejs#instalaci%C3%B3n) for more installation options.

[In
** .NET **] (https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n)
you can install the SDK from the Visual Package Manager command line
Studio:

bash
PM> Install-Package TransbankSDK
`` ''

We recommend reading [detailed installation instructions for .NET SDK] (https://github.com/TransbankDevelopers/transbank-sdk-dotnet#instalaci%C3%B3n) for more installation options.

[In
** Ruby **] (https://github.com/TransbankDevelopers/transbank-sdk-ruby#instalaci%C3%B3n) you can install the SDK as a gem:

bash
gem install transbank-sdk
`` ''

We recommend reading [detailed installation instructions for the Ruby SDK] (https://github.com/TransbankDevelopers/transbank-sdk-ruby#instalaci%C3%B3n) for more installation options.

(For Webpay in Ruby you can continue using [libwebpay] (https://github.com/TransbankDevelopers/libwebpay-ruby) or another alternative)

[In
** Python **] (https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) you can install the SDK from PyPI:

bash
pip install transbank-sdk
`` ''

We recommend reading [detailed installation instructions for the Python SDK] (https://github.com/TransbankDevelopers/transbank-sdk-python#instalaci%C3%B3n) for more installation options.

(For Webpay in Python you can still use [libwebpay] (https://github.com/TransbankDevelopers/libwebpay-python), but we recommend using [python-tbk, created by Cornershop] (https://github.com/cornershop / python-tbk) which will be the basis of what we finally integrate into transbank-sdk)

## Environments

Transbank provides two environments: ** Integration ** and ** Production **.

** Integration Environment **: In this environment, the business integrates the product to be contracted and tests
your half payment solution.

** Production environment **: In this environment the store will operate after completing the [start-up process] (# start-into-production) and will carry out transactions
with ** real ** credit or debit cards.

### Integration environment

For Webpay transactions in these environments you must use these
cards:

* VISA 4051885600446623, CVV 123, any expiration date. This card
generates approved transactions.
* MASTERCARD 5186059559590568, CVV 123, any expiration date. This
card generates declined transactions.
* Redcompra 4051884239937763 generates approved transactions (for operations
that allow Redcompra and prepaid debit)
* Redcompra 5186008541233829 generates rejected transactions (for operations
that allow Redcompra and prepaid debit)

When an authentication form appears with RUT and password, you must
use RUT 11.111.111-1 and code 123.

Regarding commerce, in this environment you can use the generic credentials
provided by Transbank that allow you to quickly test (as they come pre-configured in our SDKs). If you are in a hurry, you can go straight to see
how to test our products in this environment:

* [Webpay Plus](webpay#webpay-plus)
* [Webpay Oneclick](webpay#webpay-oneclick)
* [Onepay Checkout](onepay#integracion-checkout)

 <aside class="notice">
Tip: The SDKs and plugins provided by Transbank have pre-configured
credentials for the integration environment. You can see how to use them
in the [SDK documentation] section (https://www.transbankdevelopers.cl/documentacion/webpay#webpay-plus) and [Plugins] (https://www.transbankdevelopers.cl/plugin)
 </aside>

In the case of requiring the integration trade codes, they are the following:

Product | Commercial Code
------   | -----------
Webpay Plus | 597020000540
webpay PLus Mall <br> <i> store 1 </i> <br> <i> store 2 </i> | 597044444401 <br> 597044444402 <br> 597044444403
Webpay Plus Deferred Capture | 597044444404
Oneclick | 597044444405
Oneclick Mall <br> <i> store 1 </i> <br> <i> store 2 </i> | 597044444429 <br> 597044444430 <br> 597044444431

For more details on the credentials you can visit [the GitHub repository
`transbank-webpay-credenciales`] (https://github.com/TransbankDevelopers/transbank-webpay-credenciales/)

 <aside class="notice">
Tip: Each of these environments manages different URLs (endpoints) and
different commercial codes. Keep this in mind when making changes to a
environment to another.
 </aside>

## Security

Transbank's Web services are protected to ensure that only
members authorized by Transbank make use of the available operations.

The implemented security mechanism is based on a communication channel
secure TLS 1.2 added to WS-Security (for SOAP services) or API Keys + Messages
signed (for REST services). This provides authentication, confidentiality and
integrity to the Web Services.

The plugins and SDK for Webpay distributed by Transbank are already built with
the libraries necessary to perform the required validations, but it is
The duty of the trade to ensure that the solution or development of means of payment that
use, comply with security protocols.

## Duties of the Trade

### Plugin and SDK updates

If the merchant is using a Plugin or SDK based solution, they should
be attentive to the updates that Transbank will periodically carry out.
These updates can respond to maintain compatibility with CMS or
Shopping Cart, modifications for security, addition of properties or
functions, or corrections to communications. Official communication always
It will be done through the site <http://www.transbankdevelopers.cl>.

### HTTPS usage

Trade servers both in business environments
integration and production environment must make use of
HTTPS for your web _endpoints_ (both for the
cardholder as in the _callbacks_ exposed to
Transbank). Not using HTTPS can cause problems in
redirects in modern browsers (ex: Safari on iOS
11.3 or higher), preventing the flow of
payment.

 <aside class="warning">
** Warning **: As of December 1, 2018 Transbank will not accept
integrations that do not operate with HTTPS.
 </aside>

### Signature validation

Any merchant that uses Webpay through web services ** MUST ** ensure the
security of transactions, under the scheme of the service offered by
Transbank. In each operation that there is an electronic signature is ** OBLIGATION **
from the receiving party to validate said signature.

### Validation of amounts and purchase orders

The merchant must verify when completing any transaction that the values
reported by Transbank (purchase amount, _buyOrder_, etc.) match the
values delivered by the merchant at the beginning of the transactional flow.

## Put into Production

1. Once the merchant determines that its integration has completed, a [validation process] (# the-process-of-validation-and-start-into-production) must be carried out. If you did the integration with a plugin, consider that together with the integration form, you must generate your credentials and send the public certificate and logo (GIF, 130x59) to support@transbank.cl.

2. After Transbank confirms that the integration form is correct (does not apply to plugins), the merchant will be asked to [generation of credentials] (# credentials-en-onepay) (private key and public certificate). The public certificate must be sent together with the business logo (GIF, 130x59) to support@transbank.cl for registration.

3. Transbank will inform via email the correct registration of the public certificate sent by the merchant. After that, it will be necessary [to change the e-commerce configuration to work in production] (# configuration-for-production-using-the-sdk)

4. With the configuration of the production environment ready, it will be necessary to make a purchase of $ 10 to validate the correct operation.

### Credentials in Onepay

In the case of Onepay, the credentials consist of:

* An API Key
* A secret ("shared secret").

These values will be provided by Transbank and as a whole allow to make
transactions on behalf of the merchant. ** You must keep these credentials to
prevent them from falling into the hands of third parties **.

### Credentials in Webpay

In the case of Webpay, the credentials consist of:

* A commercial code.
* A private key.
* A public certificate.

The key and the certificate (self-signed) must be generated by the merchant,
with the condition that the common name of the certificate must match the
commercial code indicated by Transbank.

To generate these credentials you will have to be in a command line and
have OpenSSL version 1.0. The steps are as follows (you must replace
"597029124456" for your trade code):

1. Create private key:

    ```bash
      openssl genrsa -out 597029124456.key 2048
    ```

2. Create certificate requirement:

    ```bash
      openssl req -new -key 597029124456.key -out 597029124456.csr

      Country Name (2 letter code) []:CL
      State or Province Name (full name) []:
      Locality Name (eg, city) []:SANTIAGO
      Organization Name (eg, company) []:
      Organizational Unit Name (eg, section) []:
      Common Name (eg, your name or your server’s hostname) []:597029124456
      Email Address []:
      Please enter the following ‘extra’ attributes
      to be sent with your certificate request
      A challenge password []:
      An optional company name []:
    ```

    <aside class="notice">
    Tip: Nota como en el ejemplo se ha ingresado un código de comercio en el
    momento en que openssl solicita un "Common Name". Acá debes ingresar el código
    de comercio de **producción** que te entregó Transbank. Asegúrate de que el código de comercio tenga antepuesto el número 5970, como en el ejemplo superior. Además, es importante no ingresar caracteres especiales en los otros campos ("Organization Name", "Locality Name", etc).
    </aside>

3. Create self-signed certificate:

    ```bash
    openssl x509 -req -days 1460 -in 597029124456.csr -signkey 597029124456.key -out 597029124456.crt
    ```

 <aside class="notice">
  It is recommended that public certificates are valid
  of at least 4 years (1460 days).
 </aside>

Finally, you must send the public certificate to Transbank (`597029124456.crt`)
and save the certificate and also your private key (`597029124456.key`), the
that together with your commercial code will allow you to transact. ** You must guard
that private key to prevent it from falling into the hands of third parties **.

 <aside class="warning">
Unlike other SDKs, in .NET you must specify the path to a `.pfx` or` .p12` file
which you must generate from your private key and public certificate.

You can look at the following link for a quick guide on how to generate your
own file: [Create pfx file using openssl] (https://www.ssl.com/how-to/create-a-pfx-p12-certificate-file-using-openssl/)
`openssl pkcs12 -export -out 597029124456.pfx -inkey 597029124456.key -in 597029124456.crt`
 </aside>

### The validation and production start-up process

During the validation of the integration, it is intended to verify that the merchant
transacts safely and smoothly, so a
series of tests and their subsequent submission of evidence to validate the
integration. This validation is a necessary requirement to leave the merchant in
production and a business will not be allowed to use productively the
Webpay service without having a validation.

Transbank will only validate the integrations of those businesses that have a productive commerce code.
To obtain it, follow the instructions on how to become a customer on the portal <http://www.transbank.cl> or contact your
commercial executive.

At this stage, the merchant sends the evidence to support@transbank.cl using the form corresponding to the
integrated product clearly indicating purchase orders, date and time of transactions. For integrations
Webpay that use some [official plugin] (/ plugin) there is a special form.

Download the evidence form ...

* For Webpay integrations: [** Download **] (https://transbankdevelopers.cl/files/evidencia-integracion-webpay.docx)
* For Webpay integrations that use an official plugin: [** Download **] (https://transbankdevelopers.cl/files/evidencia-integracion-webpay-plugins.docx)
* For Onepay integrations: [** Download **] (https://transbankdevelopers.cl/files/evidencia-de-integracion-onepay.docx)

Support will validate that the test cases are consistent with those registered
in the Webpay systems and, if everything is correct, the
trade the conformity to go to production, receiving the instructions
for it. If the tests are not consistent, scopes will be made to the
trade regarding its integration, so that you can make corrections
corresponding and resend the evidence once said
fixes.

 <aside class="notice">
We know that this manual process can be tedious and time consuming than
strictly necessary while one party awaits responses or feedback from
the other. ** That is why we will soon launch a full validation portal
online ** and self-service in which you can carry out this process much faster.
 </aside>

Once support formally informs you that the integration is approved,
You must follow the steps that they indicate to go to production and power
start transacting in a real way.

These steps include changing the environment in which you are transacting, moving from
integration to production, in addition to the instructions for creating
credentials.

During the transition to production, you will be required to perform at least one
actual test transaction, with which the commissioning will officially end
production.

 <aside class="warning">
Important: It is the responsibility of the merchant to consider that the certificate
public that Transbank shares with merchants (and that is included in the SDKs)
for Webpay integrations it has an expiration date, such as
also the certificate that the merchant generates and that it shares with Transbank to
carry out transactions on Webpay. The merchant is responsible for
safeguard your private key and your public certificate, as well as
responsible for replacing these when they expire.
 </aside>

### Configuration for production using the SDKs

If you are using an official Transbank SDK and you want to go to the production environment, it will be necessary to follow the following steps.

1. Remove settings for test environment

    Before creating the new configuration for the production environment, it will be necessary to delete the current configuration for the test environment that complies with the following format `Configuration.ForTestingWebpayPlusNormal ()`

2. Create a new `Configuration` element

     <div class="language-simple" data-multiple-language> </div>

     <img src="/images/documentacion/configuracion/1-configuration-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/1-configuration-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/1-configuration-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

    ```javascript
      const configuration = new Transbank.Configuration()
    ```

3. Assign the productive trade code, delivered by Transbank at the time of contracting the product.

     <img src="/images/documentacion/configuracion/2-codigo-comercio-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/2-codigo-comercio-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/2-codigo-comercio-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

    ```javascript
      const configuration = new Transbank.Configuration()
                              .withCommerceCode(/* tu código de comercio */)
    ```

4. Private key configuration (`.key` for Java and PHP` .pfx` or `.p12` for .NET)
generated by trade in the previous stage.

     <img src="/images/documentacion/configuracion/3-private-key-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/3-private-key-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/3-private-key-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

    ```javascript
      const configuration = new Transbank.Configuration()
                              .withCommerceCode(/* tu código de comercio */)
                              .withPrivateCert(/* tu certificado privado en forma de string */)
    ```

5. Configuration of the public certificate (`.crt`) sent to Transbank for registration. In the case of .NET it is not
It is necessary to configure the `.crt` but the password assigned to the private key must be configured.

     <img src="/images/documentacion/configuracion/4-certificate-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/4-certificate-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/4-certificate-net(password).gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

    ```javascript
      const configuration = new Transbank.Configuration()
                              .withCommerceCode(/* tu código de comercio */)
                              .withPrivateCert(/* tu certificado privado en forma de string */)
                              .withPublicCert(/* tu certificado público en forma de string */)
    ```

6. Selection of the productive environment.

     <img src="/images/documentacion/configuracion/5-environment-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/5-environment-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
     <img src="/images/documentacion/configuracion/5-environment-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

    ```javascript
    const configuration = new Transbank.Configuration()
                            .withCommerceCode(/* tu código de comercio */)
                            .withPrivateCert(/* tu certificado privado en forma de string */)
                            .withPublicCert(/* tu certificado público en forma de string */)
                            .usingEnvironment(Transbank.environments.production) // Se define el ambiente como producción
    ```

7. Create Webpay item using production settings.

 <img src="/images/documentacion/configuracion/6-webpay-php.gif" data-lenguaje-visible='php' class="url-modal-embed rounded mx-auto d-block"/>
 <img src="/images/documentacion/configuracion/6-webpay-java.gif" data-lenguaje-visible='java' class="url-modal-embed rounded mx-auto d-block"/>
 <img src="/images/documentacion/configuracion/6-webpay-net.gif" data-lenguaje-visible='csharp' class="url-modal-embed rounded mx-auto d-block"/>

javascript
const configuration = new Transbank.Configuration ()
                        .withCommerceCode (/ * your commerce code * /)
                        .withPrivateCert (/ * your private certificate as a string * /)
                        .withPublicCert (/ * your public certificate as a string * /)
                        .usingEnvironment (Transbank.environments.production) // The environment is defined as production

const transaction = new Transbank.Webpay (configuration);
`` ''

## End of transaction and transition page requirements

### Webpay

** The trade transition page **, is the page that shows the trade
when Webpay gives you control, after the authorization process and
prior to redirecting the cardholder to the proof of success of the
transaction. Applies to all types of transactions.

** Once the transaction is completed **, the merchant must present a page to the
cardholder so that he is informed of the result of the transaction. The
Information to be presented will depend on whether the transaction was authorized or not.

It is recommended, as a minimum, that you have:

* Order number
* Name of the business (Mall Store)
* Amount and currency of the transaction
* Transaction authorization code
* Transaction date
* Type of payment made (Debit or Credit)
* Fee type
* Amount of fees
* Last 4 digits of the bank card
* Description of the goods and / or services

** When the transaction is not authorized **, it is recommended to inform the cardholder about it. You can submit explanatory text such as:

bash
Purchase Order XXXXXXX rejected
Possible causes of this rejection are:
* Error in entering your Credit or Debit card details (date and / or security code).
* Your Credit or Debit card does not have enough balance.
* Card not yet enabled in the financial system.
`` ''

 <div class="container slate">
   <div class='slate-after-footer'>
     <div class='row d-flex align-items-stretch'>
       <div class='col-12 col-lg-6'>
         <h3 class='toc-ignore fo-size-22 text-center'> Do you have any integration questions? </h3>
         <a href='https://join-transbankdevelopers-slack.herokuapp.com/' target='_blank'>
           <div class='td_block_gray'>
             <img src="https://p9.zdassets.com/hc/theme_assets/138842/200037786/logo.png" alt="" >
             <div class='td_pa-txt'>
              Join the community of integrators in Slack and share good tips by helping new ones or looking for alternative solutions. Our team is there to help you.
             </div>
           </div>
         </a>
       </div>
       <div class='mt-3 mt-lg-0 col-12 col-lg-6'>
         <h3 class='toc-ignore fo-size-22 text-center'> If you still have questions send us a message </h3>
         <a class="pointer magenta" data-toggle='modal' data-target='#modalContactForm'>
           <div class='td_block_gray'>
             <div class="fo-size-20 text-center sub-title_bloq"> <i class="fas fa-envelope"> </i> Send us a message. </div>
             <div class='td_pa-txt'>
              If you need to solve any type of incident on the portal or if there is a problem with your integration that you have not been able to solve, contact us through our form.
             </div>
           </div>
         </a>
       </div>
     </div>
   </div>
 </div>
