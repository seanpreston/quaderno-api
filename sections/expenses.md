# Expenses
Expenses are all the invoices that you receive from your vendors.

## Get expenses
`GET /expenses.json` will return all your expenses.
```json
```
## Create expense
`POST /expenses.json` will create a new expense from the parameters passed.
```json
```
This will return `201 Created`, with the location of the new expense in the Location header along with the current JSON representation of the expense if the creation was a success.  If the user does not have access to create new expenses, you'll see `403 Forbidden`.

## Update expense
`PUT /expenses/1.json` will update the expense from the parameters passed.
```json
```

This will return `200 OK` if the update was a success along with the current JSON representation of the expense. If the user does not have access to update the expense, you'll see `403 Forbidden`.

## Delete expense
`DELETE /expenses/1.json` will delete the expense specified and return `204 No Content` if that was successful. If the user does not have access to delete the expense, you'll see `403 Forbidden`.