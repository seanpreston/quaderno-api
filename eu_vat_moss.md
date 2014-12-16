# EU VAT MOSS Compliance for Stripe

In order to meet EU VAT MOSS compliance you can use the Quaderno Transactions Flow.

With this flow you can make live taxes calculations in your website and check if the customer data meets the VAT compliance before completing the order. 

**Note: If you deal with Stripe Subscriptions, the easiest way to comply with the EU VAT MOSS rules is using [quaderno.js](https://github.com/quaderno/quaderno.js).**

## The Flow

Let's start with a practical example. Suppose your business is based in Barcelona and your customer is a German guy based in Berlin.

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

You can use the tax rate to show the final amount your customer is going to pay.

In case you want to do this calculations via ajax, note that as it executes your code on the client side it will expose your API token. We recommend using mechanisms to hide it such as proxy pages.

### Step 2: Process and Complete the Order
Now that everything is verified and the information is stored, you can complete the order and take the payment on Stripe. First, create a Stripe customer

```php
$customer = Stripe_Customer::create(array(
  "description" => "Reynholm Industries",
  "email" => "text@example.com",
  "card" => "tok_103NCO2eZvKYlo2Cc4lerc1E", // obtained with Stripe.js
  "metadata" => array(
    "first_name" => "",  // not if company
    "last_name" => "",  // not if company
    "contact_person" => "", 
    "street_line_1" => "Boxhaneger Platz 1",
    "city" => "Berlin",
    "postal_code" => "10245",
    "region" => "Berlin",
    "country" => "DE", // code ISO 3166-1 alpha-2
    "tax_id" => "DE345789003"
  ) // all metadata are optional
));
```

You can create then a charge for a single purchase.

```php
Stripe_Charge::create(array(
  "amount" => 1428, // final amount, including taxes
  "currency" => "eur",
  "customer" => $customer->id, 
  "description" => "The Neverending Story, Michael Ende (EPUB)",
  "metadata" => array(
    "tax_name" => "VAT",
    "tax_rate" => 19,
    "ip_address" => $_SERVER['REMOTE_ADDR']
  ) // all metadata are optional
));
```

Or you can create a subscription on a recurring basis.

In this case, you have to create first a Stripe Invoice Item to add the taxes to the first invoice. Quaderno will take care of the recurring invoices.

```php
Stripe_InvoiceItem::create(array(
    "customer" => $customer->id,
    "amount" => 119, // the tax amount in cents for a 10€ plan
    "currency" => "eur",
    "description" => "Taxes",
    "metadata" => array(
        "type" => "tax",
        "name" => "VAT",
        "rate" => 19,
    )
));

$customer->subscriptions->create(array(
    "plan" => "awesome",
    "metadata" => array(
       "ip_address" => $_SERVER['REMOTE_ADDR']
    ) // all metadata are optional
));
```

Don't forget to set the customer IP address in both charges and subscriptions, if you want Quaderno to check the customer location according to the VAT MOSS requirements.

In both cases, Quaderno will always create an invoice and send you a notification if it's not able to check the customer location using the billing country, the IP address, and the credit card country. 
