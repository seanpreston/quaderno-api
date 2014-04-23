# Webhooks 
Quaderno Webhooks allows your aplication to receive information about document events as they occur. 

## Data formats
Every webhook uses the same data format, regardless of the event type. The webhook request is a standard POST along with a hash with the following parameters:
                                                                                        
* event_type - The event which triggered the webhook (invoice.created, estimate.updated, contact.deleted, ...).
* data - Contains a simplified json representation of the object.


```json
{
  "event_type":"invoice.created", 
  "data":
  {
    "object":
    {
      "id":"26", 
      "contact_id":"2", 
      "number":"XX00001281", 
      "issue_date":"2014-04-22", 
      "contact_name":"Hello Test APP", 
      "currency":"EUR", 
      "gross_amount_cents":"1000", 
      "total_cents":"1000", 
      "amount_paid_cents":"0", 
      "po_number":"", 
      "due_days":"", 
      "due_date":"", 
      "payment_details":"", 
      "notes":"", 
      "permalink":"20efc2ea3e9bc7060aaab1dd8c2c6b85a427b30d", 
      "state":"draft"
    }
  }
}
```

```json
{
  "event_type":"contact.updated", 
  "data":
  {
    "object":
    {
      "id":"4", 
      "kind":"company", 
      "first_name":"Adella Schowalter", 
      "last_name":"", 
      "full_name":"Adella Schowalter", 
      "contact_name":"", 
      "street_line_1":"", 
      "street_line_2":"", 
      "postal_code":"", 
      "city":"", 
      "region":"", 
      "country":"US", 
      "phone_1":"", 
      "fax":"", 
      "email":"", 
      "web":"", 
      "discount":"", 
      "language":"DE", 
      "tax_id":"", 
      "bank_account":"ES6600190020961234567890", 
      "notes":"", 
      "bic":"DEUTESBBXXX"}}
    }
  }
}
```



## Event types
Event types are combinations between the objects you want to be notified about and the object state.

Available objects at the moment are:
* invoice
* expense
* estimate

The following states are supported at the moment:
* created - The object has been created
* updated - The object has been updated
* deleted - The objecy has been deleted


For example, if you want to be notified whenever an invoice is created or deleted, events types should be `invoice.created` and `invoice.deleted`.

## Get webhooks
`GET /webhooks.json` will return all your webhooks.

```json
[
  {
    "id":2,
    "url":"https://myawesomeapp.com/notifications",
    "auth_key":"zXQgArTtQxAMaYppMrDoUQ",
    "events":["created","updated"],
    "last_sent_at":"2013-05-18T11:11:11Z",
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-05-17T14:08:05Z",
    "updated_at":"2013-05-17T14:08:05Z"
  }
  {
    "id":3,
    "url":"http://anotherapp.com/notifications",
    "auth_key":"HXQgAgblQxAMaNppMrXoSW",
    "events:_types":["invoice.created","contact.deleted"],
    "last_sent_at":null,
    "last_error":null,
    "events_sent":null,
    "created_at":"2013-07-13T14:12:01Z",
    "updated_at":"2013-07-17T14:09:59Z"
  }
]
```

## Get webhook
`GET /webhooks/3.json` will return the specified webhook.

```json
{
  "id":3,
  "url":"http://anotherapp.com/notifications",
  "auth_key":"HXQgAgblQxAMaNppMrXoSW",
  "events":["created","deleted"],
  "last_sent_at":"2013-05-18T11:11:11Z",
  "last_error":null,
  "events_sent":null,
  "created_at":"2013-05-13T14:12:01Z",
  "updated_at":"2013-05-17T14:09:59Z"
}
```

## Create webhook
`POST /webhooks.json` will create a new webhook from the parameters passed.

```json
{
    "url":"http://anotherapp.com/notifications",
    "events_types":["invoice.created", "estimate.updated", "invoice.deleted", "contact.created"]
}
```
Mandatory fields:

* url: indicates the destination URL of the webhook request. Webhook URLs should be set up to accept HEAD and POST requests. When you provide the URL where you want Quaderno to POST the data for events, we'll do a quick check that the URL exists by using a HEAD request (not POST). If the URL doesn't exist or returns something other than a 200 HTTP response to the HEAD request, Quaderno won't be able to verify that the URL exists and is valid.
* events_types: an array of strings that indicates which events will trigger the webhook request.

This will return `201 Created` with the current JSON representation of the webhook if the creation was a success.  If the user does not have access to create new webhooks, you'll see `401 Unauthorized`.

## Update webhook
`PUT /webhooks/1.json` will update the webhook from the parameters passed.

```json
{
  "events_types":["contact.updated", "estimate.deleted"]
}
```

This will return `200 OK` if the update was a success along with the current JSON representation of the webhook. If the user does not have access to update the webhook, you'll see `401 Unauthorized`.

## Delete webhook
`DELETE /webhooks/1.json` will delete the webhook specified and return `204 No Content` if that was successful. If the user does not have access to delete the webhook, you'll see `401 Unauthorized`.


##Authenticating webhook requests

Quaderno signs webhook requests so you can (optionally) verify that requests are generated by Quaderno and not a third-party pretending to be Quaderno. 

###Verifying Request Signatures

Quaderno includes an additional HTTP header with webhook POST requests, X-Quaderno-Signature, which will contain the signature for the request. To verify a webhook request, generate a signature using the same key that Quaderno uses and compare that to the value of the  X-Quaderno-Signature header.

###Get your webhook authentication key

When you create a webhook, a key is automatically generated. If you're using POST /webhooks.json, the key will be returned in the response. To retrieve a webhook key via the Quaderno API, use GET /webhooks.json or GET webhooks/1.json.

###Generate a signature

In your code that receives or processes webhook requests:

* Create a string with the webhook's URL, exactly as you entered it in Quaderno (including any query strings, if applicable). Quaderno always signs webhook requests with the exact URL you provided when you configured the webhook. A difference as small as including or removing a trailing slash will prevent the signature from validating.
* Sort the request's POST variables alphabetically by key.
* Append each POST variable's key and value to the URL string, with no delimiter.
* Hash the resulting string with HMAC-SHA1, using your webhook's authentication key to generate a binary signature.
* Base64 encode the binary signature
* Compare the binary signature that you generated to the signature provided in the X-Quaderno-Signature HTTP header.

Example PHP implementation

```php
/**
 * Generates a base64-encoded signature for a Quaderno webhook request.
 * @param string $webhook_key the webhook's authentication key
 * @param string $url the webhook url
 * @param array $params the request's POST parameters
 */
function generateSignature($webhook_key, $url, $params) {
    $signed_data = $url;
    ksort($params);
    foreach ($params as $key => $value) {
        $signed_data .= $key;
        $signed_data .= $value;
    }
    
    return base64_encode(hash_hmac('sha1', $signed_data, $webhook_key, true));
}
```
