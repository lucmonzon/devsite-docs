---
sites_supported:
  - mla
  - mlb
  - mlm
---


# Como integrar Mercado Pago Point

Para poder cobrar de maneira integrada com nosso dispositivo Point é necessário baixar a aplicação do Mercado Pago disponível nos marketplaces de iOS e Android.

Existem dois cenários na hora de integrar Point:

1) Quando a aplicação pode ser utilizada no mesmo dispositivo (celular ou tablet) onde está instalada a aplicação de Mercado Pago. É possível logar com uma integração de _deep linking_ ou _intent-based_.

2) Quando a aplicação não pode ser utilizado no mesmo dispositivo (celular ou tablet) onde está instalada a aplicação do Mercado Pago. É possível logar com uma integração via _API_.


> WARNING
>
> Pré-requisitos
>
> * Contar com a aplicação de Mercado Pago (A partir da versão 2.34 para Android e 2.32 para iOS).
> * Contar com um dispositivo Point.
> * O usuário deve estar logado com sua conta do Mercado Pago na aplicação de Mercado Pago.
> * Disponível para Android versão 2.8.0 ou superior, iOS versão 1.7.0 ou superior e, somente quando executa, em iOS 9 ou superior.


## Integração via Deep Linking

Uma das formas de integrar-se com Mercado Pago Point é mediante um _deep linking_. Quando se chama o _link_, o mesmo será aceito como um _Point-handled address_ por parte da aplicação de Mercado Pago.

Na chamada a este _link_ se pode evitar diferentes parâmetros que seriam levantados pela aplicação de Mercado Pago e impactados no pagamento. Uma vez que se faça a chamada a este _link_, o usuário será redirecionado a tela da aplicação de Mercado Pago para informar o cartão do cliente e assim realizar a cobrança.

Uma vez que o pagamento é processado, o usuário será redirecionado a `success_url` ou `fail_url`, dependendo do estado do pagamento. Este deverá ser interceptado para retornar o usuário ao fluxo da aplicação.

### Diagrama do Fluxo

![Deep linking flow diagram Mercado Pago Point](/images/point_diagram.png)


### Criação do Deep Linking

A URL a ser interceptada é a seguinte: `https://www.mercadopago.com/mp-brasil/point/lojas`

Os parâmetros que se podem incluir são:

* `amount`: O valor que será cobrado do cliente (\*).
* `description`: Uma descrição da operação (Máx.: 20 caracteres) (\*).
* `external_reference`: O código de referência do seu sistema, o mesmo permitirá conciliar seu pedido de compra com o pagamento.
* `notification_url`: É a URL que receberá a notificação desse pagamento.
* `payer_email`: É o email do pagador.
* `success_url`: É a URL para onde o usuário será redirecionado logo após o pagamento ser aprovado.
* `fail_url`: É a URL para onde o usuário será redirecionado logo após o pagamento ser rejeitado.

> WARNING
>
> Importante
>
> * Os campos marcados com (\*) são campos obrigatórios.


