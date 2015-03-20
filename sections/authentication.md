# Authentication

## API Key

You authenticate to the Quaderno API by providing your API key in the request. You can manage your API keys from your account.

Authentication to the API occurs via HTTP Basic Auth. Provide your API key as the basic auth username. You do not need to provide a password.

```shell
curl -u API-KEY:x 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts.json'
```

Remember that anyone who has your API key can see and change everything you have access to. So you want to guard that as well as you guard your passwords.

## Authorization

If you don't know your ACCOUNT-NAME, you can get it with the authorization method.  

`GET https://quadernoapp.com/api/v1/authorization.json` will return the user's data

```json
{
  "id": "999",
  "name": "Sheldon Cooper",
  "email": "s.cooperphd@yahoo.com",
  "href": "https://nippur-999.quadernoapp.com/api/v1/"
}
```
* An `identity`, which is **NOT** used for determining who this user is within Quaderno. The `id` field should **NOT** be used for submitting data within Quaderno's API.
* The `name` of the user
* The `email` of the user
* The account's `href` that you can use to make requests to the API.
