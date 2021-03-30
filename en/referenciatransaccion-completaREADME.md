# Complete Transaction

___

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

javascript
// Host: https://webpay3g.transbank.cl
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

javascript
// Host: https://webpay3gint.transbank.cl
`` ''

### Cards and test users

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

javascript
// Tbk-Api-Key-Id: Trade code
// Tbk-Api-Key-Secret: Secret key
// Content-Type: application / json
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
> environment.

## Complete Transaction

 <aside class="warning">
This product has stricter commercial requirements than the rest of the products.
Do not start the integration if they have not yet completed the commercial affiliation.
 </aside>

### Create a Full Transaction

You can check more details of this operation in [its documentation] (/ documentation / complete-transaction # create-a-transaction)

To create a complete transaction, just call the `Transaction.create ()` method.

 <strong> Transaction.create () </strong>

It allows to initialize a complete transaction in Webpay. In response to the
invocation generates a token that uniquely represents a transaction.

 <aside class="notice">
It is important to consider that once this method is invoked, the token that is
delivered has a reduced life span of 5 minutes, after this the
token is expired and cannot be used in a payment.
 </aside>

java
final FullTransactionCreateResponse response = FullTransaction.Transaction.create (
  buyOrder, // orderCompra12345678
  sessionId, // sesion1234564
  amount, // 10000
  cardNumber, // 123
  cardExpirationDate, // 4239000000000000
  cvv // 10/22
);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

$ response = Transaction :: create (
  $ buy_order, // orderCompra12345678
  $ session_id, // sesion1234564
  $ amount, // 10000
  $ cvv, // 123
  $ card_number, // 4239000000000000
  $ card_expiration_date // 10/22
);
`` ''

csharp
FullTransaction.Create (
  buyOrder: buy_order, // orderCompra12345678
  sessionId: session_id, // sesion1234564
  amount: amount, // 10000
  cvv: cvv, // 123
  cardNumber: card_number, // 4239000000000000
  cardExpirationDate: card_expiration_date // 10/22
);
`` ''

ruby
Transbank :: Complete Transaction :: Transaction :: create (
  buy_order: 'orderCompra12345678',
  session_id: 'sesion1234564',
  amount: 10000,
  card_number: 4239000000000000,
  cvv: 123,
  card_expiration_date: '22 / 10 '
)
`` ''

python
from transbank.transaccion_completa.transaction import Transaction
# ...
Transaction.create (
    buy_order = 'orderCompra12345678',
    session_id = 'sesion1234564',
    amount = 10000,
    card_number = 4239000000000000,
    cvv = 123,
    card_expiration_date = '22 / 10 '
)
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234564",
  "amount": 10000,
  "cvv": 123,
  "card_number": "4239000000000000",
  "card_expiration_date": "10/22"
}
`` ''

javascript
const response = await Complete Transaction.transaction.create (buyOrder, sessionId, amount, cvv, cardNumber, cardExpirationDate);

`` ''


 <strong> Parameters Transaction.create </strong>

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Store purchase order. This number must be unique for each transaction. Maximum length: 26. The purchase order can have: Numbers, letters, upper and lower case, and the signs <code> & # 124; _ = &%., ~: /? [+! @ ()> - </code>
session_id <br> <i> String </i> | Session identifier, internal trade use, this value is returned at the end of the transaction. Maximum length: 61
amount <br> <i> Decimal </i> | Amount of the transaction. Maximum 2 decimal places for USD. Maximum length: 17
cvv <br> <i> String </i> | (Optional) Code used as a security method in transactions in which the card is not physically present. Maximum length: 4. It should not be sent for businesses with the option `without cvv` enabled.
card_number <br> <i> String </i> | Card number. Maximum length: 16
card_expiration_date <br> <i> String </i> | Expiration date of the card with which the transaction is made. Maximum length: 5

 <strong> Transaction.create response </strong>

java
response.getToken ();
`` ''

`` `php
$ response-> getToken ();
`` ''

csharp
response.Token;
`` ''

ruby
response.token
`` ''

python
response.token
`` ''

http
200 OK
Content-Type: application / json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
}
`` ''

javascript
response.token
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token of the transaction. Length: 64.

 <strong> Mode without cvv </strong>

For product modality Complete transaction `without CVV`, this field ** must not ** be sent.

http
200 OK
Content-Type: application / json
{
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234564",
  "amount": 10000,
  "card_number": "4239000000000000",
  "card_expiration_date": "10/22"
}
`` ''

### Quota inquiry

To consult the value of the fees that the cardholder will pay in a
complete transaction, you need to call the `Transaction.installments ()` method

 <strong> Transaction.installments () </strong>

Operation that allows obtaining the amount of the installment from the number of installments.
The id of the query that the cardholder selects must be informed in the
invocation of confirmation.

java
final FullTransactionInstallmentsResponse response = FullTransaction.Transaction.installment (token, installments_number);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

$ installments = Transaction :: installments ($ token_ws, $ installments_number);
`` ''

csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Installments (
  token,
  installments_number
);
`` ''

ruby
Transbank :: Complete Transaction :: Transaction :: installments (
   token: token,
   installments_number: installments_number
)
`` ''

python
from transbank.transaccion_completa.transaction import Transaction

Transaction.installments (
  token = token,
  installments_number = installments_number
)
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/installments

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "installments_number": 10
}
`` ''

javascript
const response = await Complete Transaction.Transaction.installments (token, installmentsNumber);
`` ''

 <strong> Parameters Transaction.installments </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2

 <strong> Response Transaction.installments </strong>

java
response.getInstallmentsAmount ();
response.getIdQueryInstallments ();
DeferredPeriod deferredPeriod = response.getDeferredPeriods () [0];
deferredPeriod.getAmount ();
deferredPeriod.getPeriod ();
`` ''

`` `php
response-> getInstallmentsAmount ();
response-> getIdQueryInstallments ();
response-> getDeferredPeriods ();
`` ''

csharp
response.InstallmentsAmount;
respone.IdQueryInstallments;
response.DeferredPeriods;
`` ''

ruby
response.installments_amount
response.id_query_installments
response.deferred_periods
`` ''

python
response.installments_amount
response.id_query_installments
response.deferred_periods
`` ''

javascript
response.installments_amount
response.id_query_installments
response.deferred_periods
`` ''

If the merchant does not have deferred periods configured, the response of `deferred_periods` will be` [] `:

