# Webpay Plus

___

 <aside class="notice">
You are looking at the <strong> new REST reference </strong>. The above reference is obsolete
(SOAP). If you need to review it, click [click here] (/ reference / webpay-soap)
 </aside>

## Environments and Credentials

The Webpay REST API is protected to guarantee that only merchants authorized by Transbank make use of the available operations. Security is implemented through the following mechanisms:

* Secure channel through TLSv1.2 for customer communication with Webpay.
* Authentication and authorization through the exchange of headers `Tbk-Api-Key-Id` (commercial code) and` Tbk-Api-Key-Secret` (secret key).

### Production environment

Production endpoint URLs are hosted within
 <https://webpay3g.transbank.cl/>.

java
// Host: https://webpay3g.transbank.cl
`` ''

`` `php
// Host: https://webpay3g.transbank.cl
`` ''

csharp
// Host: https://webpay3g.transbank.cl
`` ''

ruby
# Host: https://webpay3g.transbank.cl
`` ''

python
# Host: https://webpay3g.transbank.cl
`` ''

http
Host: https://webpay3g.transbank.cl
`` ''

### Integration Environment

Integration endpoint URLs are hosted within
 <https://webpay3gint.transbank.cl/>.

java
// Host: https://webpay3gint.transbank.cl
`` ''

`` `php
// Host: https://webpay3gint.transbank.cl
`` ''

csharp
// Host: https://webpay3gint.transbank.cl
`` ''

ruby
# Host: https://webpay3gint.transbank.cl
`` ''

python
# Host: https://webpay3g.transbank.cl
`` ''

http
Host: https://webpay3gint.transbank.cl
`` ''

