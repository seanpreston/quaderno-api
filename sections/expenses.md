# Expenses
Expenses are all the invoices that you receive from your vendors.

## Get expenses
`GET /expenses.json` will return all your expenses.
```json
[
  {
    "id":"5076a6b92f412e0e2e00006c",
    "number":"00211",
    "issue_date":"2012-10-11",
    "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"ACME"
    },
    "po_number":"",
    "currency":"USD",
    "items":[
      {
        "id":"48151623423",
        "description":"Rockets",
        "quantity":"50.0",
        "unit_price":"125.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal":"$6,250.00",
        "discount":"$0.00",
        "gross_amount":"$6,250.00"
      }
    ],
    "subtotal":"$6,250.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$6,250.00",
    "payments":[],
    "tag_list":["rockets", "acme"],
    "notes":"",
    "state":"outstanding",
    "url":"https://quadernoapp.com/my-account/api/v1/expenses/5076a6b92f412e0e2e00006c"
  },
  {
    "id":"5076a6b92f412e0e2e00016d",
    "number":"00211",
    "issue_date":"2012-10-11",
    "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"ACME"
    },
    "po_number":"",
    "currency":"USD",
    "items":[
      {
      "id":" 48151623424",
      "description":"TNT",
      "quantity":"100.0",
      "unit_price":"25.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal":"$2,500.00",
      "discount":"$0.00",
      "gross_amount":"$2,500.00"
    }
    ],
    "subtotal":"$2,500.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$2,500.00",
    "payments":[],
    "tag_list":["tnt", "acme"],
    "notes":"",
    "state":"outstanding",
    "url":"https://quadernoapp.com/my-account/api/v1/expenses/5076a6b92f412e0e2e00016d"
  },

]
```

* You can filter the results by number, contact name or P.O. number by passing the `q` parameter in the url like `?q=keyword`.
* You can also can use a date range as a filter by passing the `date` parameter in the url like `?date=DATE1,DATE2`.
* State filtering is also available by passing the `state` parameter in the url like `?state=STATE`.


## Get expense
`GET /expenses/1.json` will return the specified expense.
```json
{
  "id":"5076a6b92f412e0e2e00006c",
  "number":"00211",
  "issue_date":"2012-10-11",
  "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"ACME"
  },
  "po_number":"",
  "currency":"USD",
  "items":[
    {
      "id":"48151623423",
      "description":"Rockets",
      "quantity":"50.0",
      "unit_price":"125.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal":"$6,250.00",
      "discount":"$0.00",
      "gross_amount":"$6,250.00"
    }
  ],
  "subtotal":"$6,250.00",
  "discount":"$0.00",
  "taxes":[],
  "total":"$6,250.00",
  "payments":[],
  "tag_list":["rockets", "acme"],
  "notes":"",
  "state":"outstanding",
  "url":"https://quadernoapp.com/my-account/api/v1/expenses/5076a6b92f412e0e2e00006c"
}
```
## Create expense
`POST /expenses.json` will create a new expense from the parameters passed.
```json
{     
  "contact_id":"5059bdbf2f412e0901000024",
  "contact_name":"ACME",
  "currency":"USD",
  "items_attributes":[
    {
    "description":"Rocket launcher",
    "quantity":"1.0",
    "unit_price":"0.0",
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


* __items_attributes:__ An array of hashes which contains the description, quantity, unit price and discount rate of each item. If you want to have stock tracking pass the item code as the `reference` attribute. See more about stock tracking [here](https://github.com/recrea/quaderno-api/blob/master/sections/items.md#create-item). You can also add items to an expebse by passing the reference of a pre-created item. See more [here](https://github.com/recrea/quaderno-api/blob/master/sections/items.md#adding-an-item-to-a-document-by-reference).

This will return `201 Created`, with the current JSON representation of the expense if the creation was a success along the location of the new expense in the 'url' field .  If the user does not have access to create new expenses, you'll see `401 Unauthorized`.

### Expense states

Possible expense states are:

* outstanding
* paid
* late
* archived

You can set the estimate state by passing the `state` attribute, but bear these considerations in mind:

* The `paid` state is only reachable by adding a `payment` and it is also final so you cannot overwrite it unless you remove the associated payments.

### Create an attachment during expense creation

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

Valid files extension are ```pdf txt jpg jpeg png xml xls doc rtf html```, any other format will make unable the expense creation and will return a ```422```.

## Update expense
`PUT /expenses/1.json` will update the expense from the parameters passed.
```json
{
  "po_number":"",
  "tag_list":"rocket, launcher"
  "payment_details":"Money in da bank",
  "notes":""
} 
```
This will return `200 OK` if the update was a success along with the current JSON representation of the expense. If the user does not have access to update the expense, you'll see `401 Unauthorized`.

### Save an invoice as an expense

`GET /expenses/save.json` will save another account invoice as expense using the permalink passed as parameter

```json
{ 
  "permalink":"c2d480801d0e577c6c3177ce0795c4bdaa480e22"
}

```
This will return `201 Created`, with the current JSON representation of the expense if the creation was a success along the location of the new expense in the 'url' field .  If the user does not have access to create new expenses, you'll see `401 Unauthorized` or a `422` if the permalink is invalid or the invoice is not suitable to be saved.

## Delete expense
`DELETE /expenses/1.json` will delete the expense specified and return `204 No Content` if that was successful. If the user does not have access to delete the expense, you'll see `401 Unauthorized`.