http
200 OK
Content-Type: application / json

{
  "installments_amount": 3334,
  "id_query_installments": 11,
  "deferred_periods": [
    {
      "amount": 1000,
      "period": 1
    }
  ]
}
`` ''

Name <br> <i> type </i> | Description
------   | -----------
installments_amount <br> <i> String </i> |Amount of each installment. Length: 17.
id_query_installments <br> <i> String </i> |Quota identifier. Length: 19.
deferred_periods <br> <i> Array </i> | Arrangement with deferred periods.
deferred_periods [] .amount <br> <i> String </i> |Amount. Length: 17.
deferred_periods [] .period <br> <i> String </i> |Period index. Length: 2.

### Transaction confirmation

Once the transaction has started and the amount of the fees has been consulted, you can
confirm and get the result of a complete transaction using the method
`Transaction.commit ()`.

 <strong> Transaction.commit () </strong>

Operation that allows confirming a transaction. Returns the status of the
transaction.

java
import cl.transbank.transaccioncompleta.FullTransaction;

final FullTransactionCommitResponse response = FullTransaction.Transaction.commit (
  token, idQueryInstallments, deferredPeriodIndex, gracePeriod
);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;
// ...
Transaction :: commit (
  $ token_ws,
  $ id_query_installments,
  $ deferred_period_index,
  $ grace_period
);
`` ''

csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Commit (
  token, idQueryInstallments, deferredPeriodsIndex, gracePeriods
);
`` ''

ruby
Transbank :: Complete Transaction :: Transaction :: commit (
  token: token,
  id_query_installments: id_query_installments,
  deferred_period_index: deferred_period_index,
  grace_period: grace_period
)
`` ''

python
from transbank.transaccion_completa.transaction import Transaction

Transaction.commit (
  token = token,
  id_query_installments = id_query_installments,
  deferred_period_index = deferred_period_index,
  grace_period = grace_period
)
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token}

Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "id_query_installments": 15,
  "deferred_period_index": 1,
  "grace_period": false
}
`` ''

javascript
const response = await TransactionComplete.Transaction.commit (
    token, idQueryInstallments, deferredPeriodIndex, gracePeriod);
`` ''

 <strong> Transaction.commit parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
id_query_installments <br> <i> Number </i> | (Optional) Quota identifier. Maximum length: 19. Only send if payment is in installments
deferred_period_index <br> <i> Number </i> | (Optional) Amount of deferred period. Maximum length: 2. Only send if payment is in installments
grace_period <br> <i> Boolean </i> | (Optional) Grace period indicator. Only send if the payment is in installments

 <strong> Transaction.commit response </strong>

java
response.getAccountingDate ();
response.getAmount ();
response.getAuthorizationCode ();
response.getBuyOrder ();
CardDetail cardDetail = response.getCardDetail ();
cardDetail.getCardNumber ();
response.getInstallmentsAmount ();
response.getInstallmentsNumber ();
response.getPaymentCodeType ();
response.getResponseCode ();
response.getSessionId ();
response.getTransactionDate ();
`` ''

`` `php
response-> getAccountingDate ();
response-> getAmount ();
response-> getAuthorizationCode ();
response-> getBuyOrder ();
cardDetail = response-> getCardDetail ();
cardDetail-> getCardNumber ();
response-> getInstallmentsAmount ();
response-> getInstallmentsNumber ();
response-> getPaymentCodeType ();
response-> getResponseCode ();
response-> getSessionId ();
response-> getTransactionDate ();
`` ''

csharp
response.AccountingDate;
response.Amount;
response.AuthorizationCoda;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.PaymentTypeCode;
response.ResponseCode;
response.SessionId;
response.Status;
response.TransactionDate;
`` ''

ruby
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
`` ''

python
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
`` ''

http
200 OK
Content-Type: application / json

{
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
    "card_number": "1234"
  },
  "accounting_date": "0320",
  "transaction_date": "2019-03-20T20: 18: 20Z",
  "authorization_code": "877550",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0,
  "installmentsAmount": 0
}
`` ''

javascript
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
`` ''

Name <br> <i> type </i> | Description
------   | -----------
amount <br> <i> Number </i> | Amount of the transaction. Only in case of dollar accepts two decimals. Maximum length: 17
status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
buy_order <br> <i> String </i> | Purchase order number. Maximum length: 26
session_id <br> <i> String </i> | Session ID of the purchase. Maximum length: 61
card_detail <br> <i> cardDetail </i> | Object that contains information about the card used by the cardholder.
card_number <br> <i> String </i> | The last 4 digits of the card used in the transaction, only if the merchant has configured to receive the card number. Maximum length: 19
accounting_date <br> <i> String </i> | Accounting date of the transaction in MMYY format.
transaction_date <br> <i> ISO8601 </i> | Date of the transaction.
authorization_code <br> <i> String </i> | Authorization code of the payment transaction. Maximum length: 6
payment_type_code <br> <i> String </i> | Indicates the type of card used. Maximum length: 2 <br> DV = Sale Debit. <i> (Coming soon) </i> <br> VN = Normal Sale. <br> VP = Prepaid Sale. <br> <i> (Coming soon) </i> <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Interest-free installments
response_code <br> <i> Number </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes)
installments_amount <br> <i> Number </i> | Amount of the fee. It is sent only if it has a quota value. <br> <i> Maximum length: 17 </i>
installments_number <br> <i> Number </i> | Number of installments of the transaction. <br> <i> Maximum length: 2 </i>
prepaid_balance <br> <i> Number </i> | Prepaid card balance. Sent only if balance is reported. <br> <i> Maximum length: 17 </i>

### Check the status of a complete transaction

This operation allows you to obtain the status of the transaction at any time. Under normal conditions, it is probably not required to execute, but in the event of an unexpected error, it allows knowing the status and taking the corresponding actions.

 <strong> Transaction.status () </strong>

Get transaction result from a token.

java
import cl.transbank.transaccioncompleta.FullTransaction;

final FullTransactionStatusResponse response = FullTransaction.Transaction.status (token);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

Transaction :: getStatus ($ token_ws);
`` ''

csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Status (token);
`` ''

ruby
Transbank :: TransaccionCompleta :: Transaction :: status (token: token)
`` ''

python
from transbank.transaccion_completa.transaction import Transaction

Transaction.status (token = token)
`` ''

http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

javascript
const response = await TransactionComplete.Transaction.status (token);
`` ''

 <strong> Transaction.status parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)

 <strong> Transaction.status response </strong>

java
response.getAccountingDate ();
response.getAmount ();
response.getAuthorizationCode ();
response.getBuyOrder ();
CardDetail cardDetail = response.getCardDetail ();
cardDetail.getCardNumber ();
response.getInstallmentsAmount ();
response.getInstallmentsNumber ();
response.getPaymentCodeType ();
response.getResponseCode ();
response.getSessionId ();
response.getTransactionDate ();
`` ''

`` `php
response-> getAccountingDate ();
response-> getAmount ();
response-> getAuthorizationCode ();
response-> getBuyOrder ();
cardDetail = response-> getCardDetail ();
cardDetail-> getCardNumber ();
response-> getInstallmentsAmount ();
response-> getInstallmentsNumber ();
response-> getPaymentCodeType ();
response-> getResponseCode ();
response-> getSessionId ();
response-> getTransactionDate ();
`` ''

