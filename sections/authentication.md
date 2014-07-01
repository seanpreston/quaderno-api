# Authentication

## API Key

HTTP Authentication is used to authenticate with The API. Your username is your Token ID, which is accessible through the "Profile" menu item.

```shell
curl -u token-id:foo 'https://ACCOUNT-NAME.quadernoapp.com/api/v1/contacts'
```

Remember that anyone who has your authentication token can see and change everything you have access to. So you want to guard that as well as you guard your username and password. If you come to fear that it has been compromised, just change your regular password and the authentication token will change too.

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
