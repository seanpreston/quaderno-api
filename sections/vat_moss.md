# EU VAT MOSS Compliance

In order to meet EU VAT MOSS compliance you can use the Quaderno Transactions Flow.

With this flow you can make live taxes calculations in your website and check if the customer data meets the VAT compliance before completing the order. 

**Note: If you deal with Stripe Subscriptions, the easiest way to comply with the EU VAT MOSS rules is using [quaderno.js](https://github.com/quaderno/quaderno.js).**

## The Flow

Let's start with a practical example: 

Suppose you are running an e-books store based in Barcelona, Spain. Your customer, a German guy based in Berlin, has just finished adding products to the shopping cart and now he wants to checkout.

### Step 1: Calculate Taxes
While your customer is filling in your checkout form (card number, name, etc.), you can make live taxes calculations by calling our taxes calculate method (see the [taxes section](https://github.com/quaderno/quaderno-api/blob/master/sections/taxes.md) to get more info). 

```sh
$ curl https://myshop.quadernoapp.com/api/v1/taxes/calculate.json?country=DE&postal_code=10245&vat_number=DE345789003 \
    -u HPx1vDBKCG85X1HppFo8:x
```

This call will return a JSON object with the tax you may apply to this customer. For example:

```json
{
    "name":"VAT",
    "rate":19.0,
    "notes":null
}
```

In case you want to do this calculations via ajax, note that as it executes your code on the client side it will expose your API token. We recommend using mechanisms to hide it such as proxy pages.

### Step 2: Create the Transaction Object
After finishing filling his/her data, your customer will click the "Pay button".

Before completing the order you must create a transaction object. A sample call would look like this:

```sh
$ curl https://quadernoapp.com/api/v1/transactions.json
    -u HPx1vDBKCG85X1HppFo8:x
    -d country=DE
    -d postal_code=10245
    -d vat_number=DE345789003
    -d ip=85.155.156.203
    -d iin=424242
    -d amount=1200
```

If all the required transaction fields are present there are two possible scenarios:

* **Scenario 1:** The customer has incoherent data so it doesn't meet the VAT compliance. In this case the transaction is not saved and the response code will be a `422` Quaderno cannot verify the right tax for this customer. We do not recommend proceeding with the order as you may apply wrong taxes.
* **Scenario 2:** The customer has at least two coincident data for the country (i.e. the billing country and the country of the IP address). In this case the customer's data meets the VAT compliance so Quaderno will save the transaction as a history record and will return a `201` as a response code and a JSON representation of the transaction like this one:

```json
{
    "id":8,
    "tax_name":"VAT",
    "tax_rate":19.0,
    "amount":1200,
    "tax_amount":228,
    "total_amount":1428,
}
```

If you need more information about this step you can check the [create transaction section](https://github.com/quaderno/quaderno-api/blob/master/sections/transactions.md#create-transactions)

### Step 3: Process and Complete the Order
Now that everything is verified and the information is stored, you can complete the order and take the payment on your payment gateway. For example, if you were using Stripe just add the ID of the transaction created in step 2 as a charge metadata:

```php
//create a customer
Stripe_Customer::create(array(
  "description" => "Customer Full Name",
  "email" => "text@example.com",
  "card" => "tok_103NCO2eZvKYlo2Cc4lerc1E", // obtained with Stripe.js
  "metadata" => array(
    "first_name" => "Test",  // not if company
    "last_name" => "",  // not if company
    "contact_person" => "",
    "street_line_1" => "123 Carenden Road",
    "street_line_2" => "",
    "city" => "London",
    "postal_code" => "EC5M 8AJ",
    "region" => "London",
    "country" => "GB", // code ISO 3166-1 alpha-2
    "tax_id" => "13456789",
    "language" => "EN"
  ) // all metadata are optional
));

//charge the customer
Stripe_Charge::create(array(
  "amount" => 1428,
  "currency" => "eur",
  "customer" => "cus_4BnckL2duT7i4y", 
  "description" => "The Neverending Story, Michael Ende (EPUB)",
  "metadata" => array(
    "quantity" => 1,
    "transaction_id" => 8, // obtained with Quaderno API
    "po_number" => "AX562",
    "notes" => "Lorem ipsum...",
  ) // all metadata are optional
));
```
