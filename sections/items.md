# Items
The items are those products or services that you sell to your customers.
 
## Get items
`GET /items.json` will return all your items.

```json
[
  {
    "id":1,
    "code":"BluRay 0000",
    "name":"Titanic",
    "unit_cost":"15.0",
    "stock":"100",
    "url":"https://quadernoapp.com/my-account/api/v1/items/1"
  },
  {
    "id":2,
    "code":"BluRay 0001",
    "name":"Titanic II: The revenge",
    "unit_cost":"15.0",
    "tax_1_name":"AWESOME_TAX",
    "tax_1_rate":"7.00",
    "url":"https://quadernoapp.com/my-account/api/v1/items/2"
  },
  {
    "id":3,
    "code":"BluRay 0002",
    "name":"Titanic III: The origin",
    "unit_cost":"15.0",
    "stock":"33",
    "url":"https://quadernoapp.com/my-account/api/v1/items/3"
  }
]
```

## Get item
`GET /items/1.json` will return the specified item.

```json
{
  "id":2,
  "code":"BluRay 0001",
  "name":"Titanic II: The revenge",
  "unit_cost":"15.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00",
  "url":"https://quadernoapp.com/my-account/api/v1/items/2"
}
```

## Create item
`POST /items.json` will create a new item from the parameters passed.

```json
{
  "code":"BluRay 0003",
  "name":"Titanic IV: The revenge",
  "unit_cost":"15.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00",
  "tax_2_name":"ANOTHER_AWESOME_TAX",
  "tax_2_rate":"10.00",
  "stock":"1000"
}
```
Mandatory fields:

* name: a name or brief description of the item.
* unit_cost: cost of the item.
* code: mandatory __only__ if you want to have stock tracking by defining the value `stock`.

This will return `201 Created`, with the current JSON representation of the item if the creation was a successthe along the location of the new item in the 'url' field .  If the user does not have access to create new items, you'll see `401 Unauthorized`.

## Update item
`PUT /items/1.json` will update the item from the parameters passed.

```json
{
  "unit_cost":"10.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00"
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the item. If the user does not have access to update the item, you'll see `401 Unauthorized`.

## Delete item
`DELETE /items/1.json` will delete the item specified and return `204 No Content` if that was successful. If the user does not have access to delete the item, you'll see `401 Unauthorized`.

## Adding an item to a document

You can use an item attributes to create or update a document (invoice, expense or estimate). In order to use this feature, the item must have a code that must be set as a `reference` in the `items_attributes` array. For example, given this item:

```json
{ 
  "code":"BluRay 0003",
  "name":"Titanic IV: The revenge",
  "unit_cost":"15.0",
  "tax_1_name":"AWESOME_TAX",
  "tax_1_rate":"7.00",
  "tax_2_name":"ANOTHER_AWESOME_TAX",
  "tax_2_rate":"10.00",
  "stock":"1000"
}
```

You can generate an invoice with this item by passing this data via POST to `/invoices.json`:


```json
{
  "contact_id":"5059bdbf2f412e0901000024",
  "contact_name":"STARK",
  "currency":"USD",
  "tag_list":"playboy, businessman",
  "items_attributes":[
    {
      "reference":"BluRay 0003",
      "quantity":"2",
      "unit_cost":"10.95"
    }
  ],
}
```
And that's all, the item in the invoice will pick the referenced item attributes as default values, but of course you can overwrite them by setting different values. In the example, the unit price of the items in the invoice would be $10.95 while the referenced item is $10.00. The default quantity value is one.
