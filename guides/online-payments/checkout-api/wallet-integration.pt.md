# Receba pagamentos com a carteira do Mercado Pago

Adicione na sua integração o Checkout Pro modo carteira, **para que você receba pagamentos somente de usuários cadastrados no Mercado Pago, com seus cartões e saldo disponível**.

Isto significa que seus clientes têm mais formas de te pagar e podem ter acesso aos melhores benefícios, sem mudar a dinâmica da sua API de Checkout.

![Checkout-Wallet](/images/web-payment-checkout/cho-wallet-landing-pt.png)

## Benefícios da carteira do Mercado Pago

* Seus clientes podem te pagar com mais meios de pagamento e mais rapidamente:
  * **Com cartões já salvos** nas suas contas Mercado Pago, poupando o esforço de digitar as informações de um cartão do zero.
  Eles também têm seus endereços salvos, o que simplifica todo o processo de preenchimento.
  * **Com saldo disponível no Mercado Pago**, o dinheiro está pronto para ser usado na hora, em 1 clique.
----[mla]----
* Seus clientes podem pagar em até 12 vezes no boleto, com parcelamento financiado pelo <a href="https://www.mercadolibre.com.ar/mercado-credito/meses-sin-tarjeta/" target="_blank"> Mercado Crédito</a>.
------------
----[mlb]----
* Seus clientes podem pagar em até 12 vezes no boleto, com parcelamento financiado pelo <a href="https://www.mercadolibre.com.br/mercado-credito/meses-sin-tarjeta/" target="_blank"> Mercado Crédito</a>.
------------
* A conversão do seu Checkout melhora ao oferecer meios de pagamento mais dinâmicos e fáceis de usar.
* Melhora a aprovação dos seus pagamentos com menos risco de fraude.

## Como adicionar a carteira no seu site

