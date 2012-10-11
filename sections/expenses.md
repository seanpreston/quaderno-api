# Expenses
Expenses are all the invoices that you receive from your vendors.

## Get expenses
`GET /expenses.json` will return all your expenses.
```json
[
  {
    "id":"50769be02f412e0e2e000054",
	"number":"5",
	"issue_date":"2012-10-11",
	"contact":{
	  "id":"5073f9c22f412e02d0000032",
	  "full_name":"ACME"
	},
	"po_number":null,
	"currency":"USD",
	"items":[
	  {
	    "description":"ACME TNT",
	    "quantity":"23.0",
	    "unit_price":"75.0",
		"discount_rate":"0.0",
	    "tax_1_name":null,
	    "tax_1_rate":null,
	    "tax_2_name":null,
	    "tax_2_rate":null,
	    "subtotal":"\u20ac0.00",
	    "discount":"\u20ac0.00",
    	"gross_amount":"\u20ac0.00"
	  }
	],
	"subtotal":"\u20ac0.00",
	"discount":"\u20ac0.00",
	"taxes":[],
	"total":"\u20ac0.00",
	"payments":[],
	"tags":"tnt",
	"notes":"",
	"state":"outstanding",
	"url":"http://quaderno.com/my-account/api/v1/expenses/50769be02f412e0e2e000054"
  },

  {
    "id":"50769be02f412e0e2e0002c4",
	"number":"5",
	"issue_date":"2012-10-11",
	"contact":{
	  "id":"5073f9c22f412e02d0000032",
	  "full_name":"ACME"
	},
	"po_number":null,
	"currency":"USD",
	"items":[
	  {
	    "description":"Rockets",
	    "quantity":"50.0",
	    "unit_price":"125.0",
		"discount_rate":"0.0",
	    "tax_1_name":null,
	    "tax_1_rate":null,
	    "tax_2_name":null,
	    "tax_2_rate":null,
	    "subtotal":"\u20ac0.00",
	    "discount":"\u20ac0.00",
    	"gross_amount":"\u20ac0.00"
	  }
	],
	"subtotal":"\u20ac0.00",
	"discount":"\u20ac0.00",
	"taxes":[],
	"total":"\u20ac0.00",
	"payments":[],
	"tags":"tnt",
	"notes":"",
	"state":"outstanding",
	"url":"http://quaderno.com/my-account/api/v1/expenses/50769be02f412e0e2e0002c4"
  },
]
```
## Create expense
`POST /expenses.json` will create a new expense from the parameters passed.
```json

```
This will return `201 Created`, with the location of the new expense in the Location header along with the current JSON representation of the expense if the creation was a success.  If the user does not have access to create new expenses, you'll see `401 Unauthorized`.

## Update expense
`PUT /expenses/1.json` will update the expense from the parameters passed.
```json
```

This will return `200 OK` if the update was a success along with the current JSON representation of the expense. If the user does not have access to update the expense, you'll see `401 Unauthorized`.

## Delete expense
`DELETE /expenses/1.json` will delete the expense specified and return `204 No Content` if that was successful. If the user does not have access to delete the expense, you'll see `401 Unauthorized`.