csharp
response.AccountingDate;
response.Amount;
response.AuthorizationCoda;
response.BuyOrder;
var cardDetail = response.CardDetail;
cardDetail.CardNumber;
response.InstallmentsAmount;
response.InstallmentsNumber;
response.PaymentTypeCode;
response.ResponseCode;
response.SessionId;
response.Status;
response.TransactionDate;
`` ''

ruby
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
`` ''

python
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
`` ''

http
200 OK
Content-Type: application / json

{
  "amount": 10000,
  "status": "AUTHORIZED",
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234557545",
  "card_detail": {
    "card_number": "1234"
  },
  "accounting_date": "0320",
  "transaction_date": "2019-03-20T20: 18: 20Z",
  "authorization_code": "877550",
  "payment_type_code": "VN",
  "response_code": 0,
  "installments_number": 0
}
`` ''

javascript
response.amount
response.status
response.buy_order
response.session_id
response.card_number
response.accounting_date
response.transaction_date
response.authorization_code
response.payment_type_code
response.response_code
response.installments_number
response.installments_amount
response.balance
`` ''

Name <br> <i> type </i> | Description
------   | -----------
amount <br> <i> Number </i> | Amount of the transaction. Only in case of dollar accepts two decimals. Maximum length: 17
status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
buy_order <br> <i> String </i> | Purchase order number. Maximum length: 26
session_id <br> <i> String </i> | Session ID of the purchase. Maximum length: 61
card_detail <br> <i> cardDetail </i> | Object that contains information about the card used by the cardholder.
card_detail.card_number <br> <i> String </i> | The last 4 digits of the card used in the transaction. Maximum length: 19
accounting_date <br> <i> String </i> | Accounting date of the transaction.
transaction_date <br> <i> ISO8601 </i> | Date of the transaction.
authorization_code <br> <i> String </i> | Authorization code of the payment transaction. Maximum length: 6
payment_type_code <br> <i> String </i> | Indicates the type of card used. Maximum length: 2 <br> DV = Sale Debit. <i> (Coming soon) </i> <br> VN = Normal Sale. <br> VP = Prepaid Sale. <br> <i> (Coming soon) </i> <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Interest-free installments
response_code <br> <i> Number </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes) <br>
installments_number <br> <i> Number </i> | Number of installments of the transaction. Maximum length: 2
installments_amount <br> <i> Number </i> | Amount of the fee. It is sent only if it has a quota value. <br> <i> Maximum length 17 </i>
balance <br> <i> Number </i> | Remaining amount. Maximum length: 17. This field only comes when the transaction was voided
prepaid_balance <br> <i> Number </i> | Prepaid card balance. Sent only if balance is reported. <br> <i> Maximum length: 17 </i>

### Reverse or Void a Payment Complete Transaction

This method allows all enabled merchants to reverse or cancel a transaction
complete. The method allows to generate the refund of all or part of the amount of
a transaction depending on the following business logic the invocation to
this operation will generate a reversal or an annulment:

* If the amount sent is less than the total amount then a partial cancellation will be executed.
* If the amount sent is equal to the total, then an annulment or reversal will be evaluated. It will be reversed if the time to execute it has not expired, otherwise an annulment will be executed.

Cancellation can be made a maximum of 90 days after the date of the
original transaction.

You can [read more about the cancellation in the information of the
Webpay product] (/ product / webpay # cancellations) to know
more details and restrictions.

To cancel a transaction, the `Transaction.refund ()` method must be invoked.

It allows generating a refund of all or part of the amount of a complete transaction.
Depending on the following business logic, the invocation of this operation will generate a reverse or an abort:

* If a value is specified in the "amount" field, an annulment will always be executed.
* If the maximum time to execute a reverse is exceeded, an annulment will be executed.
* If none of the above cases have occurred, a reversal will be executed.

 <strong> Transaction.refund () </strong>

It allows you to request from Webpay the cancellation of a transaction made previously and that is currently in force.

java
import cl.transbank.transaccioncompleta.FullTransaction;

final FullTransactionRefundResponse response = FullTransaction.Transaction.refund (token, amount);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

Transaction :: refund ($ token, $ amount);
`` ''

csharp
using Transbank.Webpay.TransaccionCompleta;

FullTransaction.Refund (token, amount);
`` ''

ruby
Transbank :: TransaccionCompleta :: Transaction :: refund (token: token, amount: amount)
`` ''

python
from transbank.transaccion_completa.transaction import Transaction

Transaction.refund (token = token, amount = amount)
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/refunds
Tbk-Api-Key-Id: 597055555530
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
 "amount": 1000
}
`` ''

javascript
const response = await TransactionComplete.Transaction.refund (token, amount);
`` ''

 <strong> Transaction.refund parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
amount <br> <i> Integer format for weight transactions. Only in case of dollar it accepts two decimal places. </i> |  Amount to be canceled or reversed from the transaction. Maximum length: 17.

 <strong> Transaction.refund response </strong>

java
response.getType ();
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getNullifiedAmount ();
response.getBalance ();
response.getResponse ();
`` ''

