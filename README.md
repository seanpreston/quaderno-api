# Getting started
The Endeve API is based on REST: it is comprised of resources with predictable urls and utilises standard HTTP features (like HTTP Basic Authentication, Response Codes and Methods). All requests, including errors, return JSON. The API expects JSON for all POST and PUT requests.

## Making a request
All URLs starts with `https://endeve.com/my-account/api/v1/`. The path is prefixed with the account name.

To make a request for all the contacts in your account for instance, you have to append the contacts index path to the base url to form something like https://endeve.com/my-account/api/v1/contacts.json. In curl, it would look like:

```shell
curl -u user:pass -X GET 'https://endeve.com/my-account/api/v1/contacts.json'
```

To create something, it is similar but you also have to include the Content-Type  header and the JSON data:

```shell
curl -u user:pass 
	 -H 'Content-Type: application/json' 
	 -X POST 
	 -d {"first_name":"Tony", "kind":"person", "contact_name":"Stark"}
	 'https://endeve.com/my-account/api/v1/contacts.json'
```

We only support JSON for serialization of data. Our format is to have no root element and we use snake_case to describe attribute keys. This means that you have to send Content-Type: application/json; charset=utf-8 when you're POSTing or PUTing data into Endeve. **All API URLs end in .json to indicate that they accept and return JSON**.

## Authentication
HTTP Authentication is used to authenticate with The API. Your username is your Token ID, which is accessible through the "Profile" menu item.

```shell
curl -u token-id:foo 'https://endeve.com/my-account/api/v1/contacts'
```     

## API resources
* [Contacts] (https://github.com/recrea/endeve-api/blob/master/sections/contacts.md)
* [Invoices] (https://github.com/recrea/endeve-api/blob/master/sections/invoices.md)
* [Expenses] (https://github.com/recrea/endeve-api/blob/master/sections/expenses.md)
* [Estimates] (https://github.com/recrea/endeve-api/blob/master/sections/estimates.md)
* [Payments] (https://github.com/recrea/endeve-api/blob/master/sections/payments.md)

## Rate limiting
You can perform up to 100 requests per day for account. If you exceed this limit, you'll get a `403 Forbiden` response for subsequent requests.
The following headers will inform you about the daily limit and your remaining requests.

* X-RateLimit-Limit
* X-RateLimit-Remaining

## Errors
If the app is having trouble, you might see a 5xx error. 500 means that the app is entirely down, but you might also see 502 Bad Gateway, 503 Service Unavailable, or 504 Gateway Timeout. It's your responsibility in all of these cases to retry your request later.

## Improvements
Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use GitHub issues. Fork these docs and send a pull request with improvements.

Thanks to 37-signals' BCX API for their inspiration.