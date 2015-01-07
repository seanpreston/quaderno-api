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
    "secure_id":"th3p3rm4l1nk",
    "permalink":"https:///my-account.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/contacts/456987213"
  },
  {
    "id":"456982365",
    "kind":"company",
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
    "secure_id":"4n0th3rp3rm4l1nk",
    "permalink":"https:///my-account.quadernoapp.com/billing/4n0th3rp3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/contacts/456982365"
  }
]
```
You can filter the results by full name, email or tax ID by passing the `q` parameter in the url like `?q=KEYWORD`.

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
    "secure_id":"th3p3rm4l1nk",
    "permalink":"https:///my-account.quadernoapp.com/billing/th3p3rm4l1nk",
    "url":"https://my-account.quadernoapp.com/api/v1/contacts/456987213"
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
Mandatory fields:

* kind: indicates if the contact is whether a person or a company.
* first_name: name of the contact.
* bic: This field is mandatory as long as the bank_account (in electronic IBAN format) is present. Must be 11 characters long.


This will return `201 Created`, with the current JSON representation of the contact if the creation was a success along the location of the new contact in the 'url' field .  If the user does not have access to create new contacts, you'll see `401 Unauthorized`.

## Update contact
`PUT /contacts/1.json` will update the contact from the parameters passed.

```json
{
    "last_name":"Jay Simpson",
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