`` `php
response-> getType ();
response-> getAuthorizationCode ();
response-> getAuthorizationDate ();
response-> getNullifiedAmount ();
response-> getBalance ();
response-> getResponse ();
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
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
`` ''

python
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
`` ''

http
200 OK
Content-Type: application / json
{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
`` ''

javascript
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
`` ''

Name <br> <i> type </i> | Description
------   | -----------
type <br> <i> String </i> | Type of refund (REVERSE. NULLIFY). Maximum length: 10
authorization_code <br> <i> String </i> | Cancellation authorization code. Maximum length: 6. It only comes in case of cancellation.
authorization_date <br> <i> String </i> | Date and time of authorization. It only comes in case of cancellation.
nullified_amount <br> <i> Decimal </i> | Amount canceled. Maximum length: 17. It only comes in case of cancellation.
balance <br> <i> Decimal </i> | Updated balance of the transaction (considers the sale minus the canceled amount). Maximum length: 17. It only comes in case of cancellation.
response_code <br> <i> Number </i> | Cancellation result code. If it is successful it is 0, otherwise the cancellation was not carried out. Maximum length: 2. It only comes in case of cancellation.

## Deferred Capture

Merchants that are configured to operate with deferred capture must execute the capture method to charge the cardholder.

**Valid for :**

* Webpay Plus Deferred Capture
* Full Transaction Deferred Capture

### Execute Deferred Capture Full Transaction

You can [read more about the capture in the information of the
Webpay product] (/ product / webpay # authorization-and-capture)
for more details and restrictions.

To make this explicit capture, the `Transaction.capture ()` method must be used.

 <strong> Transaction.capture () </strong>

Allows you to request from Webpay the deferred capture of a transaction with
authorization and without simultaneous capture.

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/capture
Tbk-Api-Key-Id: 597055555531
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "buy_order": "415034240",
  "authorization_code": "12345",
  "capture_amount": 1000
}
`` ''

javascript
const response = Complete Transaction.DeferredTransaction.capture (
  token, buyOrder, authorizationCode, amount
);
`` ''
 <strong> Transaction.capture parameters </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
buy_order <br> <i> String </i> | Purchase order of the transaction that is required to be captured. Maximum length: 26.
authorization_code <br> <i> String </i> |Authorization code of the transaction that is required to capture Maximum length: 6.
capture_amount <br> <i> Decimal </i> |Amount to be captured. Maximum length: 17.

 <strong> Transaction.capture response </strong>

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

javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
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

Code  | Description
------  | -----------
304     | Null input field validation
245     | Commercial code does not exist
22      | The trade is not active
316     | The store indicated does not correspond to the certificate or is not a child of the Mall store in case of MALL transactions
308     | Operation not allowed
274     | Transaction not found
16      | The transaction is not deferred capture
292     | The transaction is not authorized
284     | Capture period exceeded
310     | Transaction previously reversed
309     | Transaction previously captured
311     | Amount to capture exceeds the authorized amount
315     | Authorizer error


## Complete Mall Transaction

 <aside class="warning">
This product has stricter commercial requirements than the rest of the products.
Do not start the integration if they have not yet completed the commercial affiliation.
 </aside>

You can review more details of this operation in [its documentation] (/ documentation / transaction-complete #)

### Create a Complete Mall Transaction

To create a Mall Complete Transaction, just call the `Transaction.create ()` method.

 <strong> Transaction.create () Complete Mall </strong>

It allows to initialize a Complete Mall transaction in Webpay. In response to the
invocation generates a token that uniquely represents a transaction.

 <aside class="notice">
It is important to consider that once this method is invoked, the token that is
delivered has a reduced life span of 5 minutes, after this the
token is expired and cannot be used in a payment.
 </aside>

java
MallTransactionCreateDetails transactionDetails = MallTransactionCreateDetails.build ()
  .add (amountMallOne, commerceCodeMallOne, buyOrderMallOne, installmentsNumberMallOne)
  .add (amountMallTwo, commerceCodeMallTwo, buyOrderMallTwo, installmentsNumberMallTwo);

final MallFullTransactionCreateResponse response = MallFullTransaction.Transaction.create (
  buyOrder, // orderCompra12345678
  sessionId, // sesion1234564
  cardNumber, // 4239000000000000
  cardExpirationDate, // 10/22
  transactionDetails
);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ MallTransaccionCompleta;

$ transaction_details = [
  {
      "amount": 10000,
      "commerce_code": 597055555552,
      "buy_order": "123456789"
  },
  {
     "amount": 12000,
     "commerce_code": 597055555553,
     "buy_order": "123456790"
  },
];

$ response = MallTransaction :: create (
  $ buy_order, // orderCompra12345678
  $ session_id, // sesion1234564
  $ card_number, // 4239000000000000
  $ card_expiration_date, // 10/22
  $ transaction_details
);
`` ''

csharp
using Transbank.Webpay.TransaccionCompletaMall;

var transactionDetails = new List <CreateDetails> ();
transactionDetails.Add (new CreateDetails (
    amountMallOne,
    commerceCodeMallOne,
    buyOrderMallOne
));
transactionDetails.Add (new CreateDetails (
    amountMallTwo,
    ComerceCodeMallTwo,
    buyOrderMallTwo
));

var response = MallFullTransaction.Create (
  buyOrder,
  sessionId,
  cardNumber,
  cardExpirationDate,
  transactionDetails
);
`` ''

ruby
details = [
  {
    amount: 10000,
    commerce_code: 597055555552,
    buy_order: '123456789'
  },
  {
    amount: 12000,
    commerce_code: 597055555553,
    buy_order: '123456790'
  }
]
response = Transbank :: Complete Transaction :: MallTransaction :: create (
  buy_order: 'orderCompra12345678',
  session_id: 'sesion1234564',
  card_number: 4239000000000000,
  card_expiration_date: '22 / 10 ',
  details: details
)
`` ''

python
details = [
  {
      'commerce_code': 597055555552,
      'buy_order': '123456789',
      'amount': 10000
  },
  {
      'commerce_code': 597055555553,
      'buy_order': '123456790',
      'amount': 12000
  }
]

response = Transaction.create (
  buy_order = buy_order,
  session_id = session_id,
  card_number = card_number, card_expiration_date = card_expiration_date, details = details
)
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "buy_order": "OrdenCompra12345678",
  "session_id": "sesion1234564",
  "card_number": "4239000000000000",
  "card_expiration_date": "10/22",
  "details": [
    {
      "amount": 10000,
      "commerce_code": "597055555552",
      "buy_order": "123456789"
    },
    {
      "amount": 10000,
      "commerce_code": "597055555553",
      "buy_order": "123456790"
    }
  ]
}
`` ''

javascript
const details = [
  new TransactionDetail (amount, commerceCode, childBuyOrder),
  new TransactionDetail (amount2, commerceCode2, childBuyOrder2)
];

const response = await Complete Transaction.MallTransaction.create (
  parentBuyOrder,
  sessionId,
  cvv,
  cardNumber,
  cardExpirationDate,
  details
);
`` ''

 <strong> Parameters Transaction.create Complete Mall </strong>

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | It is the unique code of the purchase order generated by the mall trade. Maximum length: 26
session_id <br> <i> String </i> |  Session identifier, internal trade use, this value is returned at the end of the transaction. Maximum length: 61
card_number <br> <i> String </i> | Card number with which the transaction must be made. Maximum length: 19
card_expiration_date <br> <i> String </i> | Expiration date of the card with which the transaction is made. Maximum length: 5
details <br> <i> Array </i> | List of objects, one for each different store in the mall that participates in the transaction.
details [] .amount <br> <i> Decimal </i> | Amount of the transaction of a mall store. Maximum 2 decimal places for USD. Maximum length: 17.
details [] .commerce_code <br> <i> String </i> | Trade code assigned by Transbank for the store belonging to the mall to which this transaction corresponds. Length: 12.
details [] .buy_order <br> <i> String </i> | Purchase order from the mall store. This number must be unique for each transaction. Maximum length: 26. The purchase order can have: Numbers, letters, uppercase and lowercase, and the signs <code> & # 124; _ = &%., ~: /? [+! @ ()> - </code>. Maximum length: 26

 <strong> Mall Completes Transaction.create Response </strong>

java
response.getToken ();
`` ''

`` `php
response-> getToken ();
`` ''

