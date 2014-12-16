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

If you need more information about this step you can check the [create transaction section](https://github.com/quaderno/quaderno-api/blob/master/sections/transactions.md#create-transactions)

### Step 2: Process and Complete the Order
You can complete the order and take the payment on your payment gateway but sending a couple of extra data with the request the will be resent to our endpoint. For example, if you were using Stripe, add the customer country to the customer metadata and the request IP to the charge metadata.

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
    "ip_address" => "80.198.45.252",
    "quantity" => 1,
    "po_number" => "AX562",
    "notes" => "Lorem ipsum...",
  ) // all metadata are optional
));
```

### Step 3: Creation of the Evidence Object
When the gateway notification arrives Quaderno, we will create an Evidence object using the customer data passed within the metadata of the **step 2** and will associate it with a Quaderno Invoice.

After the evidence has been created there are two possible scenarios:

* **Scenario 1:** The customer has incoherent data and Quaderno cannot check if it meets the VAT compliance. We will send you an email with the information related to this customer. We recommend getting in contact with this customer to fix the issue.
* **Scenario 2:** The customer has at least two coincident data for the country (i.e. the billing country and the country of the IP address). Everything is OK in this case.