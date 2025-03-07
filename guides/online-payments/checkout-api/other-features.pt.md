# Autorização e Captura

Adicione funcionalidades específicas a sua integração segundo as necessidades do seu negócio.

Ofereça a possibilidade de realizar uma autorização antes de gerar uma captura de um pagamento. Isso te permite fazer uma reserva de fundos no cartão do seu comprador sem efetuar o pagamento.

Por exemplo, para realizar uma autorização para alugar um carro ou autorizar um valor estimado de uma compra antes da sua confirmação.

## Realize uma reserva de valores

Para fazer uma autorização de reserva de valores é preciso apenas adicionar o atributo `capture=false` da seguinte maneira:

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = new MercadoPago\Payment();

  $payment->transaction_amount = 100;
  $payment->token = "ff8080814c11e237014c1ff593b57b4d";
  $payment->description = "Título do produto";
  $payment->installments = 1;
  $payment->payment_method_id = "visa";
  $payment->payer = array(
    "email" => "test_user_19653727@testuser.com"
  );

  $payment->capture=false;

  $payment->save();

?>
```
```java

import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = new Payment();

payment.setTransactionAmount(100f)
      .setToken('ff8080814c11e237014c1ff593b57b4d')
      .setDescription('Título do produto')
      .setInstallments(1)
      .setPaymentMethodId("visa")
      .setPayer(new Payer("test_user_19653727@testuser.com"))
      .setCapture(false);

payment.save();

```
```node

var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

var payment_data = {
  transaction_amount: 100,
  token: 'ff8080814c11e237014c1ff593b57b4d'
  description: 'Título do produto',
  installments: 1,
  payment_method_id: 'visa',
  payer: {
    email: 'test_user_3931694@testuser.com'
  },
  capture: false
};

mercadopago.payment.create(payment_data).then(function (data) {

}).catch(function (error) {

});

```
```ruby

require 'mercadopago'
MercadoPago::SDK.configure(ACCESS_TOKEN: ENV_ACCESS_TOKEN)

payment = MercadoPago::Payment.new()
payment.transaction_amount = 100
payment.token = 'ff8080814c11e237014c1ff593b57b4d'
payment.description = 'Título do produto'
payment.installments = 1
payment.payment_method_id = "visa"
payment.payer = {
  email: "test_user_19653727@testuser.com"
}
payment.capture = false
payment.save()
```
```curl

curl -X POST \
    -H 'accept: application/json' \
    -H 'content-type: application/json' \
    -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
    'https://api.mercadopago.com/v1/payments' \
    -d '{
          "transaction_amount": 100,
          "token": "ff8080814c11e237014c1ff593b57b4d",
          "description": "Título do produto",
          "installments": 1,
          "payment_method_id": "visa",
          "payer": {
            "email": "test_user_19653727@testuser.com"
          },
          "capture": "false"
    }'
]]]

A resposta indica que o pagamento se encontra autorizado e pendente de captura.

```json
{
  "id": PAYMENT_ID,
  ...
  "status": "authorized",
  "status_detail": "pending_capture",
  ...
  "captured": false,
  ...
}
```

Também pode resultar rejeitado ou ficar pendente. Tenha em conta que os valores autorizados não poderão ser utilizados pelo seu cliente até que não sejam capturados. Recomendamos realizar a captura o quanto antes.


> WARNING
>
> Importante
>
> * A reserva terá validade de 7 dias. Se não capturá-la nesse período, será cancelada.
> * Deve guardar o ID do pagamento para poder finalizar o processo.

## Capture um pagamento autorizado

Para finalizar o pagamento, é necessário capturar os valores que reservou ao seu cliente. Pode-se capturar o valor total ou parcial.

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Capturar o valor total de uma reserva

Para fazer a captura do valor total deve apenas enviar o atributo `capture` como `true`.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = MercadoPago\Payment::find_by_id($payment_id);
  $payment->capture = true;
  $payment->update();
?>
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");

Payment payment = Payment.load(paymentId);
payment.capture = true;
payment.update();
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

let paymentId = 123;

mercadopago.payment.capture(paymentId, mercadopago, (error, response) => {
    if (error){
        console.log(error);
    }else{
        console.log(response)
    }
});
```
```ruby
require 'mercadopago'
MercadoPago::SDK.configure(ACCESS_TOKEN: ENV_ACCESS_TOKEN)