csharp
response.Token;
`` ''

ruby
response.token
`` ''

python
response.token
`` ''

http
200 OK
Content-Type: application / json

{
  "token": "e074d38c628122c63e5c0986368ece22974d6fee1440617d85873b7b4efa48a3",
}
`` ''

javascript
response.token
`` ''

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> |Token identifier of the transaction. Length: 64.

### Complete Mall fee consultation

To consult the value of the fees that the cardholder will pay in each transaction within the Complete Mall transaction, it is necessary to call the `Transaction.installments ()` method

 <strong> Transaction.installments () Complete Mall </strong>

Operation that allows obtaining the amount of the installment from the number of installments.
The id of the query that the cardholder selects must be informed in the
invocation of confirmation.

java
MallFullTransactionInstallmentsDetails installmentsDetails = MallFullTransactionInstallmentsDetails.build (). Add (commerceCode, buyOrder, installmentsNumber);
final MallFullTransactionInstallmentsResponse response = MallFullTransaction.Transaction.installment (token, installmentsDetails);
`` ''

`` `php
$ installments_details = [
  {
    "commerce_code": 597055555552,
    "buy_order": "123456789",
    "installments_number": 2
  },
  {
    "commerce_code": 597055555553,
    "buy_order": "123456790",
    "installments_number": 2
  },
];

$ res = MallTransaction :: installments (
  $ token,
  $ installments_details
);
`` ''

csharp
using Transbank.Webpay.TransaccionCompletaMall;

var installmentsDetails = new List <MallInstallmentsDetails> ();
installmentsDetails.Add (new CreateDetails (
    commerceCodeMallOne,
    buyOrderMallOne,
    installmentsNumberMallOne
));
installmentsDetails.Add (new CreateDetails (
    ComerceCodeMallTwo,
    buyOrderMallTwo,
    installmentsNumberMallTwo
));

var response = MallFullTransaction.Installments (
  token, installmentDetails
);
`` ''

ruby
installment_details = [
  {
    commerce_code: 597055555552,
    buy_order: '123456789',
    installments_number: 2
  },
  {
    commerce_code: 597055555553,
    buy_order: '123456790',
    installments_number: 2
  },
]

response = Transbank :: Complete Transaction :: MallTransaction :: installments (token, installment_details)
`` ''

python
from transbank.transaccion_completa_mall.transaction import Transaction

details = [
  {
    'commerce_code': 597055555552,
    'buy_order': '123456789',
    'installments_number': 2
  },
  {
    'commerce_code': 597055555553,
    'buy_order': '123456790',
    'installments_number': 2
  }
]

response = Transaction.installments (token = token, details = details)
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/installments

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "commerce_code": 597055555552,
  "buy_order": "123456789",
  "installments_number": 10
}
`` ''

javascript
const details = [
  new InstallmentDetail (childCommerceCode, childBuyOrder, installmentsNumber),
  new InstallmentDetail (childCommerceCode2, childBuyOrder2, installmentsNumber2)
  ];

const response = await Complete Transaction.MallTransaction.installments (
  token,
  details
);
`` ''

 <strong> Parameters Transaction.installments Complete Mall </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
commerce_code <br> <i> String </i> | Trade code assigned by Transbank for the store belonging to the mall to which this transaction corresponds. Length: 12
buy_order <br> <i> String </i> | Purchase order from the mall store. Maximum length: 26
installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2

 <strong> Complete Transaction.installments Response Mall </strong>

java
response.getInstallmentsAmount ();
response.getIdQueryInstallments ();
DeferredPeriod deferredPeriod = response.getDeferredPeriods () [0];
deferredPeriod.getAmount ();
deferredPeriod.getPeriod ();
`` ''

`` `php
$ response-> getInstallmentsAmount ();
$ response-> getIdQueryInstallments ();
$ deferredPeriod = response-> getDeferredPeriods () [0];
$ deferredPeriod-> getAmount ();
$ deferredPeriod-> getPeriod ();
`` ''

csharp
response.InstallmentsAmount;
response.IdQueryInstallments;
var deferredPeriod = response.DeferredPeriods [0];
deferredPeriod.Amount;
deferredPeriod.Period;
`` ''

ruby
response.installments_amount
response.id_query_installments
deferred_period = response.deferred_periods [0]
deferred_period.amount
deferred_period.period
`` ''

python
response.installments_amount
response.id_query_installments
deferred_period = response.deferred_periods [0]
deferred_period.amount
deferred_period.period
`` ''

http
200 OK
Content-Type: application / json

{
  "installments_amount": 3334,
  "id_query_installments": 11,
  "deferred_periods": [
    {
      "amount": 1000,
      "period": 1
    }
  ]
}
`` ''

javascript
response.installment_amount
response.id_query_installment
deferred_period = response.deferred_periods [0]
deferred_period.amount
deferred_period.period
`` ''

