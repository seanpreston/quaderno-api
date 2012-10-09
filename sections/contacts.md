# Contacts
A contact is any client or vendor who appears on any of your invoices or expenses.
 
## Get contacts
`GET /contacts.json` will return all your contacts.

```json
[
  {
    "id":"456987213",
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "url":"http://endeve.com/my-account/api/v1/contacts/456987213"
  },
  {
    "id":"456982365",
    "kind":"company",
    "first_name":"Apple Inc.",
    "last_name":"",
    "full_name":"Apple Inc.",
    "street_line_1":"1 Infinite Loop",
    "street_line_2":"",
    "postal_code":"95014",
    "city":"Cupertino",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"info@apple.com",
    "web":"http://apple.com",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "url":"http://endeve.com/my-account/api/v1/contacts/456982365"
  }
]
```

## Get contact
`GET /contacts/1.json` will return the specified contact.

```json
{
    "id":"456987213",
    "kind":"person",
    "first_name":"Sheldon",
    "last_name":"Cooper",
    "full_name":"Sheldon Cooper",
    "street_line_1":"2311 N. Los Robles Avenue",
    "street_line_2":"",
    "postal_code":"91104",
    "city":"Pasadena",
    "region":"CA",
    "country":"US",
    "phone_1":"",
    "phone_2":"",
    "fax":"",
    "email":"s.cooperphd@yahoo.com",
    "web":"",
    "discount":null,
    "tax_id":"",
    "language":"EN",
    "notes":"",
    "url":"http://endeve.com/my-account/api/v1/contacts/456987213"
}
```

## Create contact
`POST /contacts.json` will create a new contact from the parameters passed.

```json
{
    "kind":"person",
    "first_name":"Homer",
    "last_name":"Simpson"
}
```

This will return `201 Created`, with the location of the new contact in the Location header along with the current JSON representation of the contact if the creation was a success.  If the user does not have access to create new contacts, you'll see `401 Unauthorized`.

## Update contact
`PUT /contacts/1.json` will update the contact from the parameters passed.

```json
{
    "full_name":"Homer Jay Simpson",
    "street_line_1":"742 Evergreen Terrace",
    "postal_code":"90701",
    "city":"Sprinfield",
    "region":"CA",
    "country":"US",
    "phone_1":"555-7334",
    "phone_2":"555-3223",
    "email":"chunkylover53@aol.com"	
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the contact. If the user does not have access to update the contact, you'll see `401 Unauthorized`.

## Delete contact
`DELETE /contacts/1.json` will delete the contact specified and return `204 No Content` if that was successful. If the user does not have access to delete the contact, you'll see `401 Unauthorized`.
