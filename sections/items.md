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
  "tax_2_rate":"10.00"
}
```
Mandatory fields:

* name: a name or brief description of the item.
* unit_cost: cost of the item.

This will return `201 Created`, with the location of the new item in the Location header along with the current JSON representation of the item if the creation was a success.  If the user does not have access to create new items, you'll see `401 Unauthorized`.

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