Name <br> <i> type </i> | Description
------   | -----------
installments_amount <br> <i> String </i> |Amount of each installment. Length: 17.
id_query_installments <br> <i> String </i> |Quota identifier. Length: 19.
deferred_periods <br> <i> Array </i> | Arrangement with deferred periods.
deferred_periods [] .amount <br> <i> String </i> |Amount. Length: 17.
deferred_periods [] .period <br> <i> String </i> |Period index. Length: 2.

### Complete Mall transaction confirmation

Once the transaction has been initiated and the amount of the fees for each subtransaction has been consulted, you can confirm and obtain the result of a complete transaction using the `Transaction.commit ()` method.

 <strong> Transaction.commit () Complete Mall </strong>

Operation that allows confirming a transaction. Returns the status of the
transaction.

java
MallTransactionCommitDetails details = MallTransactionCommitDetails.build (). Add (
  commerceCode, buyOrder, idQueryInstallments, deferredPeriodIndex, gracePeriod
);

final MallFullTransactionCommitResponse response = MallFullTransaction.Transaction.commit (token, details);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

$ details = [
  {
    "commerce_code": '597055555552',
    "buy_order": 'OrdenCompra1234',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  },
  {
    "commerce_code": '597055555553',
    "buy_order": 'OrdenCompra12345',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  }
]

$ res = MallTransaction :: commit (
  $ token,
  $ details
);
`` ''

csharp
using Transbank.Webpay.TransaccionCompletaMall;

var transactionDetails = new List <MallCommitDetails> ();
transactionDetails.Add (new MallCommitDetails (
    commerceCodeMallOne,
    buyOrderMallOne,
    idQueryInstallmentsOne,
    deferredPeriodIndexOne,
    gracePeriodOne
));
transactionDetails.Add (new MallCommitDetails (
    commerceCodeMallTwo,
    buyOrderMallTwo,
    idQueryInstallmentsTwo,
    deferredPeriodIndexTwo,
    gracePeriodTwo
));

var response = MallFullTransaction.Commit (
  token, transactionDetails
);
`` ''

ruby
details = [
  {
    commerce_code: '597055555552',
    buy_order: 'orderCompra1234',
    id_query_installments: 12,
    deferred_period_index: 1,
    grace_period: false
  },
  {
    commerce_code: '597055555553',
    buy_order: 'orderCompra12345',
    id_query_installments: 12,
    deferred_period_index: 1,
    grace_period: false
  }
]

response = Transbank :: Complete Transaction :: MallTransaction :: commit (
  token: token, details: details
)
`` ''

python
details = [
  {
    "commerce_code": '597055555552',
    "buy_order": 'OrdenCompra1234',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  },
  {
    "commerce_code": '597055555553',
    "buy_order": 'OrdenCompra12345',
    "id_query_installments": 12,
    "deferred_period_index": 1,
    "grace_period": false
  }
]

response = Transaction.commit (
  token = token, details = details
)
`` ''

http
PUT /rswebpaytransaction/api/webpay/v1.0/transactions/[token}

Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "details": [
    {
      "commerce_code": 597055555552,
      "buy_order": "OrdenCompra1234",
      "id_query_installments": 12,
      "deferred_period_index": 1,
      "grace_period": false
    }
  ]
}
`` ''

javascript
let commitDetails = [
  new CommitDetail (commerceCode, childBuyOrder),
  new CommitDetail (commerceCode2, childBuyOrder2)
];
const response = await TransactionComplete.MallTransaction.commit (
  token,
  commitDetails
);
`` ''

 <strong> Parameters Transaction.commit Complete Mall </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
details <br> <i> details </i> | Listing with mall transactions.
commerce_code <br> <i> String </i> | Trade code assigned by Transbank for the store belonging to the mall to which this transaction corresponds. Maximum length: 12
buy_order <br> <i> String </i> | Purchase order from the mall store. Maximum length: 26
id_query_installments <br> <i> Number </i> | (Optional) Quota identifier. Maximum length: 19. Only send if payment is in installments
deferred_period_index <br> <i> Number </i> | (Optional) Amount of deferred period. Maximum length: 2. Only send if payment is in installments
grace_period <br> <i> Boolean </i> | (Optional) Grace period indicator. Only send if the payment is in installments

 <strong> Mall Complete Transaction.commit Response </strong>

java
response.getBuyOrder ();
response.getCardNumber ();
response.getAccountingDate ();
response.getTransactionDate ();
Detail detail = response.getDetails () [0];
detail.getAuthorizationCode ();
detail.getPaymentCodeType ();
detail.getResponseCode ();
detail.getInstallmentsAmount ();
detail.getInstallmentsNumber ();
detail.getAmount ();
detail.getCommerceCode ();
detail.getBuyOrder ();
detail.getStatus ();
detail.getBalance ();
`` ''

