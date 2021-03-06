# Payments
When an invoice is paid, you can record the payment.

## Create payment

`POST invoices/1/payments.json` or `POST expenses/1/payments.json` will add a payment to the speciefied invoice or expense from the parameters passed (both are mandatory).
```json
{
  "amount":"56.60", 
  "payment_method":"credit_card"
}
```
Mandatory fields:

* amount: amount to be paid
* payment_method: method of payment (credit_card, cash, wire_transfer, direct_debit, check, promissory_note, iou, paypal or other)

This will return `201 Created`, with the location of the new payment in the Location header along with the current JSON representation of the payment if the creation was a success.  If the user does not have access to create a new payment, you'll see `401 Unauthorized`.

## Delete payment
`DELETE expenses/1/payments/1.json` will delete the payment specified and return `204 No Content` if that was successful. If the user does not have access to delete the payment, you'll see `401 Unauthorized`.
