# Transactions
A transaction is a record that keeps relevant information (country, ip, iin, tax name, tax rate, etc) of a commercial transaction.

## Get transaction
`GET /transactions/1.json` will return the specified transaction.

```json
{
    "id":8,
    "tax_name":"IGIC",
    "tax_rate":7.0,
    "amount":1200,
    "tax_amount":84,
    "total_amount":1284,
}
```

## Create transaction
`POST /transactions.json` will create a new contact from the parameters passed.

```json
{
    "country":"ES",
    "postal_code":"08080",
    "ip":"85.155.156.203",
    "iin":"424242",
    "vat_number":"ESX9999999X",
    "amount":1200
}
```

* country: the customer's country in ISO 3166-1 format. **Mandatory**
* iin: first six digits of the credit card. It's the public [Issuer Identification Number](http://en.wikipedia.org/wiki/Bank_card_number#Issuer_identification_number_.28IIN.29). **Optional**
* ip: the customer's request ip. **Optional**
* amount: indicates (in cents) the base amount of the transaction. **Mandatory if** `total_amount` **is left blank**.
* total_amount: indicates (in cents) the total amount of the transaction (base amount + taxes). **Mandatory if** `amount` **is left blank**.

This will return `201 Created`, with the current JSON representation of the transaction if the creation was a success or a `422 Unprocessable entity` if some of the required fields are missing or the data don't meet the VAT compliance (check the country attribute message for this case).

## Delete transaction
`DELETE /transactions/1.json` will delete the transaction specified and return `204 No Content` if that was successful.

