# Payments
A payment is a

## Create payment

`POST invoices/1/payments.json` or `POST expenses/1/payments.json` will add a payment to the speciefied invoice or expense from the parameters passed
```json
{
  "amount_cents":"56600", 
  "payment_method":"credit_card"
}
```

This will return `200 OK`, with the location of the new payment in the Location header along with the current JSON representation of the payment if the creation was a success.  If the user does not have access to create new payment, you'll see `401 Unauthorized`.

## Delete payment
`DELETE expenses/1/payments/1.json` will delete the payment specified and return `204 No Content` if that was successful. If the user does not have access to delete the payment, you'll see `401 Unauthorized`.