# 5. Vault Code Examples and Explanation

To open the **data** vault, we use the `open_vault` function.

```
with open_vault('data') as vault: 
    # Get, insert, update, upsert, or delete vault data
```

### Get All Customers

```
with open_vault('data') as vault:
    customers = vault.get('customers')

    if customers:
        output.payload = customers
    else:
        output.payload = "There are no customers in the data vault."
```

To retrieve the `customers` list in the **data** vault, we use the vault method `get.` Afterwards, we check if the `customers` list is empty. If it is not empty, we assign `output.payload` the `customers` list. Otherwise, we assign `output.payload` a confirmation message that there are no customers in the list.

### Get Customer By Id

```
with open_vault('data') as vault:
    customers = vault.get('customers')
    customers = [customer for customer in customers if customer.id == input.path.id]

    if customers:
        output.payload = customers[0]
    else:
        output.payload = f'There are no customers with an id of: {input.path.id}'
```

To get a customer by Id in the **data** vault, we first use the vault method `get` to retrieve the `customers` list. Afterwards, we use a list comprehension to extract the customer from the `customers` list that has an Id that matches the `id` in `input.path`. At the end, we assign `output.payload` the customer's information in the form of a dictionary object or an error message if the customer was found.&#x20;

### Add a Customer

```
with open_vault('data') as vault:
    customers = vault.get('customers')
    customers.append(insert_customer.response.payload)
    vault.update('customers', customers)
    output.payload = { "id": insert_customer.response.payload.id }
```

To add a customer in the **data** vault, we first use the vault method `get` to retrieve the `customers` list. Afterwards, we append `response.payload` of `insert_customer` to the `customers` list and use the vault method `update` to save our changes to the `customers` list. At the end, we assign `output.payload` the Id of the new customer.&#x20;

### Update Customer

```
with open_vault('data') as vault:
    customers = vault.get('customers')
    customer = [customer for customer in customers if customer.id == input.path.id]

    if customer:
        customer[0].update(modify_customer.response.payload)
        vault.update('customers', customers)
        output.payload = customer[0]
    else:
        output.payload = f'There are no customers with an id of: {input.path.id}'
```

To update a customer in the **data** vault, we first use the vault method `get` to retrieve the `customers` list. Afterwards, we search for the customer via their Id. If the customer is found, we update the customer's information from `response.payload` of the service request `modify_customer`. At the end, we assign `output.payload` the customer's information in the form of dictionary object or an error message if the customer was not found.

### Delete Customer

```
with open_vault('data') as vault:
    customers = vault.get('customers')
    previous_length = len(customers)
    customers = [customer for customer in customers if customer.id != input.path.id]

    if previous_length != len(customers):
        vault.update('customers', customers)
        output.payload = f"The customer with an id of {input.path.id} has been deleted."
    else:
        output.payload = f'There are no customers with an id of: {input.path.id}'
```

To delete a customer in the **data** vault, we first use the vault method `get` to retrieve the `customers` list. Afterwards, we record the current length of the customers list and we use a list comprehension to remove the customer from the `customers` list that has an Id that matches the `id` in `input.path`. If the length of the `customers` list has changed, we use the vault method `update` to save our changes to the `customers` list. At the end, we assign `output.payload` a confirmation message that the customer was deleted or an error message if the customer was not found.
