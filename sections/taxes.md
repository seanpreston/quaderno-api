# Taxes

## Calculate taxes
`GET /taxes/calculate.json` will calculate the taxes applied for a given customer data. 

| Parameter | Mandatory | Description |
| --- | --- | --- |
| `country` | Yes | Customer's country (2-letter [ISO code](http://en.wikipedia.org/wiki/ISO_3166-1#Current_codes)) |
| `postal_code` | No | Customer's postal code (ZIP) |
| `vat_number`| No | Customer's VAT number |
| `transaction_type` | No | Values: `eservice`, `ebook`, `standard`. The default value is `eservice`| 

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


