## Transactions flow

In order to meet VAT MOSS compliance you can use the Quaderno transactions flow.

With the Quaderno transactions flow you can make live taxes calculations in your website and check if the customer data meets the VAT compliance before completing the order. 

If you are not using the [quaderno.js for Stripe](https://github.com/quaderno/quaderno.js), you probably want to do most of it via ajax, so in this case please note that as it executes your code on the client side it will expose your API token. **We recommend using mechanisms to hide it such as proxy pages**.

Here is a practical example: 

Suppose you are running an e-books store placed in Madrid, Spain. A customer has just finished adding products to the shopping cart and now he/she wants to checkout.

### Step 1: Calculate taxes
While your customer is filling his/her data (card number, name, etc.) in your website form you can make live taxes calculations by calling the taxes api calculate method (see the taxes section to get more info). 

GET `https://quadernoapp.com/api/v1/taxes/calculate.json?country=ES&postal_code=08080&vat_number=ESA58818501`

This call will return a json object with the tax you may apply to this customer. For example:

```json
{
    "name":"VAT",
    "rate":21.0,
    "notes":null
}
```

### Step 2: Create the transaction object
So, the customer, after finishing filling his/her data is going to press the "Pay button".
Before completing the order you must create a transaction object. A sample call would look like this (if the customer was a North consumer and the picked e-book cost 12.00 € without taxes):

POST `https://quadernoapp.com/api/v1/transactions.json` 

with the body:

```json
{
    "country":"ES",
    "postal_code":"08080",
    "ip":"85.155.156.203",
    "iin":"424242",
    "vat_number":"ESA58818501",
    "amount":1200
}
```

If all the required transaction fields are present there are two possible scenarios:

* **Scenario 1:** The customer has incoherent data so it doesn't meet the VAT compliance. In this case the transaction is not saved because and the response code will be a `422` Quaderno cannot verify the right tax for this customer. We do not recommend proceeding with the order as you may apply wrong taxes.
* **Scenario 2:** The customer has at least two coincident data for the country (i.e. the country of the request ip  and the country of the zip code are the same). In this case the customer's data meets the VAT compliance so Quaderno will save the transaction as a history record and will return a `201` as a response code and a json representation of the transaction like this one:

```json
{
    "id":8,
    "tax_name":"IGIC",
    "tax_rate":21.0,
    "amount":1200,
    "tax_amount":252,
    "total_amount":1452,
}
```

Now you can proceed to the step 3.


If you need more information about this step you can check the [create transaction section](https://github.com/quaderno/quaderno-api/blob/master/sections/transactions.md#create-transactions)


### Step 3: Process and complete the order
Now that everything is verified and the information is stored, you can complete the order and create the payment on your website (i.e. debit the customer’s payment method). For example, if you were using Stripe to create charges just add the ID of the transaction created in the step 2 and add it to the charge as a metadata:

```php
Stripe_Charge::create(array(
  "amount" => 1452,
  "currency" => "eur",
  "card" => "tok_103NCO2eZvKYlo2Cc4lerc1E",
  "customer" => "cus_4BnckL2duT7i4y", 
  "description" => "The neverending story, Miachel Ende (EPUB)",
  "metadata" => array(
    "quantity" => 1,
    "transaction_id" => 8, // obtained with Quaderno API
    "tax_name" => "VAT",
    "tax_rate" => 21,
    "po_number" => "AX562",
    "notes" => "Lorem ipsum...",
    "invoice_email" => "text@example.com"
  ) // all metadata are optional
));
```