Você deve integrar o [Checkout Pro](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-pro/introduction) configurado como [modo carteira](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-pro/configurations#bookmark_aceitar_pagamentos_somente_de_usuários_cadastrados) para adicionar a carteira do Mercado Pago no seu site.

Para integrá-lo, você deve [gerar a preferência de pagamento](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-pro/integration#bookmark_etapas_para_se_integrar) com as informações do produto ou serviço que quer oferecer e adicionar a opção de pagamento no seu site. 

### Passos para integrar a carteira

> SERVER_SIDE
>
> h4
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;1. Gere sua preferência

Para começar, você precisa gerar sua preferência de pagamento pelo seu backend com a [SDK do Mercado Pago](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-api/previous-requirements#bookmark_sempre_utilize_nossas_bibliotecas) que você usou na sua API de Checkout. 

[[[
```php
===
O modo carteira funciona adicionando o atributo _purpose_ na preferência.
===
<?php
// Cria um objeto de preferência
$preference = new MercadoPago\Preference();

// Cria um item na preferência
$item = new MercadoPago\Item();
$item->title = 'Meu produto';
$item->quantity = 1;
$item->unit_price = 75;
$preference->items = array($item);
$preference->purpose = 'wallet_purchase';
$preference->save();
?>
```
```node
===
O modo carteira funciona adicionando o atributo _purpose_ na preferência.
===
// Cria um objeto de preferência
let preference = {
  items: [
    {
      title: 'Meu produto',
      unit_price: 100,
      quantity: 1,
    }
  ],
  purpose: 'wallet_purchase'
};

mercadopago.preferences.create(preference)
.then(function(response){
// Este valor substituirá a string "<%= global.id %>" no seu HTML
  global.id = response.body.id;
}).catch(function(error){
  console.log(error);
});
```
```java
===
O modo carteira funciona adicionando o atributo _purpose_ na preferência.
===
// Cria um objeto de preferência
Preference preference = new Preference();

// Cria um item na preferência
Item item = new Item();
item.setTitle("Meu produto")
    .setQuantity(1)
    .setUnitPrice((float) 75);
preference.appendItem(item);
preference.setPurpose("wallet_purchase");
preference.save();
```
```ruby
===
O modo carteira funciona adicionando o atributo _purpose_ na preferência.
===
# Cria um objeto de preferência
preference_data = {
  "items": [
    {
      "title": "Meu produto",
      "unit_price": 100,
      "quantity": 1
    }
  ],
  "purpose": "wallet_purchase"
}
preference = $mp.create_preference(preference_data)

# Este valor substituirá a string "<%= @preference_id %>" no seu HTML
@preference_id = preference["response"]["id"]
```
```csharp
===
O modo carteira funciona adicionando o atributo _purpose_ na preferência.
===
// Cria um objeto de preferência
Preference preference = new Preference();

// Cria um item na preferência
preference.Items.Add(
  new Item()
  {
    Title = "Meu produto",
    Quantity = 1,
    CurrencyId = CurrencyId.[FAKER][CURRENCY][ACRONYM],
    UnitPrice = (decimal)75
  }
);
preference.Purpose = "wallet_purchase"
preference.Save();
```
```curl
===
O modo carteira funciona adicionando o atributo _purpose_ na preferência.
===
curl -X POST \
  'https://api.mercadopago.com/checkout/preferences' \
  -H 'Content-Type: application/json' \
  -H 'cache-control: no-cache' \
  -H 'Authorization: Bearer **PROD_ACCESS_TOKEN**' \
  -d '{
    "items": [
        {
            "title": "Meu produto",
            "quantity": 1,
            "unit_price": 75
        }
    ],
    "purpose": "wallet_purchase"
}'
```
]]]

----[mlc, mco]----

> WARNING
>
> Importante
>
> O valor de `unit_price` deve ser um número inteiro.

------------
<span></span>

> CLIENT_SIDE
>
> h4
>
> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;2. Adicione o checkout ao seu site

Depois, do seu frontend, adicione o seguinte código para exibir o botão de pagamento do Checkout Pro modo carteira onde você quiser que ele apareça.

[[[
```php
<script
  src="https://www.mercadopago.com.ar/integrations/v1/web-payment-checkout.js"
  data-preference-id="<?php echo $preference->id; ?>"
  data-button-label="Paga com Mercado Pago"
  data-button-type="wallet">
</script>
```
```node
<script
  src="https://www.mercadopago.com.ar/integrations/v1/web-payment-checkout.js"
  data-preference-id="<%= global.id %>"
  data-button-label="Paga com Mercado Pago"
  data-button-type="wallet">
</script>
```
```java
<script
  src="https://www.mercadopago.com.ar/integrations/v1/web-payment-checkout.js"
  data-preference-id="${preference.id}"
  data-button-label="Paga com Mercado Pago"
  data-button-type="wallet">
</script>
```
```ruby
<script
  src="https://www.mercadopago.com.ar/integrations/v1/web-payment-checkout.js"
  data-preference-id="<%= @preference_id %>"
  data-button-label="Paga com Mercado Pago"
  data-button-type="wallet">
</script>
```
```csharp
<script
  src="https://www.mercadopago.com.ar/integrations/v1/web-payment-checkout.js"
  data-preference-id="@Html.DisplayFor(model => model.id)"
  data-button-label="Paga com Mercado Pago"
  data-button-type="wallet">
</script>
```
]]]


Para mais informações sobre cada atributo, confira a [Referência de API](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/reference/preferences/_checkout_preferences/post). 

Pronto! você já tem a carteira do Mercado Pago integrada no seu site.

> WARNING
>
> Importante
>
> Para testar, não esqueça de acessar de outro navegador ou encerrar a sessão da sua conta Mercado Pago já que você não pode fazer um pagamento para si mesmo.<br/> 

---

### Próximos passos

> LEFT_BUTTON_RECOMMENDED_PT
>
> Integração avançada do Checkout Pro
>
> Otimize a integração da sua carteira para melhorar a gestão das suas vendas.
>
> [Integração avançada](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-pro/advanced-integration)


> RIGHT_BUTTON_RECOMMENDED_PT
>
> Teste a carteira
>
> Confira se está tudo correto na sua integração com os usuários de teste.
>
> [Pruebas](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/pt/guides/online-payments/checkout-pro/test-integration/)
