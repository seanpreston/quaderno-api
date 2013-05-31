# Getting started
The Quaderno API is based on REST: it is comprised of resources with predictable urls and utilises standard HTTP features (like HTTP Basic Authentication, Response Codes and Methods). All requests, including errors, return JSON. The API expects JSON for all POST and PUT requests.

## Making a request
All URLs starts with `https://quadernoapp.com/ACCOUNT-NAME/api/v1/`. The path is prefixed with the account name and the API version. You can get your account name on your profile page.

To make a request for all the contacts in your account for instance, you have to append the contacts index path to the base url to form something like https://quadernoapp.com/my-account/api/v1/contacts.json. In curl, it would look like:

```shell
curl -u user:pass -X GET 'https://quadernoapp.com/ACCOUNT-NAME/api/v1/contacts.json'
```

To create something, it is similar but you also have to include the Content-Type  header and the JSON data:

```shell
curl -u user:pass 
	 -H 'Content-Type: application/json' 
	 -X POST 
	 -d {"first_name":"Tony", "kind":"person", "contact_name":"Stark"}
	 'https://quadernoapp.com/ACCOUNT-NAME/api/v1/contacts.json'
```

We only support JSON for serialization of data. Our format is to have no root element and we use snake_case to describe attribute keys. This means that you have to send Content-Type: application/json; charset=utf-8 when you're POSTing or PUTing data into Quaderno. **All API URLs end in .json to indicate that they accept and return JSON**.

## Authentication
HTTP Authentication is used to authenticate with The API. Your username is your Token ID, which is accessible through the "Profile" menu item.

```shell
curl -u token-id:foo 'https://quadernoapp.com/ACCOUNT-NAME/api/v1/contacts'
```     

## API resources
* [Contacts] (https://github.com/recrea/quaderno-api/blob/master/sections/contacts.md)
* [Invoices] (https://github.com/recrea/quaderno-api/blob/master/sections/invoices.md)
* [Expenses] (https://github.com/recrea/quaderno-api/blob/master/sections/expenses.md)
* [Estimates] (https://github.com/recrea/quaderno-api/blob/master/sections/estimates.md)
* [Items] (https://github.com/recrea/quaderno-api/blob/master/sections/items.md)
* [Payments] (https://github.com/recrea/quaderno-api/blob/master/sections/payments.md)
* [Webhooks] (https://github.com/recrea/quaderno-api/blob/master/sections/webhooks.md)

## Rate limiting
You can perform up to 2000 requests per month for account. If you exceed this limit, you'll get a `403 Forbiden` response for subsequent requests.
The following headers will inform you about the daily limit and your remaining requests.

* X-RateLimit-Limit
* X-RateLimit-Remaining

## Ping the API
In order to test if the service is available, test your authenticate token or know the remaining requests without doing an actual request, you can ping it as follows with no rate limiting cost:

```shell
curl -u token_id:foo 'https://quadernoapp.com/ACCOUNT-NAME/api/v1/ping.json'
```
## Errors
If the app is having trouble, you might see a 5xx error. 500 means that the app is entirely down, but you might also see 502 Bad Gateway, 503 Service Unavailable, or 504 Gateway Timeout. It's your responsibility in all of these cases to retry your request later.

## API kits
At the moment, API kits are available in Ruby and PHP5. Their goal is to make integrating with our API as easy as possible.

**Ruby**: [https://github.com/recrea/quaderno-ruby](https://github.com/recrea/quaderno-ruby)  
**PHP**: [https://github.com/recrea/quaderno-php](https://github.com/recrea/quaderno-php)

## Improvements
Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use GitHub issues. Fork these docs and send a pull request with improvements.

Thanks to 37-signals' BCX API for their inspiration.