No artigo do [GitHub](https://github.com/mercadopago/point-android_integration#deep-linking) é possível obter mais informação e o exemplo correspondente.


## Integração via Intent-Based

> WARNING
>
> Importante
>
> * Esta integração só está disponível para Android versão 2.8.0 ou superior.


Outra forma de integrar-se com a aplicação de Mercado Pago é mediante código nativo de Android, mediante o conceito de _Intent-Based_.

Deve utilizar o método “startActivityForResult” para iniciar diretamente o processo de pagamento. O resultado do pagamento irá retornar como “activityResult”.

É muito importante que o código trate a possibilidade de que o usuário não tenha instalada a aplicação do Mercado Pago em seu dispositivo. Neste caso recomendamos redirecionar o usuário a Play Store para baixar a mesma.

Como referência é possível utilizar o código de exemplo e documentação que tem o formato para poder enviar a informação do pagamento e tratar o objeto de retorno.


No artigo do [GitHub](https://github.com/mercadopago/point-android_integration#intent) é possível obter mais informação e o exemplo correspondente.


## Integração via API

> WARNING
>
> Importante
>
> * Esta integração somente está disponível para Android versão 2.8.0 ou superior.
> * Não está disponível para iOS.
> * Para poder utilizar esta integração é necessário que se comunique com [nossa central de ajuda](https://www.mercadopago.com.br/developers/pt/support/) para que seja habilitada a opção de integração na aplicação de Mercado Pago.

A outra forma de se integrar com a aplicação de Mercado Pago para cobrar com Point é mediante nossas APIs.

Para esta integração, primeiro é necessário configurar na aplicação de Mercado Pago o `device_name` . O mesmo serve para identificar o celular ou tablet e relacioná-lo com a conta do Mercado Pago. Desta maneira, saberá a que dispositivo enviar a ordem de pagamento.

O passo seguinte, consiste em gerar uma ordem de pagamento e envia-la via API ao dispositivo de onde se deseja cobra-la. O usuário verá que na tela desse dispositivo se abre a aplicação de Mercado Pago pronta para passar o cartão e avançar com o fluxo de pagamento utilizando a Point.

Uma vez que o pagamento é processado, o usuário verá o resultado do pagamento na aplicação do Mercado Pago. Finalmente, a ordem gerada se fechará e se criará o pagamento correspondente.

### Criação de uma ordem de pagamento

O POST nessa API gera uma ordem, que é a que recebe a aplicação de Mercado Pago para cobrar com a Point, com um identificador que se pode utilizar para saber o status da mesma.

```curl
curl -X POST \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
-d '{"amount":100,"description":"xícara","device_name":"dispositivo","cc_type":"credit_card"}' \
'https://api.mercadopago.com/point/services/integrations/v1'
```

Os parâmetros que se podem incluir são:

* `amount`: O valor que será cobrado do cliente (\*).
* `description`: Uma descrição da operação (Máx.: 20 caracteres) (\*).
* `device_name`: É o nome do dispositivo que receberá a ordem de pagamento (\*).
* `cc_type`: O tipo de cartão. Pode ser _credit_card_ (\*).
* `external_reference`: O código de referência do seu sistema, o mesmo permitirá conciliar o pedido de compra com o pagamento.
* `disable_back_button`: True ou False. Para que ao clicar no botão de voltar a ordem de pagamento se feche ou não.
* `notification_url`: É a URL que irá receber a notificação desse pagamento.
* `payer_email`: É o email do pagador.

> WARNING
>
> Importante
>
> * Os campos marcados com (\*) são campos obrigatórios.

A resposta terá o seguinte formato:

```json
{
  "status":"OPEN",
  "id":<order_id>
}
```

**Response status code: 200 OK**

### Obter a ordem de pagamento

O GET nesta API, junto ao `order_id`, possibilita recuperar o estado da mesma para saber se foi gerado o pagamento ou não.

```curl
curl -X GET \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/point/services/integrations/v1/:ID'
```

Se o status da ordem é `OPEN` quer dizer que a mesma ainda não foi paga. Se o status é `CLOSED` quer dizer que a ordem já foi paga e, portanto, obterá o `payment_id` com o resto da informação. A resposta terá o seguinte formato.

```json
{
  "status":"CLOSED",
  "id":<order_id>,
  "payment_id":<payment_id>,
  "payment_status":"<payment_status>",
  "external_reference": "<external_reference>",
  "payer_email": "<email_payer>"
}
```

**Response status code: 200 OK**

### Eliminar a ordem de pagamento

O DELETE nesta API possibilita eliminar uma ordem. Existem duas formas de eliminar a ordem.

É possível eliminar a ordem por `device_name`:

```curl
curl -X DELETE \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/point/services/integrations/v1/attempt/device/:DEVICE_NAME'
```

A resposta terá o seguinte formato.

```json
{
  "status":"OK"
}
```

**Response status code: 200 OK**

Ou é possível eliminar a ordem por `order_id`:

```curl
curl -X DELETE \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/point/services/integrations/v1/:ID'
```

**Response status code: 204 OK**


### Obter todos os dispositivos de uma conta

O GET nesta API possibilita obter todos os dispositivos configurados e sincronizados para sua conta do Mercado Pago

```curl
curl -X GET \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/point/services/integrations/v1/devices'
```

Se o status do dispositivo é `FREE` quer dizer que o dispositivo pode receber uma ordem nova. Se o status é `BUSY` quer dizer que o dispositivo já tem uma ordem associada. A resposta terá o seguinte formato.

```json
{
  "devices"[{
    "name": "<device_name>",
    "caller": <id_interno>,
    "id": <id_interno>,
    ”status”:{
      status”:”FREE”,
      "payment_attempt": <order_id>
    }
  }]
}
```

**Response status code: 200 OK**


### Eliminar um dispositivo de uma conta

O DELETE nesta API possibilita apagar algum dos dispositivos configurados e sincronizados para sua conta de Mercado Pago.


```curl
curl -X DELETE \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer ACCESS_TOKEN' \
'https://api.mercadopago.com/point/services/integrations/v1/devices/:DEVICE_NAME'
```

A resposta terá o seguinte formato.

```json
{
  "status":"OK"
}
```

**Response status code: 200 OK**


## Notificações de Pagamento

É necessário que envie sua `notification_url`, onde receberá aviso de todos os novos pagamentos e atualizações de status que se gerem.

No artigo de [notificações](https://www.mercadopago.com.br/developers/pt/guides/notifications/webhooks) é possível obter mais informações.


## Pagamentos de Point

Os pagamentos de Point podem ser buscados na API de Payments. Pode-se encontrar mais informações no artigo de [API's](https://www.mercadopago.com.br/developers/pt/reference/payments/_payments_id/get)

Existe uma API exclusiva de Point que conta com informações adicionais do pagamento: 


```curl
curl -X GET \
-H "Content-Type: application/json" \
-H 'Authorization: Bearer <access_token>' \
https://api.mercadopago.com/point/services/payment/<payment_id>
```

A resposta terá o seguinte formato:

```json
{
  "payment_id": 12345,
  "caller_id": 44444,
  "poi": "BBPOS-123123123",
  "poi_type": "BBPOS",
  "operator_id": 555555,
  "buyer_info": {
    "email": "email@email.com"
  }
}
```

> O campo "poi" é o identificador físico do dispositivo Point.
