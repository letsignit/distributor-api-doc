# Letsignit API distributor

## Getting started
Before using the Letsignit API distributors you must contact your Letsignit representative, to receive access.

**Production HOST**: api.letsignit.com

With the API Distributor you can create, modify or delete a subscription on our plateform.

Before making these requests, you must authenticate yourself using the credentials given by your Letsignit representative.


## Authenticate
* Http method: POST
* Url: https://**HOST**/auth/login
* Content-Type: application/json
* Body:
```
{
    "app_id":"<your_app_id>",
    "api_key":"<your_api_key>"
}
```
**app_id** (string): The application ID

**api_key** (string): The API key

### Authentication successful

* Status code 200
* Content-Type: application/json
* Body: 
```
{
    "access_token": "token_value",
    "token_type": "Bearer",
    "expires_in": 86400
}
```

**access_token** (string): The access token to use for the provisioning requests

**token_type** (string): The token type 'Bearer'

**expires_in** (int): The token lifetime in seconds (24 hours)

### Authentication failed

* Status code 401
* Content-Type: application/json
* Body: 
```
{
    "message": "Invalid Credentials"
}
```

**message** (string): A message explaining the authentication fail

## Create a subscription
* Http method: POST
* Url: https://**HOST**/distributorsapi/v1/subscriptions
* Content-Type: application/json
* Body:
```
{
    "id": "subscription_id",
    "cluster": "fr",
    "distributor": {
        ...
    },
    "product": {
        ...
    },
    "plan": {
        ...
    },
    "quantity": 231,
    "owner": {
        ...
    },
    "customer": {
        ...
    },
    "suspend": false
}
```

**id** (string): Your internal identifier representing the subscription. Must be unique.

**cluster** (string): The cluster where you want to store the data. values can be: eu, use-new, ca-new, fr.

**Warning: we are still working on this, cluster FR is not FR only, its a replicated worldwide.**

Value *eu* is for Europe, *use-new* is for the United States, *ca-new* is for Canada, and *fr* is for others countries not available yet.

**distributor** (object): The distributor object is explained below

**product** (object): The product object is explained below

**plan** (object): The plan object is explained below

**quantity** (int): The quantity of licences

**owner** (object) : The owner object is explained below

**customer** (object): The customer object is explained below

**suspend** (boolean): Determines if the subscription is suspended or not.


### **Distributor**
```
 "distributor": {
    "id": "distributor_id",
    "name": "Software distribution Corp. USA",
    "email": "purchase@distributor.com"
}
```
**id** (string): Your internal identifier representing the distributor. Must be unique for each distributor.

**name** (string): Distributor's name, use an explicit name

**email** (string): Distributor's email, used for support purposes

### **Product**

```
"product": {
    "id": "product_id",
    "name": "O365 - Letsignit Bundle",
    "is_bundle": true,
    "bundle_id":"bundle_id"
}
```
**id** (string): Your internal identifier representing the product/SKU. Must be unique for each product.

**name** (string): Your product's/SKU name

**is_bundle** (boolean): Determine if the current product/SKU is a bundle or not.

**bundle_id** (string): Required only if is_bundle is set to true.

### **Plan**
```
"plan": {
    "id": "plan_id",
    "name": "Awesome Letsignit Business Monthly offer",
    "letsignit_plan_id": "full",
    "interval": "monthly",
    "is_nfr": true
}
```
**id** (string): Your internal identifier representing the plan. Must be unique for each plan.

**name** (string): Your plan's name, use an explicit name like 'Letsignit full', or 'Letsignit basic'

**letsignit_plan_id** (string): Letsignit identifier for plan, must be 'full' or 'basic'

**interval** (string): The interval for the billing, only two values authorized: 'monthly', 'annually'

**is_nfr** (boolean): Determines if current provisioning is a NFR (not for resel). In this case Owner information and Customer information should be the same. (the customer is the owner)