See [documentation for test cards that work on
the integration environment] (/ documentation / how to start # environments).

### Trade Credentials

java
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
`` ''

`` `php
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
`` ''

csharp
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
`` ''

ruby
# Tbk-Api-Key-Id: Trade Code
# Tbk-Api-Key-Secret: Secret key
# Content-Type: application / json
`` ''

python
# Tbk-Api-Key-Id: Trade Code
# Tbk-Api-Key-Secret: Secret key
# Content-Type: application / json
`` ''

http
Tbk-Api-Key-Id: Commercial Code
Tbk-Api-Key-Secret: Secret key
Content-Type: application / json
`` ''

All the requests you make must include the commercial code and the key
secret issued by Transbank, both acting as the credentials that authorize
different operations.

 <aside class="notice">
Please note that your trade code (s) in production environment are not
equal to those delivered for the integration environment.
 </aside>

### Trade Codes

In the documentation you can check [all commercial codes] (/ documentation / como_empezar # commercial-codes) of the integration environment

> The SDKs already include those trade codes and secret keys
which work in the integration environment, so you can get
> quickly a configuration ready to do your first tests in said
> environment:

java
// The SDK points by default to the test environment, it is not necessary to configure the following
WebpayPlus.Transaction.setCommerceCode (597055555532);
WebpayPlus.Transaction.setApiKey ('579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C');
WebpayPlus.Transaction.setIntegrationType (IntegrationType.TEST);
`` ''

`` `php
// The SDK points by default to the test environment, it is not necessary to configure the following
Transbank \ Webpay \ WebpayPlus :: setCommerceCode ('597055555532');
Transbank \ Webpay \ WebpayPlus :: setApiKey ('579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C');
Transbank \ Webpay \ WebpayPlus :: setIntegrationType ('TEST');
`` ''

csharp
// The SDK points by default to the test environment, it is not necessary to configure the following
WebpayPlus.Transaction.CommerceCode = 597055555532;
WebpayPlus.Transaction.ApiKey = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
WebpayPlus.Transaction.IntegrationType = WebpayIntegrationType.Test;
`` ''

ruby
# The SDK points to the test environment by default, it is not necessary to configure the following
Transbank :: Webpay :: WebpayPlus :: Base.commerce_code = 597055555532;
Transbank :: Webpay :: WebpayPlus :: Base.api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C";
Transbank :: Webpay :: WebpayPlus :: Base.integration_type = "TEST";

`` ''

python
# The SDK points to the test environment by default, it is not necessary to configure the following
transbank.webpay.webpay_plus.webpay_plus_default_commerce_code = 597055555532
transbank.webpay.webpay_plus.default_api_key = "579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C"
transbank.webpay.webpay_plus.default_integration_type = IntegrationType.TEST
`` ''

javascript
const Environment = require ('transbank-sdk'). Environment;

WebpayPlus.commerceCode = 597055555532;
WebpayPlus.apiKey = '579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C';
WebpayPlus.environment = Environment.Integration;
`` ''

http
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
`` ''

## Webpay Plus

A normal authorization transaction (or normal transaction), corresponds to
a request for financial authorization of a credit card payment or
debit, where who makes the payment enters the merchant's site,
select products or services, and the entry associated with the card details
Credit, debit or prepayment is done safely in Webpay.

### Flow in case of success and abort a payment

Check [the documentation] (/ documentation / webpay-plus # flow-in-case-of-success) of Webpay plus to review the different possible payment flows.


### Create a transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # create-a-transaction)

It allows to initialize a transaction in Webpay. In response to the invocation
a token is generated that uniquely represents a transaction.

To create a transaction, just call the `Transaction.create ()` method.

java
import cl.transbank.webpay.webpayplus.WebpayPlus;
import cl.transbank.webpay.webpayplus.model.CreateWebpayPlusTransactionResponse;

final WebpayPlusTransactionCreateResponse response = WebpayPlus.Transaction.create (
  buyOrder, sessionId, amount, returnUrl
);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus \ Transaction;

$ response = Transaction :: create ($ buy_order, $ session_id, $ amount, $ return_url);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Create (buyOrder, sessionId, amount, returnUrl);
`` ''

ruby
response = Transbank :: Webpay :: WebpayPlus :: Transaction :: create (
  buy_order: buy_order,
  session_id: session_id,
  amount: amount,
  return_url: return_url
)
`` ''

python
response = transbank.webpay.webpay_plus.create (buy_order, session_id, amount, return_url)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.Transaction.create (buyOrder, sessionId, amount, returnUrl);
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
 "buy_order": "OrdenCompra12345678",
 "session_id": "sesion1234557545",
 "amount": 10000,
 "return_url": "http://www.comercio.cl/webpay/retorno"
}
`` ''

 <strong> Parameters Transaction.create </strong>

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Store purchase order. This number must be unique for each transaction. Maximum length: 26. The purchase order can have: Numbers, letters, upper and lower case, and the signs <code> & # 124; _ = &%., ~: /? [+! @ ()> - </code>
session_id <br> <i> String </i> | Session identifier, internal trade use, this value is returned at the end of the transaction. Maximum length: 61
amount <br> <i> Decimal </i> | Amount of the transaction. Maximum 2 decimal places for USD. Maximum length: 17
return_url <br> <i> String </i> | URL of the merchant, to which Webpay will redirect after the authorization process. Maximum length: 256

 <strong> Transaction.create response </strong>

java
response.getUrl ();
response.getToken ();
`` ''

`` `php
$ response-> getUrl ();
$ response-> getToken ();
`` ''

csharp
response.Url;
response.Token;
`` ''

ruby
response.url
response.token
`` ''

python
response.url
response.token
`` ''

javascript
response.url
response.token
`` ''

http
200 OK
Content-Type: application / json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Length: 64.
url <br> <i> String </i> | Webpay payment form URL. Maximum length: 255.

### Confirm a transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # confirm-a-transaction)

It allows you to confirm and obtain the result of the transaction once Webpay has resolved your financial authorization.

When the trade retakes control through `return_url` you must confirm and obtain
the result of a transaction using the `Transaction.commit ()` method.

java
final CreateWebpayPlusTransactionResponse response = WebpayPlus.Transaction.commit (token);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus \ Transaction;

$ response = Transaction :: commit ($ token);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Commit (token);
`` ''

ruby
response = Transbank :: Webpay :: WebpayPlus :: Transaction :: commit (token: @token)
`` ''

python
response = transbank.webpay.webpay_plus.transaction.commit (token)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.Transaction.commit (token);
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token}
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

 <strong> Transaction.commit parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)

 <strong> Transaction.commit response </strong>

java
response.getVci ();
response.getAmount ();
response.getStatus ();
response.getBuyOrder ();
response.getSessionId ();
response.getCardDetail ();
response.getAccountingDate ();
response.getTransactionDate ();
response.getAuthorizationCode ();
response.getPaymentTypeCode ();
response.getResponseCode ();
response.getInstallmentsAmount ();
response.getInstallmentsNumber ();
response.getBalance ();
`` ''

`` `php
$ response-> getVci ();
$ response-> getAmount ();
$ response-> getStatus ();
$ response-> getBuyOrder ();
$ response-> getSessionId ();
$ response-> getCardDetail ();
$ response-> getAccountingDate ();
$ response-> getTransactionDate ();
$ response-> getAuthorizationCode ();
$ response-> getPaymentTypeCode ();
$ response-> getResponseCode ();
$ response-> getInstallmentsAmount ();
$ response-> getInstallmentsNumber ();
$ response-> getBalance ();
`` ''

csharp
response.Vci;
response.Amount;
response.Status;
response.BuyOrder;
response.SessionId;
response.CardDetail;
response.AccountingDate;
response.TransactionDate;
response.AuthorizationCode;
response.PaymentTypeCode;
response.ResponseCode;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.Balance;
`` ''

ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
`` ''

python
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
`` ''

javascript
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
`` ''

http
200 OK
Content-Type: application / json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16: 41: 21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
vci <br> <i> String </i> | Result of cardholder authentication. It can take the value TSY (Authentication successful), TSN (Authentication failed), TO (Maximum time exceeded for authentication), ABO (Aborted authentication by cardholder), U3 (Internal authentication error), NP (Does not participate, probably because a foreign card that does not participate in the 3DSecure program), ACS2 (Foreign Authentication Failed). It can be empty if the transaction was not authenticated. Maximum length: 3. This field is supplementary additional information to the `responseCode` but the merchant ** does not ** have to validate this field. Because new authentication mechanisms are constantly being added that translate into new values for this field that are not necessarily documented. (In the case of international cards that do not provide 3D-Secure, the merchant's decision to accept them or not is made at the merchant's configuration level in Transbank and must be discussed with the merchant's executive)
amount <br> <i> Decimal </i> | Integer format for weight transactions and decimal for dollar transactions. Maximum length: 17
status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
buy_order <br> <i> String </i> | Purchase order from the store indicated in `Transaction.create ()`. Maximum length: 26
session_id <br> <i> String </i> |Session identifier, the same one originally sent by the merchant in `Transaction.create ()`. Maximum length: 61.
card_detail <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
card_detail.card_number <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for businesses authorized by Transbank is the full number sent. Maximum length: 19.
accounting_date <br> <i> String </i> | Authorization date. Length: 4, MMDD format
transaction_date <br> <i> String </i> | Date and time of authorization. Length: 6, format: MMDDHHmm
authorization_code <br> <i> String </i> | Transaction authorization code Maximum length: 6
payment_type_code <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VD = Sale Debit. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Installments without interest <br> VP = Prepaid Sale.
response_code <br> <i> String </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br>
installments_amount <br> <i> Number </i> | Amount of fees. Maximum length: 17
installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2
balance <br> <i> Number </i> | Remaining amount for a voided detail. Maximum length: 17


### Get status of a transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # get-status-of-a-transaction)

This operation allows you to obtain the status of the transaction at any time. Under normal conditions it is likely
that is not required to execute, but in the event of an unexpected error, it allows knowing the status and taking the
corresponding actions.

 <strong> Transaction.status () </strong>


java
final StatusWebpayPlusTransactionResponse response = WebpayPlus.Transaction.status (token);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus \ Transaction;

$ response = Transaction :: getStatus ($ token);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Status (token);
`` ''

ruby
response = Transbank :: Webpay :: WebpayPlus :: Transaction :: status (token: @token)
`` ''

python
response = transbank.webpay.webpay_plus.transaction.status (token)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.Transaction.status (token);
`` ''

http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

 <strong> Transaction.status parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)

 <strong> Transaction.status response </strong>

java
response.getVci ();
response.getAmount ();
response.getStatus ();
response.getBuyOrder ();
response.getSessionId ();
response.getCardDetail ();
response.getAccountingDate ();
response.getTransactionDate ();
response.getAuthorizationCode ();
response.getPaymentTypeCode ();
response.getResponseCode ();
response.getInstallmentsAmount ();
response.getInstallmentsNumber ();
response.getBalance ();
`` ''

`` `php
$ response-> getVci ();
$ response-> getAmount ();
$ response-> getStatus ();
$ response-> getBuyOrder ();
$ response-> getSessionId ();
$ response-> getCardDetail ();
$ response-> getAccountingDate ();
$ response-> getTransactionDate ();
$ response-> getAuthorizationCode ();
$ response-> getPaymentTypeCode ();
$ response-> getResponseCode ();
$ response-> getInstallmentsAmount ();
$ response-> getInstallmentsNumber ();
$ response-> getBalance ();
`` ''

csharp
response.Vci;
response.Amount;
response.Status;
response.BuyOrder;
response.SessionId;
response.CardDetail;
response.AccountingDate;
response.TransactionDate;
response.AuthorizationCode;
response.PaymentTypeCode;
response.ResponseCode;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.Balance;
`` ''

ruby
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
`` ''

python
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
`` ''

javascript
response.vci
response.amount
response.status
response.buy_order
response.session_id
response.card_detail
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_amount
response.installments_number
response.balance
`` ''

http
200 OK
Content-Type: application / json

{
  "vci": "TSY",
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
      "card_number": "6623"
  },
  "accounting_date": "0522",
  "transaction_date": "2019-05-22T16: 41: 21.063Z",
  "authorization_code": "1213",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
vci <br> <i> String </i> | Result of cardholder authentication. It can take the value TSY (Authentication successful), TSN (Authentication failed), TO (Maximum time exceeded for authentication), ABO (Aborted authentication by cardholder), U3 (Internal authentication error), NP (Does not participate, probably because a foreign card that does not participate in the 3DSecure program), ACS2 (Foreign Authentication Failed). It can be empty if the transaction was not authenticated. Maximum length: 3. This field is supplementary additional information to the `responseCode` but the merchant ** does not ** have to validate this field. Because new authentication mechanisms are constantly being added that translate into new values for this field that are not necessarily documented. (In the case of international cards that do not provide 3D-Secure, the merchant's decision to accept them or not is made at the merchant's configuration level in Transbank and must be discussed with the merchant's executive)
amount <br> <i> Integer format for weight transactions and decimal for dollar transactions. </i> | Amount of the transaction. Maximum length: 17
status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
buy_order <br> <i> String </i> | Purchase order from the store indicated in `Transaction.create ()`. Maximum length: 26
session_id <br> <i> String </i> |Session identifier, the same one originally sent by the merchant in `Transaction.create ()`. Maximum length: 61.
card_detail <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
card_detail.card_number <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for businesses authorized by Transbank is the full number sent. Maximum length: 19.
accounting_date <br> <i> String </i> | Authorization date. Length: 4, MMDD format
transaction_date <br> <i> String </i> | Date and time of authorization. Length: 6, format: MMDDHHmm
authorization_code <br> <i> String </i> | Transaction authorization code Maximum length: 6
payment_type_code <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VD = Sale Debit. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Installments without interest <br> VP = Prepaid Sale.
response_code <br> <i> String </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br>
installments_amount <br> <i> Number </i> | Amount of fees. Maximum length: 17
installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2
balance <br> <i> Number </i> | Remaining amount for a voided detail. Maximum length: 17

### Reverse or Void a payment
You can review more details of this operation in [its documentation] (/ documentation / webpay-plus # reverse-or-cancel-a-transaction)

To cancel a transaction, the `Transaction.refund ()` method must be invoked.

 <strong> Transaction.refund () </strong>


> The SDKs allow you to optionally indicate the commercial code of the
> transaction to be canceled, to support cancellation in Webpay Plus merchants
> Mall. In Webpay Plus stores, it is not necessary to specify the code
> Trade as the one indicated in the configuration is used.

 <aside class="notice">
The `Transaction.refund ()` method must always be invoked indicating the code of the merchant that carried out the transaction. In the case of Webpay Plus Mall merchants, the code must be the code of the specific virtual store.
 </aside>

java
final RefundWebpayPlusTransactionResponse response = WebpayPlus.Transaction.refund (token, amount);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus \ Transaction;

$ response = Transaction :: refund ($ token, $ amount);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var response = Transaction.Refund (token, amount);
`` ''

ruby
response = Transbank :: Webpay :: WebpayPlus :: Transaction :: refund (token: @token, amount: @amount)
`` ''

python
response = Transbank.webpay.webpay_plus.refund (token, amount)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.Transaction.refund (token, amount);
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/refunds
Tbk-Api-Key-Id: 597055555532
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "amount": 1000
}
`` ''

 <strong> Transaction.refund parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
amount <br> <i> Integer format for weight transactions. Only in case of dollar it accepts two decimal places. </i> | Amount to be canceled or reversed from the transaction. Maximum length: 17.

 <strong> Transaction.refund response </strong>

java
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getBalance ();
response.getNullifiedAmount ();
response.getResponseCode ();
response.getType ();
`` ''

`` `php
$ response-> getAuthorizationCode ();
$ response-> getAuthorizationDate ();
$ response-> getBalance ();
$ response-> getNullifiedAmount ();
$ response-> getResponseCode ();
$ response-> getType ();
`` ''

csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
`` ''

ruby
response.authorization_code;
response.authorization_date;
response.balance;
response.nullified_amount;
response.response_code;
response.type;
`` ''

python
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

javascript
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

http
200 OK
Content-Type: application / json
{
  "type": "NULLIFIED",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
type <br> <i> String </i> | Type of refund (REVERSED or NULLIFIED). If it is REVERSED, no transaction data will be returned (authorization code, etc). Maximum length: 10
authorization_code <br> <i> String </i> | (Only if it is NULLIFIED) Authorization code of the cancellation. Maximum length: 6
authorization_date <br> <i> String </i> | (Only if it is NULLIFIED) Date and time of the authorization.
balance <br> <i> Decimal </i> | (Only if it is NULLIFIED) Updated balance of the transaction (considers the sale minus the canceled amount). Maximum length: 17
nullified_amount <br> <i> Decimal </i> | (Only if it is NULLIFIED) Amount canceled. Maximum length: 17
response_code <br> <i> Number </i> | (Only if it is NULLIFIED) Result code of the reversal / cancellation. If it is successful it is 0, otherwise the reversal / cancellation was not carried out Maximum Length: 2

In the event of an error, the following common error codes may appear for the `Transaction.refund ()` method:

Code | Description
------ | -----------
304 | Null input field validation
245 | Commercial code does not exist
22 | The trade is not active
316 | The indicated merchant does not correspond to the certificate or is not a child of the MALL merchant in case of MALL transactions
308 | Operation not allowed
274 | Transaction not found
16 | The transaction does not allow cancellation
292 | The transaction is not authorized
284 | Cancellation period exceeded
310 | Transaction previously voided
311 | Amount to be canceled exceeds the balance available to cancel
312 | Generic error for overrides
315 | Authorizer error
53 | The transaction does not allow partial cancellation of transactions with fees

### Capture a transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # capture-a-transaction)

Allows you to request from Webpay the deferred capture of a transaction with
authorization and without simultaneous capture.

 <strong> Transaction.capture () </strong>

java
final CaptureWebpayPlusTransactionResponse response = WebpayPlus.DeferredTransaction.capture (token, buyOrder, authorizationCode, amount);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus;

$ response = Transaction :: capture ($ token, $ buyOrder, $ authCode, $ amount);
`` ''

csharp
var response = DeferredTransaction.Capture (token, buyOrder, authorizationCode, captureAmount);
`` ''

ruby
response = Transbank :: Webpay :: WebpayPlus :: DeferredTransaction :: capture (
  token: @token,
  buy_order: @buy_order,
  authorization_code: @auth_code,
  capture_amount: @amount
)
`` ''

python
response = DeferredTransaction.capture (
  token = token, buy_order = buy_order, authorization_code = authorization_code, capture_amount = amount
)
`` ''

javascript
const response = await WebpayPlus.DeferredTransaction.capture (token, buyOrder, authorizationCode, captureAmount);
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/capture
Tbk-Api-Key-Id: 597055555531
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "commerce_code": "597055555531",
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
`` ''

 <strong> Transaction.capture parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
commerce_code <br> <i> Number </i> | (Optional, only use in Mall case) Child store that carried out the transaction. Length: 12.
buy_order <br> <i> String </i> | Purchase order of the transaction that is required to be captured. Maximum length: 26.
authorization_code <br> <i> String </i> |Authorization code of the transaction that is required to capture Maximum length: 6.
capture_amount <br> <i> Decimal </i> |Amount to be captured. Maximum length: 17.

 <strong> Transaction.capture response </strong>

java
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getCapturedAmount ();
response.getResponseCode ();
`` ''

`` `php
$ response-> getAuthorizationCode ();
$ response-> getAuthorizationDate ();
$ response-> getCapturedAmount ();
$ response-> getResponseCode ();
`` ''

csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.CapturedAmount;
response.ResponseCode;
`` ''

ruby
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

python
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

http
200 OK
Content-Type: application / json
{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "captured_amount": 1000,
  "response_code": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Maximum length: 64
authorization_code <br> <i> String </i> | Authorization code for deferred capture. Maximum length: 6
authorization_date <br> <i> String </i> | Date and time of authorization.
captured_amount <br> <i> Decimal </i> | Amount captured. Maximum length: 6
response_code <br> <i> Number </i> | Capture result code. If it is successful it is 0, otherwise the capture was not carried out. Maximum length: 2

In case of error the following unique codes of the method may appear
`Transaction.capture ()`:

Code | Description
------ | -----------
304 | Null input field validation
245 | Commercial code does not exist
22 | The trade is not active
316 | The store indicated does not correspond to the certificate or is not a child of the Mall store in case of MALL transactions
308 | Operation not allowed
274 | Transaction not found
16 | The transaction is not deferred capture
292 | The transaction is not authorized
284 | Capture period exceeded
310 | Transaction previously reversed
309 | Transaction previously captured
311 | Amount to capture exceeds the authorized amount
315 | Authorizer error

## Webpay Plus Mall

A Mall Normal transaction corresponds to an authorization request
financial of a set of payments with credit or debit cards, where
who makes the payment enters the merchant's site, selects products or
services, and the income associated with the credit or debit card details
You do it once in a secure way in Webpay for all payments.
Each payment will have its own result, authorized or rejected.

Check more details about this modality in [the documentation] (/ documentation / webpay-plus # webpay-plus-mall)

### Create a mall transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # create-a-transaction-mall)

To create a transaction, just call the `Transaction.create ()` method.

 <strong> Transaction.create () Mall </strong>

java
CreateMallTransactionDetails transactionDetails = CreateMallTransactionDetails.build ()
    .add (amountMallOne, commerceCodeMallOne, buyOrderMallOne)
    .add (amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo);

final CreateWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.create (buyOrder, sessionId, returnUrl, transactionDetails);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus;
use Transbank \ Webpay \ WebpayPlus \ Transaction;

WebpayPlus :: configureMallForTesting ();

$ transaction_details = [
  [
      "amount" => 10000,
      "commerce_code" => 597055555536,
      "buy_order" => "orderCompraDetalle1234"
  ],
  [
     "amount" => 12000,
     "commerce_code" => 597055555537,
     "buy_order" => "orderCompraDetalle4321"
  ],
];

$ response = Transaction :: createMall ($ buy_order, $ session_id, $ return_url, $ transaction_details);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var transactionDetails = new List <TransactionDetail> ();
transactions.Add (new TransactionDetail (
    amountMallOne,
    commerceCodeMallOne,
    buyOrderMallOne
));
transactions.Add (new TransactionDetail (
    amountMallTwo,
    ComerceCodeMallTwo,
    buyOrderMallTwo
));

var result = MallTransaction.Create (buyOrder, sessionId, returnUrl, transactionDetails);
`` ''

ruby
transaction_details = [
  {
      amount: 10000,
      commerce_code: 597055555536,
      buy_order: "orderCompraDetalle1234"
  },
  {
     amount: 12000,
     commerce_code: 597055555537,
     buy_order: "orderCompraDetalle4321"
  },
]

response = Transbank :: Webpay :: WebpayPlus :: MallTransaction :: create (
  buy_order: buy_order,
  session_id: session_id,
  return_url: return_url,
  details: transaction_details
)
`` ''

python
transaction_details = MallTransactionCreateDetails (
  amount_child_1, commerce_code_child_1, buy_order_child_1
) .add (
  amount_child_2, commerce_code_child_2, buy_order_child_2
)

response = MallTransaction.create (
    buy_order = buy_order,
    session_id = session_id,
    return_url = return_url,
    details = transaction_details,
)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;
const TransactionDetail = require ('transbank-sdk'). TransactionDetail;

let details = [
  new TransactionDetail (
    amount, commerceCode, buyOrder
  ),
  new TransactionDetail (
    amount2, commerceCode2, buyOrder2
  ),
];

const createResponse = await WebpayPlus.MallTransaction.create (
  buyOrder,
  sessionId,
  returnUrl,
  details
);
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
 "buy_order": "OrdenCompra12345678",
 "session_id": "sesion1234557545",
 "return_url": "http://www.comercio.cl/webpay/retorno",
 "details": [
     {
         "amount": 10000,
         "commerce_code": 597055555536,
         "buy_order": "OrdenCompraDetalle1234"
     },
     {
        "amount": 12000,
        "commerce_code": 597055555537,
        "buy_order": "OrdenCompraDetalle4321"
     },
 ]
}
`` ''

 <strong> Transaction.create Mall parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | It is the unique code of the purchase order generated by the mall trade.
session_id <br> <i> String </i> |  Session identifier, internal trade use, this value is returned at the end of the transaction. Maximum length: 61
return_url <br> <i> String </i> | Merchant URL, to which Webpay will redirect after the authorization process Maximum length: 256.
details <br> <i> Array </i> | List of objects, one for each different store in the mall that participates in the transaction.
details [] .amount <br> <i> Decimal </i> | Amount of the transaction of a mall store. Maximum 2 decimal places for USD. Maximum length: 10.
details [] .commerce_code <br> <i> String </i> | Trade code assigned by Transbank for the store belonging to the mall to which this transaction corresponds. Length: 12.
details [] .buy_order <br> <i> String </i> | Purchase order from the mall store. This number must be unique for each transaction. Maximum length: 26. The purchase order can have: Numbers, letters, uppercase and lowercase, and the signs <code> & # 124; _ = &%., ~: /? [+! @ ()> - </code>.

 <strong> Response Transaction.create () Mall </strong>

java
response.getToken ();
response.getUrl ();
`` ''

`` `php
$ response-> getToken ();
$ response-> getUrl ();
`` ''

csharp
response.Token;
response.Url;
`` ''

ruby
response.token
response.url
`` ''

python
response.token
response.url
`` ''

javascript
response.token
response.url
`` ''

http
200 OK
Content-Type: application / json

{
 "token": "e9d555262db0f989e49d724b4db0b0af367cc415cde41f500a776550fc5fddd3",
 "url": "https://webpay3gint.transbank.cl/webpayserver/initTransaction"
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Length: 64.
url <br> <i> String </i> | Webpay payment form URL. Maximum length: 256.

### Confirm a mall transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # confirm-a-transaction-mall)


It allows confirming a transaction and obtaining the result of the transaction
once Webpay has resolved your financial authorization.


 <strong> Transaction.commit () Mall </strong>

java
final CommitWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.commit (token);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus \ Transaction;

$ response = Transaction :: commitMall ($ token);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.commit (token);
`` ''

ruby
response = MallTransaction.commit (token)
`` ''

python
response = MallTransaction.commit (token)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.MallTransaction.commit (token);
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token}
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

 <strong> Transaction.commit Mall parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)

 <strong> Transaction.commit Mall Response </strong>

java
response.getAccountingDate ();
response.getBuyOrder ();
final CardDetail cardDetail = response.getCardDetail ();
cardDetail.getCardNumber ();
response.getSessionId ();
response.getTransactionDate ();
response.getVci ();
final List <Detail> details = response.getDetails ();
for (Detail detail: details) {
    detail.getAmount ();
    detail.getAuthorizationCode ();
    detail.getBuyOrder ();
    detail.getCommerceCode ();
    detail.getInstallmentsNumber ();
    detail.getPaymentTypeCode ();
    detail.getResponseCode ();
    detail.getStatus ();
}
`` ''

`` `php
$ response-> getAccountingDate ();
$ response-> getBuyOrder ();
$ card_detail = $ response-> getCardDetail ();
$ card_detail-> getCardNumber ();
$ response-> getSessionId ();
$ response-> getTransactionDate ();
$ response-> getVci ();
$ details = $ response-> getDetails ();
foreach ($ details as $ detail) {
    $ detail-> getAmount ();
    $ detail-> getAuthorizationCode ();
    $ detail-> getBuyOrder ();
    $ detail-> getCommerceCode ();
    $ detail-> getInstallmentsNumber ();
    $ detail-> getPaymentTypeCode ();
    $ detail-> getResponseCode ();
    $ detail-> getStatus ();
}
`` ''

csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
`` ''

ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do | detail |
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
`` ''

python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
`` ''

javascript
response.accounting_date
response.buy_order
cardDetail = response.card_detail
cardDetail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.forEach (detail => {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
});
`` ''

http
200 OK
Content-Type: application / json

{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15: 43: 48.523Z",
  "details": [
    {
      "amount": 10000,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555536",
      "buy_order": "505479072",
      "status": "AUTHORIZED"
  }]
}

`` ''

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order from the mall. Maximum length: 26
session_id <br> <i> String </i> |Session identifier, the same one originally sent by the merchant in `Transaction.create ()`. Maximum length: 61.
card_detail <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
card_detail.card_number <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for merchants authorized by Transbank the full number is sent. Maximum length: 16.
accouting_date <br> <i> String </i> | Authorization date. Length: 4, MMDD format
transaction_date <br> <i> String </i> | Date and time of authorization. Length: 6, format: MMDDHHmm
vci <br> <i> String </i> | Result of cardholder authentication. It can take the value TSY (Authentication successful), TSN (Authentication failed), TO (Maximum time exceeded for authentication), ABO (Aborted authentication by cardholder), U3 (Internal authentication error), NP (Does not participate, probably because a foreign card that does not participate in the 3DSecure program), ACS2 (Foreign Authentication Failed). It can be empty if the transaction was not authenticated. Maximum length: 3. This field is supplementary additional information to the `responseCode` but the merchant ** does not ** have to validate this field. Because new authentication mechanisms are constantly being added that translate into new values for this field that are not necessarily documented. (In the case of international cards that do not provide 3D-Secure, the merchant's decision to accept them or not is made at the merchant's configuration level in Transbank and must be discussed with the merchant's executive)
details <br> <i> Array </i> | List with result of each of the transactions sent in `Transaction.create ()`.
details [] .authorization_code <br> <i> String </i> | Transaction authorization code Maximum length: 6
details [] .payment_type_code <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VD = Sale Debit. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Installments without interest <br> VP = Prepaid Sale.
details [] .response_code <br> <i> String </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br>
details [] .amount <br> <i> Integer format for weight transactions and decimal for dollar transactions. </i> | Amount of the transaction. Maximum length: 10
details [] .installments_amount <br> <i> Number </i> | Amount of each installment. Maximum length: 17
details [] .installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2
details [] .commerce_code <br> <i> String </i> | Store trade code. Length: 12
details [] .buy_order <br> <i> String </i> | Store purchase order. Maximum length: 26
details [] .status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 26
balance <br> <i> Number </i> | Remaining amount for a voided detail. Maximum length: 17

### Get status of a mall transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # get-status-of-a-transaction-mall)

This operation allows you to obtain the status of the transaction at any time. Under normal conditions, it is probably not required to execute, but in the event of an unexpected error, it allows knowing the status and taking the corresponding actions.

 <strong> Transaction.status () Mall </strong>

Get transaction result from a token.

java
final StatusWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.status (token);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus \ Transaction;

$ response = Transaction :: getMallStatus ($ token);
`` ''

csharp
using Transbank.Webpay.WebpayPlus;

var response = MallTransaction.status (token)
`` ''

ruby
response = MallTransaction.status (token)
`` ''

python
response = MallTransaction.status (token)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.MallTransaction.status (token);
`` ''

http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

 <strong> Transaction.status Mall parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)

 <strong> Transaction.status Mall response </strong>

java
response.getAccountingDate ();
response.getBuyOrder ();
final CardDetail cardDetail = response.getCardDetail ();
cardDetail.getCardNumber ();
response.getSessionId ();
response.getTransactionDate ();
response.getVci ();
final List <Detail> details = response.getDetails ();
for (Detail detail: details) {
    detail.getAmount ();
    detail.getAuthorizationCode ();
    detail.getBuyOrder ();
    detail.getCommerceCode ();
    detail.getInstallmentsNumber ();
    detail.getPaymentTypeCode ();
    detail.getResponseCode ();
    detail.getStatus ();
}
`` ''

`` `php
$ response-> getAccountingDate ();
$ response-> getBuyOrder ();
$ card_detail = $ response-> getCardDetail ();
$ card_detail-> getCardNumber ();
$ response-> getSessionId ();
$ response-> getTransactionDate ();
$ response-> getVci ();
$ details = $ response-> getDetails ();
foreach ($ details as $ detail) {
    $ detail-> getAmount ();
    $ detail-> getAuthorizationCode ();
    $ detail-> getBuyOrder ();
    $ detail-> getCommerceCode ();
    $ detail-> getInstallmentsNumber ();
    $ detail-> getPaymentTypeCode ();
    $ detail-> getResponseCode ();
    $ detail-> getStatus ();
}
`` ''

csharp
response.AccountingDate;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.SessionId;
response.TransactionDate;
response.Vci;
var details = response.Details;
foreach (var detail in details) {
    detail.Amount;
    detail.AuthorizationCode;
    detail.BuyOrder;
    detail.CommerceCode;
    detail.InstallmentsNumber;
    detail.PaymentTypeCode;
    detail.ResponseCode;
    detail.Status;
}
`` ''

ruby
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.each do | detail |
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
end
`` ''

python
response.accounting_date
response.buy_order
card_detail = response.card_detail
card_detail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
for detail in details:
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
`` ''

javascript
response.accounting_date
response.buy_order
cardDetail = response.card_detail
cardDetail.card_number
response.session_id
response.transaction_date
response.vci
details = response.details
details.forEach (detail => {
  detail.amount
  detail.authorization_code
  detail.buy_order
  detail.commerce_code
  detail.installments_number
  detail.payment_type_code
  detail.response_code
  detail.status
});
`` ''

http
200 OK
Content-Type: application / json
{
  "buy_order": "415034240",
  "card_detail": {
    "card_number": "6623"
  },
  "accounting_date": "0321",
  "transaction_date": "2019-03-21T15: 43: 48.523Z",
  "vci": "TSY",
  "details": [
    {
      "amount": 10000,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555536",
      "buy_order": "505479072",
      "status": "AUTHORIZED"
  }]
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order from the mall. Maximum length: 26
session_id <br> <i> String </i> |Session identifier, the same one originally sent by the merchant in `Transaction.create ()`. Maximum length: 61.
card_detail <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
card_detail.card_number <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for merchants authorized by Transbank the full number is sent. Maximum length: 16.
accouting_date <br> <i> String </i> | Authorization date. Length: 4, MMDD format
transaction_date <br> <i> String </i> | Date and time of authorization. Length: 6, format: MMDDHHmm
vci <br> <i> String </i> | Result of cardholder authentication. It can take the value TSY (Authentication successful), TSN (Authentication failed), TO (Maximum time exceeded for authentication), ABO (Aborted authentication by cardholder), U3 (Internal authentication error), NP (Does not participate, probably because a foreign card that does not participate in the 3DSecure program), ACS2 (Foreign Authentication Failed). It can be empty if the transaction was not authenticated. Maximum length: 3. This field is supplementary additional information to the `responseCode` but the merchant ** does not ** have to validate this field. Because new authentication mechanisms are constantly being added that translate into new values for this field that are not necessarily documented. (In the case of international cards that do not provide 3D-Secure, the merchant's decision to accept them or not is made at the merchant's configuration level in Transbank and must be discussed with the merchant's executive)
details <br> <i> Array </i> | List with result of each of the transactions sent in `Transaction.create ()`.
details [] .authorization_code <br> <i> String </i> | Transaction authorization code Maximum length: 6
details [] .payment_type_code <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VD = Sale Debit. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Installments without interest <br> VP = Prepaid Sale.
details [] .response_code <br> <i> String </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br>
details [] .amount <br> <i> Integer format for weight transactions and decimal for dollar transactions. </i> | Amount of the transaction. Maximum length: 10
details [] .installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2
details [] .installments_amount <br> <i> Number </i> | Amount of each installment. Maximum length: 17
details [] .commerce_code <br> <i> String </i> | Store trade code. Length: 12
details [] .buy_order <br> <i> String </i> | Store purchase order. Maximum length: 26
details [] .status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 26
balance <br> <i> Number </i> | Remaining amount for a voided detail. Maximum length: 17

### Reverse or void a mall transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # reverse-or-cancel-a-transaction-mall)

This method allows all enabled merchants to reverse or cancel a transaction
that was generated in Webpay Plus Mall.

To cancel a transaction, the `Transaction.refund ()` method must be invoked.

 <strong> Transaction.refund () Mall </strong>

java
final RefundWebpayPlusMallTransactionResponse response = WebpayPlus.MallTransaction.refund (token, buyOrder, commerceCode, amount);
`` ''

`` `php
$ response = Transaction :: refundMall ($ token, $ buy_order, $ commerce_code, $ amount);
`` ''

csharp
var response = Transaction.refund (token, buyOrder, commerceCode, amount);
`` ''

ruby
response = Transaction.refund (token, buy_order, commerce_code, amount)
`` ''

python
response = Transaction.refund (token, buy_order, commerce_code, amount)
`` ''

javascript
const WebpayPlus = require ('transbank-sdk'). WebpayPlus;

const response = await WebpayPlus.MallTransaction.refund (token, buyOrder, commerceCode, amount);
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/refunds
Tbk-Api-Key-Id: 597055555535
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "commerce_code": "597055555536",
  "buy_order": "OrdenCompra12345678",
  "amount": 1000
}
`` ''

 <strong> Transaction.refund Mall parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
buy_order <br> <i> String </i> | Purchase order of the transaction that is required to be canceled. Maximum length: 26.
amount <br> <i> Integer format for weight transactions. Only in case of dollar it accepts two decimal places. </i> | Amount to be canceled or reversed from the transaction. Maximum length: 17.
commerce_code <br> <i> Number </i> | Commercial code of the mall store that carried out the transaction. Length: 12.

 <strong> Transaction.refund Mall response </strong>

java
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getBalance ();
response.getNullifiedAmount ();
response.getResponseCode ();
response.getType ();
`` ''

`` `php
$ response-> getAuthorizationCode ();
$ response-> getAuthorizationDate ();
$ response-> getBalance ();
$ response-> getNullifiedAmount ();
$ response-> getResponseCode ();
$ response-> getType ();
`` ''

csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.Balance;
response.NullifiedAmount;
response.ResponseCode;
response.Type;
`` ''

ruby
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

python
response.authorization_code
response.authorization_date
response.balance
response.nullified_amount
response.response_code
response.type
`` ''

javascript
  response.authorization_code
  response.authorization_date
  response.balance
  response.nullified_amount
  response.response_code
  response.type
`` ''

http
200 OK
Content-Type: application / json
{
  "type": "NULLIFIED",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
type <br> <i> String </i> | Type of refund (REVERSED or NULLIFIED). If it is REVERSED, no transaction data will be returned (authorization code, etc). Maximum length: 10
authorization_code <br> <i> String </i> | (Only if it is NULLIFIED) Authorization code of the cancellation. Maximum length: 6
authorization_date <br> <i> String </i> | (Only if it is NULLIFIED) Date and time of the authorization.
balance <br> <i> Decimal </i> | (Only if it is NULLIFIED) Updated balance of the transaction (considers the sale minus the canceled amount). Maximum length: 17
nullified_amount <br> <i> Decimal </i> | (Only if it is NULLIFIED) Amount canceled. Maximum length: 17
response_code <br> <i> Number </i> | (Only if NULLIFIED) Reverse / abort result code. If it is successful it is 0, otherwise the reversal / cancellation was not performed. Maximum length: 2

In the event of an error, the following common error codes may appear for the `Transaction.refund ()` method:

Code  | Description
------  | -----------
304     | Null input field validation
245     | Commercial code does not exist
22      | The trade is not active
316     | The indicated merchant does not correspond to the certificate or is not a child of the MALL merchant in case of MALL transactions
308     | Operation not allowed
274     | Transaction not found
16      | The transaction does not allow cancellation
292     | The transaction is not authorized
284     | Cancellation period exceeded
310     | Transaction previously voided
311     | Amount to be canceled exceeds the balance available to cancel
312     | Generic error for overrides
315     | Authorizer error
53      | The transaction does not allow partial cancellation of transactions with fees

### Capture a mall transaction
You can check more details of this operation in [its documentation] (/ documentation / webpay-plus # capture-a-transaction-mall)

Allows you to request from Webpay the deferred capture of a transaction with
authorization and without simultaneous capture.

 <strong> Transaction.capture () </strong>

java
final WebpayPlusMallTransactionCaptureResponse response = WebpayPlus.MallDeferredTransaction.capture (token, childCommerceCode, buyOrder, authorizationCode, amount);
`` ''

`` `php
use Transbank \ Webpay \ WebpayPlus;

$ response = Transaction :: captureMall ($ childCommerceCode, $ token, $ buyOrder, $ authorizationCode, $ captureAmount);
`` ''

csharp
var response = Transbank.Webpay.WebpayPlus.MallDeferredTransaction.Capture (token, childCommerceCode, buyOrder, authorizationCode, captureAmount);
`` ''

ruby
response = Transbank :: Webpay :: WebpayPlus :: MallDeferredTransaction :: capture (
  token: @token,
  child_commerce_code: @child_commerce_code,
  buy_order: @buy_order,
  authorization_code: @auth_code,
  capture_amount: @amount
)
`` ''

python
response = MallDeferredTransaction.capture (
  token = token, capture_amount = amount, commerce_code = commerce_code,
  buy_order = buy_order, authorization_code = authorization_code
)
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/capture
Tbk-Api-Key-Id: 597055555531
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "commerce_code": "597055555531",
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
`` ''

 <strong> Transaction.capture parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
commerce_code <br> <i> Number </i> | (Optional, only use in Mall case) Child store that carried out the transaction. Length: 12.
buy_order <br> <i> String </i> | Purchase order of the transaction that is required to be captured. Maximum length: 26.
authorization_code <br> <i> String </i> |Authorization code of the transaction that is required to capture Maximum length: 6.
capture_amount <br> <i> Decimal </i> |Amount to be captured. Maximum length: 17.

 <strong> Transaction.capture response </strong>

java
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getCapturedAmount ();
response.getResponseCode ();
`` ''

`` `php
$ response-> getAuthorizationCode ();
$ response-> getAuthorizationDate ();
$ response-> getCapturedAmount ();
$ response-> getResponseCode ();
`` ''

csharp
response.AuthorizationCode;
response.AuthorizationDate;
response.CapturedAmount;
response.ResponseCode;
`` ''

ruby
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

python
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
`` ''

http
200 OK
Content-Type: application / json
{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "captured_amount": 1000,
  "response_code": 0
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Maximum length: 64
authorization_code <br> <i> String </i> | Authorization code for deferred capture. Maximum length: 6
authorization_date <br> <i> String </i> | Date and time of authorization.
captured_amount <br> <i> Decimal </i> | Amount captured. Maximum length: 6
response_code <br> <i> Number </i> | Capture result code. If it is successful it is 0, otherwise the capture was not carried out. Maximum length: 2

In case of error the following unique codes of the method may appear
`Transaction.capture ()`:

Code | Description
------ | -----------
304 | Null input field validation
245 | Commercial code does not exist
22 | The trade is not active
316 | The store indicated does not correspond to the certificate or is not a child of the Mall store in case of MALL transactions
308 | Operation not allowed
274 | Transaction not found
16 | The transaction is not deferred capture
292 | The transaction is not authorized
284 | Capture period exceeded
310 | Transaction previously reversed
309 | Transaction previously captured
311 | Amount to capture exceeds the authorized amount
315 | Authorizer error

## Error codes and messages

When making any request to the REST API, in addition to the response data, one of the following HTTP response status codes will be included depending on the result obtained:

### Successful application

When the requested operation is executed correctly, these HTTP statuses can be received:

HTTP status code | Description
------ | -----------
200 | The operation has been executed successfully
204 | DELETE operation has been executed successfully

 <strong> Error codes </strong>

All errors reported by the Webpay REST API display a JSON message with a description of the error.

json
{
  "error_message": "token is required"
}
`` ''

HTTP status code | Description
------ | -----------
400 | The JSON message is invalid. It may be that it does not correspond to a well structured message or that it contains an unexpected field.
401 | Not authorized. Invalid API Key and / or API Secret
404 | The transaction has not been found.
405 | Method not allowed.
406 | It was not possible to process the response in the format that the client indicates.
415 | Message type not allowed.
422 | The request could not be processed either by data validations or by business logic.
500 | An unexpected error has occurred.

## Put into Production

You can review the process necessary to operate in the production environment in [the documentation] (/ documentation / how to start # put-into-production)

### Configuration for production using the SDKs

If you are using an official Transbank SDK, then you must follow the following steps.

 <aside class="warning">
Never leave your trade code and shared secret directly in your code, we recommend using environment variables or another method that allows you to keep your credentials safe.
 </aside>

Check the [this section] (/ documentation / how to get started # b-using-the-sdk) of the documentation to see the code needed to configure your own trading code and Api Key Secret.

