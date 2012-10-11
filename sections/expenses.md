# Expenses
Expenses are all the invoices that you receive from your vendors.

## Get expenses
`GET /expenses.json` will return all your expenses.
```json
[
  {
    "id":"5076a6b92f412e0e2e00006c",
    "number":"00211",
    "issue_date":"2012-10-11",
    "contact":{
	  "id":"5059bdbf2f412e0901000024",
	  "full_name":"ACME"
    },
    "po_number":"",
    "currency":"USD",
    "items":[
      {
	    "description":"Rockets",
	    "quantity":"50.0",
	    "unit_price":"125.0",
	    "discount_rate":"0.0",
	    "tax_1_name":"",
	    "tax_1_rate":"",
	    "tax_2_name":"",
	    "tax_2_rate":"",
	    "subtotal":"$6,250.00",
	    "discount":"$0.00",
	    "gross_amount":"$6,250.00"
	  }
    ],
    "subtotal":"$6,250.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$6,250.00",
    "payments":[],
    "tags":"rockets,acme",
    "notes":"",
    "state":"outstanding",
    "url":"http://localhost:3000/assur-219/api/v1/expenses/5076a6b92f412e0e2e00006c"
  },
  {
    "id":"5076a6b92f412e0e2e00016d",
    "number":"00211",
    "issue_date":"2012-10-11",
    "contact":{
	  "id":"5059bdbf2f412e0901000024",
	  "full_name":"ACME"
    },
    "po_number":"",
    "currency":"USD",
    "items":[
      {
	    "description":"TNT",
	    "quantity":"100.0",
	    "unit_price":"25.0",
	    "discount_rate":"0.0",
	    "tax_1_name":"",
	    "tax_1_rate":"",
	    "tax_2_name":"",
	    "tax_2_rate":"",
	    "subtotal":"$2,500.00",
	    "discount":"$0.00",
	    "gross_amount":"$2,500.00"
	  }
    ],
    "subtotal":"$2,500.00",
    "discount":"$0.00",
    "taxes":[],
    "total":"$2,500.00",
    "payments":[],
    "tags":"tnt,acme",
    "notes":"",
    "state":"outstanding",
    "url":"http://localhost:3000/assur-219/api/v1/expenses/5076a6b92f412e0e2e00016d"
  },

]
```
## Create expense
`POST /expenses.json` will create a new expense from the parameters passed.
```json
{     
	"contact_id":"5059bdbf2f412e0901000024",
	"contact_name":"ACME",
	"currency":"USD",
	"items":[
	  {
		"description":"Rocket launcher",
		"quantity":"1.0",
		"unit_price":"0.0",
		"discount_rate":"0.0",
	  }
	],
}
```
This will return `201 Created`, with the location of the new expense in the Location header along with the current JSON representation of the expense if the creation was a success.  If the user does not have access to create new expenses, you'll see `401 Unauthorized`.

## Update expense
`PUT /expenses/1.json` will update the expense from the parameters passed.
```json
{
	"po_number":"",
	"tags":"rocket, launcher"
	"payment_details":"Money in da bank",
	"notes":""
} 
```

This will return `200 OK` if the update was a success along with the current JSON representation of the expense. If the user does not have access to update the expense, you'll see `401 Unauthorized`.

## Delete expense
`DELETE /expenses/1.json` will delete the expense specified and return `204 No Content` if that was successful. If the user does not have access to delete the expense, you'll see `401 Unauthorized`.