## Transactions flow

In order to meet VAT MOSS compliance you can use the Quaderno transactions flow.

With the Quaderno transactions flow you can make live taxes calculations in your website and check if the customer data meets the VAT compliance before completing the order. 
If you are not using the [quaderno.js for Stripe](https://github.com/quaderno/quaderno.js), you probably want to do most of it via ajax, so in this case please note that as it executes your code on the client side it will expose your API token. **We recommend using mechanisms to hide it such as proxy pages**.

Here is a practical example: 

Suppose you manage an e-books store. A customer has just finished adding products to the shopping cart and now he/she wants to checkout.

### Step 1: Calculate taxes
While your customer is filling his data (card number, name, etc.) in your website form you can make live taxes calculations by calling the taxes api calculate method (see the taxes section to get more info). 

GET `https://quadernoapp.com/api/v1/taxes/calculate.json?token=pk_test_YourStr1pePubl1sh4bleKey&country=UK&consumer=true`

This call will return a json object with the tax you may apply to this customer.

### Step 2: Create the transaction object
So, the customer, after finishing filling his data is going to press the "Pay button".
Before completing the order you must create a transaction object. A sample call would look like this;

POST `https://quadernoapp.com/api/v1/transactions.json`

 [create transaction section](https://github.com/quaderno/quaderno-api/blob/master/sections/transactions.md#create-transactions). If all the required transaction fields are present there are two possible scenarios:

* **Scenario 1:** The customer has incoherent data so it doesn't meet the VAT compliance. In this case the transaction is not saved because Quaderno cannot verify the right tax for this customer. We do not recommend proceeding with the order as you may apply wrong taxes.
* **Scenario 2:** The customer has at least two coincident data for the country (i.e. the country of the request ip  and the country of the zip code are the same). In this case the customer's data meets the VAT compliance so Quaderno will save the transaction as a history record. Now you can proceed to the step 3.

### Step 3: Process and complete the order
Now that everything is verified and the information is stored, you can complete the order and create the payment on your website (i.e. debit the customer’s payment method).
