# Taxes

## Calculate taxes
`GET /taxes/calculate.json` will calculate the taxes applied for a given customer data. 

Parameters:

* country: 2 characters representing the country code of the customer in [ISO 3166-1 format](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes).
* postal_code: the customer's zip/postal code.
* vat_number: the customer's VAT number.

All the parameters are optional but omitting relevant information may result in less precise results.

This will return `200 OK` if the request was a success along with taxes applied represented as a json string. If the user does not have access the API, you'll see `401 Unauthorized`.

Example request and response:

`/taxes/calculate.json?country=ES&postal_code=08080&vat_number=ESA58818501`

```json
{
    "name":"VAT",
    "rate":21.0,
    "notes":null
}
```