payment = MercadoPago::Payment.load(paymentId)
payment.capture=true
payment.update()
```
```curl
curl -X PUT \
  'https://api.mercadopago.com/v1/payments/PAYMENT_ID' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{"capture": true}'
```
]]]

A resposta devolverá que o pagamento se encontra aprovado e creditado.

```json
{
  ...
  "status": "approved",
  "status_detail": "accredited",
  ...
  "captured": true,
  ...
}
```

> NOTE
>
> Nota
>
> Se não adicionar o valor, será capturado o total reservado.

## Capturar um pagamento por um valor inferior ao reservado

----[mla]----
> WARNING
>
> Importante
>
> Disponível somente para Visa, Cabal, Master e American Express.
------------

Para capturar um valor inferior ao reservado, é preciso adicionar o atributo `transaction_amount`com o novo valor.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = MercadoPago\Payment::find_by_id($payment_id);
  $payment->transaction_amount = 75;
  $payment->capture = true;
  $payment->update();
?>
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");


Payment payment = Payment.load(paymentId);
payment.transaction_amount = 75;
payment.capture = true;
payment.update();
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

let captureInfo = {id: 123, transaction_amount: 5}

mercadopago.payment.capturePartial(captureInfo, mercadopago, (error, response) => {
    if (error){
        console.log(error);
    }else{
        console.log(response)
    }
});
```
```ruby
require 'mercadopago'
MercadoPago::SDK.configure(ACCESS_TOKEN: ENV_ACCESS_TOKEN)

payment = MercadoPago::Payment.load(paymentId)
payment.transaction_amount = 75
payment.capture=true
payment.update()
```
```curl

curl -X PUT \
  'https://api.mercadopago.com/v1/payments/PAYMENT_ID' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{
          "transaction_amount": 75,
          "capture": true
}'
```
]]]

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Resposta

```json
{
  ...
  "status": "approved",
  "status_detail": "accredited",
  ...
  "transaction_amount": 75,
  ...
  "captured": true,
  ...
}
```


> NOTE
>
> Nota
>
> Não é possível capturar um valor superior ao reservado, para isso é preciso cancelar a reserva e gerar uma nova.

## Cancelar uma reserva

Pode-se cancelar uma reserva e liberar o limite do cartão ao atualizar o atributo `status` do pagamento ao estado `cancelled`.

[[[
```php
<?php

  MercadoPago\SDK::setAccessToken("ENV_ACCESS_TOKEN");

  $payment = MercadoPago\Payment::find_by_id($payment_id);
  $payment->status = "cancelled";
  $payment->update();
?>
```
```java
import com.mercadopago.*;
MercadoPago.SDK.configure("ENV_ACCESS_TOKEN");


Payment payment = Payment.load(paymentId);
payment.status = "canceled";
payment.update();
```
```node
var mercadopago = require('mercadopago');
mercadopago.configurations.setAccessToken(config.access_token);

let paymentToBeCanceled = 123;

mercadopago.payment.cancel(paymentToBeCanceled, mercadopago, (error, response) => {
    if (error){
        console.log(error);
    }else{
        console.log(response)
    }
});
```
```ruby
require 'mercadopago'
MercadoPago::SDK.configure(ACCESS_TOKEN: ENV_ACCESS_TOKEN)

payment = MercadoPago::Payment.load(paymentId)
payment.status = "canceled"
payment.update()
```
```curl
curl -X PUT \
  'https://api.mercadopago.com/v1/payments/PAYMENT_ID' \
  -H 'Content-Type: application/json' \
  -H 'Authorization: Bearer ENV_ACCESS_TOKEN' \
  -d '{"status": "cancelled"}'
```
]]]

### &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Resposta

```json
{
  ...
  "status": "cancelled",
  "status_detail": "by_collector",
  ...
  "captured": false,
  ...
}
```

---
### Próximos passos

> LEFT_BUTTON_REQUIRED_PT
>
> Requisitos para ir a produção
>
> Conheça os requisitos necessários para começar a receber pagamentos.
>
> [Requisitos para ir a produção](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/goto-production/)

> RIGHT_BUTTON_RECOMMENDED_PT
>
> Referências de API
>
> Encontre toda a informação necessária para interagir com nossas APIs.
>
> [Referências de API](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/es/reference)