`` `php
$ response.getBuyOrder ();
$ response.getCardNumber ();
$ response.getAccountingDate ();
$ response.getTransactionDate ();
$ detail = response.getDetails () [0];
$ detail.getAuthorizationCode ();
$ detail.getPaymentCodeType ();
$ detail.getResponseCode ();
$ detail.getInstallmentsAmount ();
$ detail.getInstallmentsNumber ();
$ detail.getAmount ();
$ detail.getCommerceCode ();
$ detail.getBuyOrder ();
$ detail.getStatus ();
$ detail.getBalance ();
`` ''

csharp
response.BuyOrder;
response.CardNumber;
response.AccountingDate;
response.TransactionDate;
detail = response.Details [0];
detail.AuthorizationCode;
detail.PaymentCodeType;
detail.ResponseCode;
detail.InstallmentsAmount;
detail.InstallmentsNumber;
detail.Amount;
detail.CommerceCode;
detail.BuyOrder;
detail.Status;
detail.Balance;
`` ''

ruby
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details [0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
`` ''

python
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details [0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
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
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555552",
      "buy_order": "505479072"
    }
  ]
}
`` ''

javascript
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details [0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
`` ''

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order from the mall. Maximum length: 26
card_detail <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
card_detail.card_number <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for businesses authorized by Transbank is the full number sent. Maximum length: 19.
accounting_date <br> <i> String </i> | Authorization date. Length: 4, MMYY format
transaction_date <br> <i> String </i> | Date and time of authorization. format: ISO8601
details <br> <i> Array </i> | List with results of each of the transactions sent.
details [] .amount <br> <i> Number </i> | Amount of the transaction. Maximum length: 17 <br> <i> Accepts decimals in case of dollar operation </i>
details [] .status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
details [] .authorization_code <br> <i> String </i> | Transaction authorization code Maximum length: 2
details [] .payment_type_code <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Interest-free installments. <br> VD = Sale Debit. <i> (Coming soon) </i> <br> VP = Prepaid Sale. <i> (Coming soon) </i>
details [] .responseCode <br> <i> Number </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes)
details [] .installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2
details [] .installments_amount <br> <i> Number </i> | Amount of each installment. Maximum length: 17
details [] .commerce_code <br> <i> String </i> | Store trade code. Length: 12
details [] .buy_order <br> <i> String </i> | Store purchase order. Maximum length: 26
prepaid_balance <br> <i> Number </i> | Prepaid card balance. Sent only if balance is reported. <br> <i> Maximum length 17 </i>

### Check the status of a Complete Mall Transaction

This operation allows obtaining the status of the Complete Mall transaction at any time. Under normal conditions, it is probably not required to execute, but in the event of an unexpected error, it allows knowing the status and taking the corresponding actions.

 <strong> Transaction.status () Complete Mall </strong>

Get transaction result from a token.

java
MallFullTransaction.Transaction.status (token)
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

$ response = MallTransaction :: status (token)
`` ''

csharp
using Transbank.Webpay.TransaccionCompletaMall;

MallFullTransaction.Status (token);
`` ''

ruby
Transbank :: TransaccionCompleta :: MallTransaction :: status (token)
`` ''

python
Transaction.status (token)
`` ''

http
GET /rswebpaytransaction/api/webpay/v1.0/transactions/{token}
Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json
`` ''

javascript
const response = await Complete Transaction.MallTransaction.status (token);
`` ''

 <strong> Parameters Transaction.status Complete Mall </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)

 <strong> Mall Complete Transaction.status Response </strong>

java
response.getBuyOrder ();
response.getCardNumber ();
response.getAccountingDate ();
response.getTransactionDate ();
Detail detail = response.getDetails () [0];
detail.getAuthorizationCode ();
detail.getPaymentCodeType ();
detail.getResponseCode ();
detail.getInstallmentsAmount ();
detail.getInstallmentsNumber ();
detail.getAmount ();
detail.getCommerceCode ();
detail.getBuyOrder ();
detail.getStatus ();
detail.getBalance ();
`` ''

`` `php
$ response.getBuyOrder ();
$ response.getCardNumber ();
$ response.getAccountingDate ();
$ response.getTransactionDate ();
$ detail = response.getDetails () [0];
$ detail.getAuthorizationCode ();
$ detail.getPaymentCodeType ();
$ detail.getResponseCode ();
$ detail.getInstallmentsAmount ();
$ detail.getInstallmentsNumber ();
$ detail.getAmount ();
$ detail.getCommerceCode ();
$ detail.getBuyOrder ();
$ detail.getStatus ();
$ detail.getBalance ();
`` ''

csharp
response.BuyOrder;
response.CardNumber;
response.AccountingDate;
response.TransactionDate;
detail = response.Details [0];
detail.AuthorizationCode;
detail.PaymentCodeType;
detail.ResponseCode;
detail.InstallmentsAmount;
detail.InstallmentsNumber;
detail.Amount;
detail.CommerceCode;
detail.BuyOrder;
detail.Status;
detail.Balance;
`` ''

ruby
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details [0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
`` ''

python
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details [0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
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
      "amount": 500,
      "status": "AUTHORIZED",
      "authorization_code": "1213",
      "payment_type_code": "VN",
      "response_code": 0,
      "installments_number": 0,
      "commerce_code": "597055555552",
      "buy_order": "505479072"
    }
  ]
}
`` ''
javascript
response.buy_order
response.card_number
response.accounting_date
response.transaction_date
detail = response.details [0]
detail.authorization_code
detail.payment_code_type
detail.response_code
detail.installments_amount
detail.installments_number
detail.amount
detail.commerce_code
detail.buy_order
detail.status
detail.balance
`` ''