### **Owner**
The owner of the customer subscription is usually the reseller
```
"owner": {
    "id": "reseller_id",
    "admin_name": "John Smith",
    "company_name": "Resellers Corp.",
    "website": "www.reseller.com",
    "email": "purchase@reseller.com"
}
```
**id** (string): Your internal identifier representing the owner. Must be unique for each owner.

**admin_name** (string): The subscription owner name, that will be shown for the customer.

**company_name** (string): The subscription owner company name, that will be shown for the customer.

**website** (string): The subscription owner website, that will be shown for the customer.

**email** (string): The subscription owner email used by Letsignit in case of support.

### **Customer**
```
"customer": {
    "id": "customer_id",
    "company_name": "ACME Corp",
    "language": "en",
    "admin_email": "admin@customer.com",
}
```
**id** (string): Your internal identifier representing the customer. Must be unique for each customer.

**company_name** (string): The customer company name.

**language** (string): The language of the company, (support: fr, en)

**admin_email** (string): Customer email. That will be used to create the customer and the first administrator. Must be unique, an email cannot be used twice, otherwise the purchase will fail.

### Create successful

* Status code 201
* Content-Type: application/json
* Body: 
```
{
    "client_id": "5ed60d2e7b9f99000799d539",
    "id": "subscription_id"
}
```
**client_id**: The identifier representing the client created on our plateform

**id**: Your internal identifier used for the provisioning

### Create failed

You have two types of errors returned when failed, one about the model of the body and the other is about the values used.

* Model error
* Status code 400
* Content-Type: application/json
* Body: 
```
{
    "errors": {
        "plan": "'plan' is a required property"
    },
    "message": "Input payload validation failed"
}
```
**errors**: The list of errors encoutered during the provisioning

**message**: A global message explaining the error 

* Value error
* Status code 400
* Content-Type: application/json
* Body:  
```
{
    "code": "0002",
    "context": "application.subscription.errors",
    "message": "Email is not available."
}
```
**code**: A code used to identify the error

**context**: The context of the error, in your case should always be "application.subscription.errors" 

**message**: A message explaining the error  

## Update a subscription
* Http method: PUT
* Url: https://**HOST**/distributorsapi/v1/subscriptions/**SUBSCRIPTION_ID**
* Request Params Subscription_ID: the identifier of the subscription used for the creation.
* Content-Type: application/json
* Body:
```
{
    "id": "subscription_id",
    "cluster": "fr",
    "distributor": {
        ...
    },
    "product": {
        ...
    },
    "plan": {
        ...
    },
    "quantity": 231,
    "owner": {
        ...
    },
    "customer": {
        ...
    },
    "suspend": false
}
```
To modify a subscription, you must use the same body payload you used for the creation, including the modification.

### Update successful

* Status code 200
* Content-Type: application/json
* Body:  
```
{
    "client_id": "5ed60d2e7b9f99000799d539",
    "id": "subscription_id"
}
```
**client_id**: The identifier representing the client updated on our plateform

**id**: Your internal identifier used for the provisioning

### Update failed

You have two types of errors returned when failed, one about the body payload and the other is about the values used.

Model error:
* Status code 400
* Content-Type: application/json
* Body:  
```
{
    "errors": {
        "plan": "'plan' is a required property"
    },
    "message": "Input payload validation failed"
}
```
**errors**: The list of errors encoutered during the provisioning

**message**: A global message explaining the error 

Value error:
* Status code 400
* Content-Type: application/json
* Body:  
```
{
    "code": "0002",
    "context": "application.subscription.errors",
    "message": "Email is not available."
}
```
**code**: A code use to identify the error

**context**: The context of the error, in your case should always be "application.subscription.errors" 

**message**: A message explaining the error 

## Cancel a subscription
* Http method: DELETE
* Url: https://**HOST**/distributorsapi/v1/subscriptions/**SUBSCRIPTION_ID**
* Request Params Subscription_ID: the identifier of the subscription used for the creation.