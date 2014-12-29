# Invoices
An invoice is a detailed list of goods shipped or services rendered, with an account of all costs.

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
        "id":"48151623429",
        "description":"lasagna",
        "quantity":"25.0",
        "unit_price":"3.75",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Awesome!",
        "subtotal":"$93.75",
        "discount":"$0.00",
        "gross_amount":"$93.75"
      }
    ],
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
        "url":"https://my-account.quadernoapp.com/api/v1/invoices/507693322f412e0e2e00000f/payments/50aca7d92f412eda5200002c.json"
      },
    ],
    "payment_details":"Ask Jon",
    "notes":"",
    "state":"paid",
    "tag_list":["lasagna", "cat"],
    "secure_id":"7hef1rs7p3rm4l1nk",
    "permalink":"https://quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/invoices/507693322f412e0e2e00000f"
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
        "id":"481516234291",
        "description":"pizza",
        "quantity":"15.0",
        "unit_price":"60.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"Even the bad ones taste good!",
        "subtotal":"$60.00",
        "discount":"$0.00",
        "gross_amount":"$60.00"
      }
    ],
    "subtotal":"$60.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$60.00",
    "payments":[],
    "payment_details":"",
    "notes":"",
    "state":"draft",
    "tag_list":["pizza", "turtles"],
    "secure_id":"7hes3c0ndp3rm4l1nk",
    "permalink":"https://my-account.quadernoapp.com/invoice/7hes3c0ndp3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/invoices/507693322f412e0e2e0000da"
  },
]
```
* You can filter the results by number, contact name or P.O. number by passing the `q` parameter in the url like `?q=keyword`.
* You can also can use a date range as a filter by passing the `date` parameter in the url like `?date=DATE1,DATE2`.
* State filtering is also available by passing the `state` parameter in the url like `?state=STATE`.
* If you want to index the invoices for a specific contact you can do it by passing the contact id in the `contact` parameter like `?contact=3231`. 

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
      "id":"48151623429",
      "description":"pizza",
      "quantity":"15.0",
      "unit_price":"60.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "reference":"Even the bad ones taste good!",
      "subtotal":"$60.00",
      "discount":"$0.00",
      "gross_amount":"$60.00"
    }
  ],
  "subtotal":"$60.00",
  "discount":"$0.00",
  "taxes":[],
  "total":"$60.00",
  "payments":[],
  "payment_details":"",
  "notes":"",
  "state":"draft",
  "tag_list":["lasagna", "cat"],
  "secure_id":"7hef1rs7p3rm4l1nk",
  "permalink":"https://my-account.quadernoapp.com/invoice/7hef1rs7p3rm4l1nk",
  "url":"https://my-account.quadernoapp.com/api/v1/invoices/507693322f412e0e2e0000da"
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
  "tag_list":"playboy, businessman",
  "items_attributes":[
    {
      "description":"Whiskey",
      "quantity":"1.0",
      "unit_price":"20.0",
      "discount_rate":"0.0",
      "reference":"item_code_X"
    }
  ],
}
```

Mandatory fields:

* __contact_id:__ identifier of the contact. Alternatively, __you can use the__ ´contact´ __attribute instead ´contact_id´__. For example:
  ```json
  {
    "contact":{
      "first_name":"Tony",
      "last_name":"Stark",
      "type":"person"
    },
    "po_number":"",
    "currency":"USD",
    "tag_list":"playboy, businessman",
    "items_attributes":[
      {
        "description":"Whiskey",
        "quantity":"1.0",
        "unit_price":"20.0",
        "discount_rate":"0.0",
        "reference":"item_code_X"
      }
    ],
  }
  ```
  This have two possible consequences, if the contact first_name and last name combination does not match any of your existing contacts, it will be created, otherwise the invoice will be created for the matching result. Please keep in mind that __you can pass__ `contact_id` __or__ `contact` __but not both of them at the same time, results might not be what you've expected.__

  Allowed fields for `contact` are the same as when creating a contact via the contacts API. You can get more information [here](https://github.com/recrea/quaderno-api/blob/master/sections/contacts.md).


* __items_attributes:__ An array of hashes which contains the description, quantity, unit price and discount rate of each item. If you want to have stock tracking pass the item code as the `reference` attribute. See more about stock tracking [here](https://github.com/recrea/quaderno-api/blob/master/sections/items.md#create-item). You can also add items to an invoice by passing the reference of a pre-created item. See more [here](https://github.com/recrea/quaderno-api/blob/master/sections/items.md#adding-an-item-to-a-document-by-reference).

This will return `201 Created`, with the current JSON representation of the invoice if the creation was a success along the location of the new invoice in the 'url' field .  If the user does not have access to create new invoices, you'll see `401 Unauthorized`.

### Invoices states

Possible invoice states are:

* draft
* sent
* partial
* paid
* late
* archived

You can set the invoice state by passing the `state` attribute, but bear these considerations in mind:

* The `paid` state is only reachable by adding a `payment` to the invoice and it is also final, so you cannot overwrite it unless you remove the associated payments.
* The `sent` state only can overwrite the state `draft`.

### Create an attachment during invoice creation

Optionally, you can pass the following key-value along with the rest of the parameters:

```json
{
  "attachment":{
    "data":"aBaSe64EnCoDeDFiLe",
    "filename":"the_filename.png"
  }
}
```
Fields:
* data: contains a Base64 encoded string which represents the file.
* filename: attachment file name.

Valid files extension are ```pdf txt jpg jpeg png xml xls doc rtf html```, any other format will make unable the invoice creation and will return a ```422```.

## Update invoice
`PUT /invoices/1.json` will update the invoice from the parameters passed.
```json
{
  "tag_list":"whiskey, alcohol",
  "notes":"You better pay this time, Tony"
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the invoice. If the user does not have access to update the invoice, you'll see `401 Unauthorized`.

## Delete invoice
`DELETE /invoices/1.json` will delete the invoice specified and return `204 No Content` if that was successful. If the user does not have access to delete the invoice, you'll see `401 Unauthorized`.


