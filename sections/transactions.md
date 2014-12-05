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
Mandatory fields:

* country: the customer's country in ISO 3166-1 format.
* iin: the first six digits of the customer's credit card.
* ip: the customer's request ip.
* amount: indicates (in cents) the base amount of the transaction. Must exist if `total_amount` is left blank.
* total_amount: indicates (in cents) the total amount of the transaction (base amount + taxes). Must exist if `amount` is left blank.


This will return `201 Created`, with the current JSON representation of the transaction if the creation was a success.

## Delete contact
`DELETE /transactions/1.json` will delete the transaction specified and return `204 No Content` if that was successful.


## Transactions flow

With the Quaderno transactions flow you can make live taxes calculations and check if the customer data meets the VAT compliance before completing the order. 
If you are not using the [quaderno.js for Stripe](https://github.com/quaderno/quaderno.js), you probably want to do most of it via ajax, so in this case please note that as it executes your code on the client side it will expose your API token. We recommend using mechanisms to hide it such proxy pages.

### Step 1: Calculate taxes
While your customer is filling his data in your website form you can make live taxes calculations by calling the taxes api calculate method as seen [here](https://github.com/quaderno/quaderno-api/blob/master/sections/taxes.md#calculate-taxes). 

### Step 2: Create the transaction object
If the customer wants to proceed to checkout, before completing the order you must create a transaction object as seen in the [create transaction section](https://github.com/quaderno/quaderno-api/blob/master/sections/transactions.md#create-transactions). If all the required transaction fields are present there are two possible scenarios:

* **Scenario 1:** The customer has incoherent data so it doesn't meet the VAT compliance. In this case the transaction is not saved because Quaderno cannot verify the right tax for this customer. We do not recommend proceeding with the order as you may apply wrong taxes.
* **Scenario 2:** The customer has at least two coincident data for the country (i.e. the country of the request ip  and the country of the zip code are the same). In this case the customer's data meets the VAT compliance so Quaderno will save the transaction as a history record. Now you can proceed to the step 3.

### Step 3: Process and complete the order
Now that everything is verified and the information is stored, you can complete the order and create the payment in your website (i.e. debit the customer’s payment method).