Name <br> <i> type </i> | Description
------   | -----------
buy_order <br> <i> String </i> | Purchase order from the mall. Maximum length: 26
card_detail <br> <i> carddetails </i> | Object that represents the cardholder's credit card data.
card_detail.card_number <br> <i> String </i> |Last 4 numbers of the cardholder's credit card. Only for businesses authorized by Transbank is the full number sent. Maximum length: 19.
accouting_date <br> <i> String </i> | Authorization date. Length: 4, MMDD format
transaction_date <br> <i> String </i> | Date and time of authorization. Length: 6, format: ISO8601
details <br> <i> Array </i> | List with results of each of the transactions sent.
details [] .amount <br> <i> Number </i> | Amount of the transaction. Maximum length: 17 <br> <i> Accepts decimals in case of dollar operation </i>
details [] .status <br> <i> String </i> | Status of the transaction (INITIALIZED, AUTHORIZED, REVERSED, FAILED, NULLIFIED, PARTIALLY_NULLIFIED, CAPTURED). Maximum length: 64
details [] .authorization_code <br> <i> String </i> | Transaction authorization code Maximum length: 6
details [] .payment_type_code <br> <i> String </i> | [Payment type] (/ product / webpay # payment-types) of the transaction. <br> VN = Normal Sale. <br> VC = Sale in installments. <br> SI = 3 installments without interest. <br> S2 = 2 installments without interest. <br> NC = N Interest-free installments. <br> VD = Sale Debit. <i> (Coming soon) </i> <br> VP = Prepaid Sale. <i> (Coming soon) </i>
details [] .responseCode <br> <i> Number </i> | Authorization response code. Possible values: <br> 0 = Transaction approved <br> You can check the rejection response codes in the following [link] (/ product / webpay # authorization-response-codes)
details [] .installments_number <br> <i> Number </i> | Amount of fees. Maximum length: 2
details [] .installments_amount <br> <i> Number </i> | Amount of each installment. Maximum length: 17
details [] .commerce_code <br> <i> String </i> | Store trade code. Length: 12
details [] .buy_order <br> <i> String </i> | Store purchase order. Maximum length: 26
balance <br> <i> Number </i> | Remaining amount for a voided detail. Maximum length: 17
prepaid_balance <br> <i> Number </i> | Prepaid card balance. Sent only if balance is reported. <br> <i> Maximum length 17 </i>

### Cancellation Complete Mall Transaction

It allows generating a refund of all or part of the amount of a complete transaction.
Depending on the following business logic, the invocation of this operation will generate a reverse or an abort:

* If a value is specified in the "amount" field, an annulment will always be executed.
* If the maximum time to execute a reverse is exceeded, an annulment will be executed.
* If none of the above cases have occurred, a reversal will be executed.

 <strong> Transaction.refund () Complete Mall </strong>

It allows you to request from Webpay the cancellation of a transaction made previously and that is currently in force.

java
final MallFullTransactionRefundResponse response = MallFullTransaction.Transaction.refund (
  token, amount, commerceCode, buyOrder
);
`` ''

`` `php
use Transbank \ TransaccionCompleta \ Transaction;

MallTransaction :: refund (
  token,
  child_buy_order,
  child_commerce_code,
  amount
);
`` ''

csharp
using Transbank.Webpay.TransaccionCompletaMall;

MallRefundRequest (
  token,
  childBuyOrder,
  childCommerceCode,
  amount
);
`` ''

ruby
Transbank :: Complete Transaction :: MallTransaction :: refund (
  token: token,
  child_buy_order: child_buy_order,
  child_commerce_code: child_commerce_code,
  amount: amount
)
`` ''

python
from transbank.transaccion_completa_mall.transaction import Transaction

Transaction.refund (
  token = token, amount = amount, child_commerce_code = child_commerce_code, child_buy_order = child_buy_order
)
`` ''

http
POST /rswebpaytransaction/api/webpay/v1.0/transactions/[token-lex.europa.eu/refunds
Tbk-Api-Key-Id: 597055555551
Tbk-Api-Key-Secret: 579B532A7440BB0C9079DED94D31EA1615BACEB56610332264630D42D0A36B1C
Content-Type: application / json

{
  "buy_order": "415034240",
  "commerce_code": "597055555552",
  "amount": 1000
}
`` ''

javascript
const refundResponse = await Complete Transaction.MallTransaction.refund (
  token,
  buyOrder,
  commerceCode,
  amount
);
`` ''

 <strong> Parameters Transaction.refund Complete Mall </strong>

Name <br> <i> type </i> | Description
------   | -----------
token <br> <i> String </i> | Token of the transaction. Length: 64. (It is sent in the URL, not in the body)
buy_order <br> <i> String </i> | Purchase order of the transaction that is required to be captured. Maximum length: 26.
commerce_code <br> <i> Number </i> | Daughter store that carried out the transaction. Length: 12.
amount <br> <i> Integer format for weight transactions. Only in case of dollar it accepts two decimal places. </i> |  Amount to be canceled or reversed from the transaction. Maximum length: 17

 <strong> Mall Complete Transaction.refund Response </strong>

java
response.getType ();
response.getAuthorizationCode ();
response.getAuthorizationDate ();
response.getNullifiedAmount ();
response.getBalance ();
response.getResponseCode ();
`` ''

`` `php
response-> getType ();
response-> getAuthorizationCode ();
response-> getAuthorizationDate ();
response-> getNullifiedAmount ();
response-> getBalance ();
response-> getResponseCode ();
`` ''

csharp
response.Type;
response.AuthorizationCode;
response.AuthorizationDate;
response.NullifiedAmount;
response.Balance;
response.ResponseCode;
`` ''

ruby
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
`` ''

python
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
`` ''

http
200 OK
Content-Type: application / json
{
  "type": "NULLIFY",
  "authorization_code": "123456",
  "authorization_date": "2019-03-20T20: 18: 20Z",
  "nullified_amount": 1000.00,
  "balance": 0.00,
  "response_code": 0
}
`` ''

javascript
response.type
response.authorization_code
response.authorization_date
response.nullified_amount
response.balance
response.response_code
`` ''

Name <br> <i> type </i> | Description
------   | -----------
type <br> <i> String </i> | Refund type (REVERSE or NULLIFY). If it is REVERSE, no transaction data will be returned. Maximum length: 10
authorization_code <br> <i> String </i> | Cancellation authorization code. Maximum length: 6. It only comes in case of cancellation.
authorization_date <br> <i> String </i> | Date and time of authorization. It only comes in case of cancellation.
nullified_amount <br> <i> Decimal </i> | Amount canceled. Maximum length: 17. It only comes in case of cancellation.
balance <br> <i> Decimal </i> | Updated balance of the transaction (considers the sale minus the canceled amount). Maximum length: 17. It only comes in case of cancellation.
response_code <br> <i> Number </i> | Payment result code. If it is successful it is 0, otherwise the payment was not made. Maximum length: 2. It only comes in case of cancellation.

## Deferred Capture mall

Merchants that are configured to operate with deferred capture must execute the capture method to charge the cardholder.

**Valid for :**

* Webpay Plus Deferred Capture
* Full Transaction Deferred Capture

### Run deferred capture mall

You can [read more about the capture in the information of the
Webpay product] (/ product / webpay # authorization-and-capture)
for more details and restrictions.

To make this explicit capture, the `Transaction.capture ()` method must be used.

 <strong> Transaction.capture () </strong>

> The SDKs allow you to optionally indicate the commercial code of the
> transaction to capture, to support capture in Webpay Plus merchants
> Mall or Mall Complete Transaction. In stores without Mall mode it is not necessary to specify the code
> trade, since the one indicated in the configuration is used.


 <aside class="notice">
The `Transaction.capture ()` method must always be invoked indicating the code of the
merchant that carried out the transaction. In the case of Mall mode stores,
the code must be the code of the specific virtual store.
 </aside>


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

javascript
const response = Complete Transaction.MallDeferredTransaction.capture (
  token, commerceCode, buyOrder, authorizationCode, amount
);
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

javascript
response.authorization_code
response.authorization_date
response.captured_amount
response.response_code
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

Code  | Description
------  | -----------
304     | Null input field validation
245     | Commercial code does not exist
22      | The trade is not active
316     | The store indicated does not correspond to the certificate or is not a child of the Mall store in case of MALL transactions
308     | Operation not allowed
274     | Transaction not found
16      | The transaction is not deferred capture
292     | The transaction is not authorized
284     | Capture period exceeded
310     | Transaction previously reversed
309     | Transaction previously captured
311     | Amount to capture exceeds the authorized amount
315     | Authorizer error

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

Check [this section] (/ documentation / getting started # b-using-the-sdk) of the documentation to see the code needed to configure your own trading code and Api Key Secret.

