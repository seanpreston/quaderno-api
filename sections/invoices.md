# Expenses
An invoice is a

## Get invoices
`GET /invoices.json` will return all your invoices.
```json
```
## Get invoices
`GET /invoice/1.json` will return the specified invoice.
```json
```
## Deliver invoice
`GET /invoices/1/deliver.json` will send the invoice to the contact email.

## Create invoice
`POST /invoices.json` will create a new invoice from the parameters passed.
```json
```
This will return `201 Created`, with the location of the new invoice in the Location header along with the current JSON representation of the invoice if the creation was a success.  If the user does not have access to create new invoices, you'll see `403 Forbidden`.

## Update invoice
`PUT /invoices/1.json` will update the invoice from the parameters passed.
```json
```

This will return `200 OK` if the update was a success along with the current JSON representation of the invoice. If the user does not have access to update the invoice, you'll see `403 Forbidden`.

## Delete invoice
`DELETE /invoices/1.json` will delete the invoice specified and return `204 No Content` if that was successful. If the user does not have access to delete the invoice, you'll see `403 Forbidden`.


