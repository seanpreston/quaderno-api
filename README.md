# Getting started
The Quaderno API is based on REST: it is comprised of resources with predictable urls and utilises standard HTTP features (like HTTP Basic Authentication, Response Codes and Methods). All requests, including errors, return JSON. The API expects JSON for all POST and PUT requests.

## Making a request
All URLs starts with `https://ACCOUNT-NAME.quadernoapp.com/api/v1/`. The host is formed by the `ACCOUNT-NAME` as the subdomain and the `quadernoapp.com` domain. The path is prefixed with the account name and the API version. You can get your account name from your account.

To make a request for all the contacts in your account for instance, you have to append the contacts index path to the base url to form something like `https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json`. In curl, it would look like:

```shell
curl -u API-KEY:x -X GET 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

To create something, it is similar but you also have to include the Content-Type header and the JSON data:

```shell
curl -u API-KEY:x 
	 -H 'Content-Type: application/json' 
	 -X POST 
	 -d {"first_name":"Tony", "kind":"person", "contact_name":"Stark"}
	 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

We only support JSON for serialization of data. Our format is to have no root element and we use snake_case to describe attribute keys. This means that you have to send Content-Type: application/json; charset=utf-8 when you're POSTing or PUTing data into Quaderno. **All API URLs end in .json to indicate that they accept and return JSON**.

## Authentication
You authenticate to the Quaderno API by providing your API key in the request. You can manage your API keys from your account.

Authentication to the API occurs via HTTP Basic Auth. Provide your API key as the basic auth username. You do not need to provide a password.

```shell
curl -u API-KEY:x https://ACCOUNT-NAME.quadernoapp.com/api/v1/invoices.json
```

You can get more information in the [Authentication page](https://github.com/quaderno/quaderno-api/blob/master/sections/authentication.md).

## API resources
* [Contacts] (https://github.com/quaderno/quaderno-api/blob/master/sections/contacts.md)
* [Invoices] (https://github.com/quaderno/quaderno-api/blob/master/sections/invoices.md)
* [Expenses] (https://github.com/quaderno/quaderno-api/blob/master/sections/expenses.md)
* [Estimates] (https://github.com/quaderno/quaderno-api/blob/master/sections/estimates.md)
* [Items] (https://github.com/quaderno/quaderno-api/blob/master/sections/items.md)
* [Payments] (https://github.com/quaderno/quaderno-api/blob/master/sections/payments.md)
* [Taxes] (https://github.com/quaderno/quaderno-api/blob/master/sections/taxes.md)
* [Webhooks] (https://github.com/quaderno/quaderno-api/blob/master/sections/webhooks.md)

## Rate limiting

There is a time limit of 100 API calls per 15 seconds. We reserve the right to tune the limitations, but they are always set high enough to allow a well-behaving interactive program to do its job. If you exceed this rate limit you will receive a HTTP 503 (Service Unavailable).

To make it easier for your application to determine if it is being rate-limited, or if it is likely to be in the future, we've added the following HTTP headers to successful responses:

* X-RateLimit-Reset
* X-RateLimit-Remaining

## Pagination
Please keep in mind that Quaderno paginates the index results, showing 25 results per page. You can change the page by passing the parameter ```page``` (default page is 1). 
The following headers will inform you about the page context (current page and total pages availables):

* X-Pages-CurrentPage
* X-Pages-TotalPages  

## Ping the API
In order to test if the service is available, test your authenticate token or know the remaining requests without doing an actual request, you can ping it as follows with no rate limiting cost:

```shell
curl -u API-KEY:x 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/ping.json'
```
## Errors
If the app is having trouble, you might see a 5xx error. 500 means that the app is entirely down, but you might also see 502 Bad Gateway, 503 Service Unavailable, or 504 Gateway Timeout. It's your responsibility in all of these cases to retry your request later.

## API kits
At the moment, API kits are available in Ruby and PHP5. Their goal is to make integrating with our API as easy as possible.

**Ruby**: [https://github.com/quaderno/quaderno-ruby](https://github.com/quaderno/quaderno-ruby)  
**PHP**: [https://github.com/quaderno/quaderno-php](https://github.com/quaderno/quaderno-php)  
**iOS**: [https://github.com/quaderno/quaderno-ios](https://github.com/quaderno/quaderno-ios)

## Improvements
Please tell us how we can make the API better. If you have a specific feature request or if you found a bug, please use GitHub issues. Fork these docs and send a pull request with improvements.

Thanks to 37-signals' BCX API for their inspiration.
