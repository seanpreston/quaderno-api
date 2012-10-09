# Payments
A payment is a

## Create payment

`POST expenses/1/payments.json`
```json
```
This will return `201 Created`, with the location of the new payment in the Location header along with the current JSON representation of the expense if the creation was a success.  If the user does not have access to create new payment, you'll see `401 Unauthorized`.

## Delete payment
`DELETE expenses/1/payments/1.json`
```json
```
`DELETE /expenses/1.json` will delete the payment specified and return `204 No Content` if that was successful. If the user does not have access to delete the payment, you'll see `401 Unauthorized`.