# Invoices
An invoice is a

## Get invoices
`GET /invoices.json` will return all your invoices.
```json
[
  {
    "id":"507693322f412e0e2e00000f",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "contact":{
      "id":"5073f9c22f412e02d0000032",
      "full_name":"Garfield"
    },
    "po_number":null,
    "due_date":null,
    "currency":"USD",
    "items":[
      {
        "description":"lasagna",
        "quantity":"25.0",
        "unit_price":"3.75",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal":"$93.75",
        "discount":"$0.00",
        "gross_amount":"$93.75"
      }
    ]
    "subtotal":"$93.75",
    "discount":"$0.00",
    "taxes":[],
    "total":"$93.75",
    "payments":[
      {
        "id":"50aca7d92f412eda5200002c",
        "date":"2012-11-21",
        "payment_method":"credit_card",
        "amount":"\u20ac93.75",
        "url":"http://app.quaderno.io/my-account/api/v1/invoices/507693322f412e0e2e00000f/payments/50aca7d92f412eda5200002c.json"
      },
    ],
    "tags":"lasagna cat",
    "payment_details":"Ask Jon"
    "url":"http://app.quaderno.io/my-account/api/v1/invoices/507693322f412e0e2e00000f"
  },

  {
    "id":"507693322f412e0e2e0000da",
    "number":"0000047",
    "issue_date":"2012-10-11",
    "contact":{
      "id":"5073f9c22f412e02d00004cf",
      "full_name":"Teenage Mutant Ninja Turtles"
    },
    "po_number":null,
    "due_date":null,
    "currency":"USD",
    "items":[
      {
        "description":"pizza",
        "quantity":"15.0",
        "unit_price":"60.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal":"$60.00",
        "discount":"$0.00",
        "gross_amount":"$60.00"
      }
    ]
    "subtotal":"$60.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$60.00",
    "payments":[],
    "tags":"lasagna cat",
    "payment_details":""
    "url":"http://app.quaderno.io/my-account/api/v1/invoices/507693322f412e0e2e0000da"
  },
]
```

## Get invoice
`GET /invoices/1.json` will return the specified invoice.
```json
{
  "id":"507693322f412e0e2e0000da",
  "number":"0000047",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"5073f9c22f412e02d00004cf",
    "full_name":"Teenage Mutant Ninja Turtles"
  },
  "po_number":null,
  "due_date":null,
  "currency":"USD",
  "items":[
    {
      "description":"pizza",
      "quantity":"15.0",
      "unit_price":"60.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal":"$60.00",
      "discount":"$0.00",
      "gross_amount":"$60.00"
    }
  ]
  "subtotal":"$60.00",
  "discount":"$0.00",
  "taxes":[],
  "total":"$60.00",
  "payments":[],
  "tags":"lasagna cat",
  "payment_details":""
  "url":"http://app.quaderno.io/my-account/api/v1/invoices/507693322f412e0e2e0000da"
}
```

## Deliver invoice
`GET /invoices/1/deliver.json` will send the invoice to the contact email.
This will return `200 OK` if the deliver was a success along the current JSON representation of the invoice with the registered activities for the document. If the destination contact has not  an email you'll see `422` 

## Create invoice
`POST /invoices.json` will create a new invoice from the parameters passed.
```json
{
  "contact_id":"5059bdbf2f412e0901000024",
  "contact_name":"STARK",
  "po_number":"",
  "currency":"USD",
  "items":[
    {
      "description":"Whiskey",
      "quantity":"1.0",
      "unit_price":"20.0",
      "discount_rate":"0.0",
    }
  ],
}
```

Mandatory fields:

* contact_id: identifier of the contact. 
* contact_name: name of the contact.
* items: An array of hashes which contains the description, quantity, unit price and discount rate of each item. 

This will return `201 Created`, with the location of the new invoice in the Location header along with the current JSON representation of the invoice if the creation was a success.  If the user does not have access to create new invoices, you'll see `401 Unauthorized`.

## Update invoice
`PUT /invoices/1.json` will update the invoice from the parameters passed.
```json
{
  "tags":"whiskey, alcohol"
  "notes":"You better pay this time, Tony"
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the invoice. If the user does not have access to update the invoice, you'll see `401 Unauthorized`.

## Delete invoice
`DELETE /invoices/1.json` will delete the invoice specified and return `204 No Content` if that was successful. If the user does not have access to delete the invoice, you'll see `401 Unauthorized`.


