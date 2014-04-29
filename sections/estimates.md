# Estimates
An estimate is an offer that you give a client in order to get a specific job. With the time, estimates are usually turned into issued invoices. 

## Get estimates
`GET /estimates.json` will return all your estimates.
```json
[
  {  
    "id":"50603e722f412e0435000024",
    "number":"0000003",
    "issue_date":"2012-09-24",
    "contact":{
      "id":"5059bdbf2f412e0901000024",
      "full_name":"Wild E. Coyote"
    },
    "po_number":"",
    "currency":"EUR",
    "items":[
      {
        "id":"4815162342",
        "description":"ACME TNT",
        "quantity":"1.0",
        "unit_price":"100.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal":"\u20ac100.00",
        "discount":"\u20ac0.00",
        "gross_amount":"\u20ac100.00"
       }
    ],
    "subtotal":"\u20ac100.00",
    "discount":"\u20ac0.00",
    "taxes":[],
    "total":"\u20ac100.00",
    "payment_details":"",
    "notes":"",
    "state":"draft",
    "tag_list":[],
    "permalink":"https://quadernoapp.com/estimate/7hef1rs7p3rm4l1nk",
    "url":"https://quadernoapp.com/my-account/api/v1/estimates/50603e722f412e0435000024.json"
  },
  {  
    "id":"50603e722f412e0435000144",
    "number":"0000005",
    "issue_date":"2012-09-24",
    "contact":{
      "id":"5059bdbf2f412e0901000044",
      "full_name":"Cookie Monster"
    },
    "po_number":"",
    "currency":"EUR",
    "items":[
      {
        "id":"48151623421",
        "description":"Cookies",
        "quantity":"5.0",
        "unit_price":"1.95",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "subtotal":"\u20ac9.75",
        "discount":"\u20ac0.00",
        "gross_amount":"\u20ac9.75"
       }
    ],
    "subtotal":"\u20ac9.75",
    "discount":"\u20ac0.00",
    "taxes":[],
    "total":"\u20ac9.75",
    "payment_details":"",
    "notes":"",
    "state":"draft",
    "tag_list":[],
    "permalink":"https://quadernoapp.com/estimate/7hes3c0ndp3rm4l1nk",
    "url":"https://quadernoapp.com/my-account/api/v1/estimates/50603e722f412e0435000144.json"
  },
]
```

## Get estimate
`GET /estimates/1.json` will return the specified estimate.
```json
{  
  "id":"50603e722f412e0435000024",
  "number":"0000003",
  "issue_date":"2012-09-24",
  "contact":{
    "id":"5059bdbf2f412e0901000024",
    "full_name":"Wild E. Coyote"
  },
  "po_number":"",
  "currency":"EUR",
  "items":[
    {
      "id":"4815162342"
      "description":"ACME TNT",
      "quantity":"1.0",
      "unit_price":"\u20ac100.0",
      "discount_rate":"0.0",
      "tax_1_name":"",
      "tax_1_rate":"",
      "tax_2_name":"",
      "tax_2_rate":"",
      "subtotal":"\u20ac100.00",
      "discount":"\u20ac0.00",
      "gross_amount":"\u20ac100.00"
    }
  ],
  "subtotal":"\u20ac100.00",
  "discount":"\u20ac0.00",
  "taxes":[],
  "total":"\u20ac100.00",
  "payment_details":"",
  "notes":"",
  "state":"draft",
  "tag_list":[],
  "permalink":"https://quadernoapp.com/estimate/7hef1rs7p3rm4l1nk",
  "url":"https://quadernoapp.com/my-account/api/v1/estimates/50603e722f412e0435000024.json"
}
```
## Deliver estimate
`GET /estimates/1/deliver.json` will send the estimate to the contact email.
This will return `200 OK` if the deliver was a success along the current JSON representation of the invoice with the registered activities for the document. If the destination contact has not  an email you'll see `422` 

## Create estimate
`POST /estimates.json` will create a new estimate from the parameters passed.
```json
{
  "number":"0000006",
  "contact_id":"50603e722f412e0435000024",
  "contact_name":"Wild E. Coyote",
  "po_number":"",
  "currency":"EUR",
  "items_attributes":[
    {
        "description":"ACME Catapult",
        "quantity":"1.0",
        "unit_price":"0.0",
        "discount_rate":"0.0",
        "tax_1_name":"",
        "tax_1_rate":"",
        "tax_2_name":"",
        "tax_2_rate":"",
        "reference":"item_code_X"
      }
  ],
  "tag_list":"tnt",
  "payment_details":"",
  "notes":""
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


* __items_attributes:__ An array of hashes which contains the description, quantity, unit price and discount rate of each item. If you want to have stock tracking pass the item code as the `reference` attribute. See more about stock tracking [here](https://github.com/recrea/quaderno-api/blob/master/sections/items.md#create-item). You can also add items to an estimate by passing the reference of a pre-created item. See more [here](https://github.com/recrea/quaderno-api/blob/master/sections/items.md#adding-an-item-to-a-document-by-reference).

This will return `201 Created`, with the current JSON representation of the estimate if the creation was a successthe along the location of the new estimate in the 'url' field .  If the user does not have access to create new estimates, you'll see `401 Unauthorized`.

### Create an attachment during estimate creation

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

Valid files extension are ```pdf txt jpg jpeg png xml xls doc rtf html```, any other format will make unable the estimate creation and will return a ```422```.

## Update estimate
`PUT /estimates/1.json` will update the estimate from the parameters passed.
```json
{ 
  "tag_list":"Wacky, racer"
  "contact_id":"505c3b402f412e0248000044",
  "contact_name":"Dick Dastardly",
 }
```

This will return `200 OK` if the update was a success along with the current JSON representation of the estimate. If the user does not have access to update the contact, you'll see `401 Unauthorized`.


## Delete contact
`DELETE /estimates/1.json` will delete the estimate specified and return `204 No Content` if that was successful. If the user does not have access to delete the estimate, you'll see `401 Unauthorized`.
