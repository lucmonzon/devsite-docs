# Glossary


We know, some terms are technical and you may not be familiar with all of them. Use this glossary to not get lost!

| Name on the report column | What it means |
| --- | --- |
| DATE | Release, block or unlock date, as appropriate. |
| SOURCE_ID | Operation ID in Mercado Pago (for example, the payment of a sale). |
| EXTERNAL_REFERENCE | <br/> ID that helps identify the origin of the operation. For example, it can be the sale through the order ID or the shipment (if it is a cart purchase) or the own ID provided by the seller in case of an external integration. <br/><br/> Keep in mind that it is possible that this field is empty for some cases such as ticket payment or money transfer, among others. <br/> <br/> |
| RECORD_TYPE | <br/> `initial_available_balance` → Money available from the previous period.<br/><br/> `block` → Money blocked by a complaint or chargeback. <br/><br/> `unblock` → Money released because a complaint or chargeback was resolved. <br/><br/> `release` → Money from a collection that was released. <br/><br/> `fullblock` → Money blocked by restriction. <br/><br/> `subtotal` → Sum of the amounts of each record type. <br/><br/> `total` → Total Net Amount. <br/> <br/> |
| DESCRIPTION | <table style="border:none;background:none;font-size:16px;" ><tr style="border:none;background:none;"><td style="border:none;background:none;"> Possible values that the field can take: <br/><ul><li> For `block o unblock`: chargeback, dispute, shipping_return, credit_payment, reserve_for_payment, reserve_for_debt_payment, reserve_for_refund, reserve_for_bpp_shipping_return, reserve_for_cbk_cross_recovery, reserve_for_embargo_invested, restriction----[mlb]----, judgment_bacen, fraud ------------</li><li>For `release`: payment, withdrawal, refund, tax_payment_ibcf, tax_payment_ibcf_cancel, tax_payment_ibex, tax_payment_iibb, tax_payment_iibb_cancel, shipping, shipping_cancel, tax_withdholding, tax_withdholding_cancel, mediation,mediation_cancel, chargeback, fee_release_in_advance, asset_management_gain, asset_management_loss</li><li>For `fullblock`: restriction.</li><li>For `subtotal`: block, unblock o release.</li></ul></td></tr><tr style="border:none;"><td style="border:none;">Definitions to consider: <br/><br/> `chargeback`:  appears when a chargeback associated with the referred payment is initiated or resolved. <br/><br/> `dispute`: appears when a mediation or complaint on the referred payment is initiated or resolved. It can occur before or after the payment has been released as available money and even withdrawn from the account. <br/><br/>`shipping_return`: appears when a payment made by express return is blocked or unlocked. <br/><br/>`payment`:  payment that is released in any of the channels in which the client operates. <br/><br/>`withdrawal`: withdrawal made from the available money. <br/><br/>`refund`: refund associated with the referred payment. <br/><br/>`tax_payment_ibcf`: perception of gross income in Federal Capital, it is calculated once a month according to the operations transacted. To reconcile by operation, see the detail in the [Invoice Reports in MyML](https://vendedores.mercadolibre[FAKER][URL][DOMAIN]/blog/notas/todo-lo-que-tenes-que-saber-sobre-tu-facturacion).<br/><br/>`tax_payment_ibcf_cancel`: cancellation of the gross income tax in Federal Capital.<br/><br/>`tax_payment_ibex`: collection of gross income per subject exceeded by a simplified regime, it is calculated once a month according to the transactions traded. To reconcile by operation, see the detail in the [Invoice Reports in MyML](https://vendedores.mercadolibre[FAKER][URL][DOMAIN]/blog/notas/todo-lo-que-tenes-que-saber-sobre-tu-facturacion). <br/><br/> `tax_payment_iibb`: collection of gross income in the province of Buenos Aires, it is calculated once a month according to the transactions transacted. To reconcile by operation, see the detail in the [Invoice Reports in MyML](https://vendedores.mercadolibre[FAKER][URL][DOMAIN]/blog/notas/todo-lo-que-tenes-que-saber-sobre-tu-facturacion).<br/><br/> `tax_payment_iibb_cancel`:cancellation of the gross income tax. <br/><br/> `tax_withdholding`: the collection of withholdings that could not be transactionally executed to the associated payment. In Argentina they are only withholdings of Gross Revenue (perceptions are debited as another operation). In Uruguay they are VAT withholdings. In Colombia they are withholdings of VAT, ICA and Source as applicable. <br/><br/> `tax_withdholding_cancel`: the cancellation of the tax_withdholding retention. <br/><br/> `shipping`: shipping commission for cart purchases that is not included in each cart payment. <br/><br/> `shipping_cancel`: cancellation of the shipping commission for cart purchases that is not included in each cart payment. <br/><br/> `mediation`: resolution of a mediation in favor of the buyer that ends up subtracting from the money available from the seller. <br/><br/> `mediation_cancel`: cancellation of mediation resolved in favor of the buyer. <br/><br/> `chargeback`: chargeback either for or against an operation. <br/><br/>`fee-release_in_advance`: advance fee. <br/><br/> `asset_management_gain`: positive return generated by the variation in the value of shares subscribed in the mutual fund.<br/><br/> `asset_management_loss`: negative return generated by the variation in the value of shares subscribed in the mutual fund.<br/><br/> `restriction`: occurs when you apply a restriction for fraudulent behavior. <br/><br/> `credit_payment`: appears when the fee for a loan granted is charged. <br/><br/> `payout`: money withdrawal available at Mercado Pago.<br/><br/> `reserve_for_embargo_invested`: blocking of your invested money. This value appears when there is a reserve of money in investment funds.<br/><br/> `reserve_for_bpp_shipping_return`: reserve for returns.<br/><br/> `reserve_for_debt_payment`: withheld for debt payment.<br/><br/> `reserve_for_refund`: withheld for approved returns.<br/><br/> `reserve_for_cbk_cross_recovery`: withheld for linked account chargeback.<br/><br/> `reserve_for_payment`: expenses pending confirmation. ----[mlb]---- <br/><br/>`judgment_bacen`: Central Bank court order. The money may be frozen in the account because of ongoing litigation. It may be associated with blocked amounts.<br/><br/> `fraud`: security patch.<br/><br/>`trava_de_recibibles`: block receivable. ------------</td></tr></table>|
| NET_DEBIT_AMOUNT | Accredited to the amount available. |
| NET_CREDIT_AMOUNT | Debited to the amount available. |
| GROSS_AMOUNT | Gross operation amount. |
| MP_FEE_AMOUNT | Commission Payment of Mercado Pago and/or Mercado Libre. ----[mla,mpe,mco,mlm,mlu,mlc]---- Includes VAT. ------------ |
| FINANCING_FEE_AMOUNT | Cost for offering interest-free installments. |
| SHIPPING_FEE_AMOUNT | Shipping cost. |
| TAXES_AMOUNT | Taxes charged for gross income withholdings. |
| COUPON_AMOUNT | Discount coupon amount. Only the gross amount (`GROSS_AMOUNT`) is discounted if provided by the seller. |
| INSTALLMENTS | Number of installments in which the operation was carried out. |
| PAYMENT METHOD | Payment method. ----[mla]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_argentina) ------------ ----[mlb]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_brasil) ------------ ----[mpe]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_perú) ------------ ----[mco]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_colombia) ------------ ----[mlm]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_méxico) ------------ ----[mlu]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_uruguay) ------------ ----[mlc]---- [It can be](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/resources/localization/payment-methods/#bookmark_chile) ------------ |
| TAX_DETAIL | <br/> Description of the tax withheld by operation in `TAXES_AMOUNT`. ----[mla]---- It can take the following values according to the jurisdiction: <br/>cordoba<br/>mendoza<br/>la_pampa<br/>santa_fe<br/>tucuman<br/>entre_rios<br/>catamarca<br/>neuquen<br/>santiago_del_estero<br/>rio_negro<br/>jujuy ------------ <br/><br/> |
| TAX_AMOUNT_TELCO | It is the value of the tax on telecommunications companies that is deducted from the gross value. |
| TRANSACTION_APPROVAL_DATE | Date of approval of the operation. |
| POS_ID | Cash ID if the payment is made through a physical commerce. |
| POS_NAME | Cash name for the payment made through a physical commerce. |
| EXTERNAL_POS_ID | User-defined cashier ID for payment made through a physical commerce. |
| STORE_ID | Branch ID if payment is made through a physical commerce. |
| STORE_NAME | Branch name for payment made through a physical commerce. |
| EXTERNAL_STORE_ID | User-defined branch ID for payment made through a physical commerce. |
| ORDER_ID | Purchase Order. |
| SHIPPING_ID | Shipping Identification. |
| SHIPMENT_MODE | Shipping Mode. |
| PACK_ID | Package identification in the cart. |
| TAXES_DISAGGREGATED | Taxes disaggregated in JSON format. |
| EFFECTIVE_COUPON_AMOUNT | Cost for offering discount. |

<hr/>

### Next steps

> LEFT_BUTTON_RECOMMENDED_EN
>
> How to use this report
>
> Learn how the report is composed and how to analyze it to make your reconciliation. 
>
> [How to use this report](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/reports/released-money/how-to-use)

> RIGHT_BUTTON
>
> Generate your report
>
> Learn the ways to generate a report and follow the steps to set your preferences.
>
> [Generate your report](https://www.mercadopago[FAKER][URL][DOMAIN]/developers/en/guides/manage-account/reports/released-money/generate)
