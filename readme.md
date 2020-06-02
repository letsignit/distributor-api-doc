# Letsignit distributor API

## Getting started
Before using the Letsignit distributors API you must contact your Letsignit contact, to receive an access.

**Production HOST** : api.letsignit.com

With the API Distributor you can create, modify or delete a subscription on our plateform.

Before making these requests you must authenticate yourself using the credentials given by your Letsignit contact.


## Authenticate
* Http method : POST
* Url : https://**HOST**/auth/login
* Body payload :
```
{
    "app_id":"<your_app_id>",
    "api_key":"<your_api_key>"
}
```
**app_id** (string) : The application ID

**api_key** (string) : The API key

## Create a subscription
* Http method : POST
* Url : https://**HOST**/distributorsapi/v1/subscriptions
* Body payload :
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
    }
}
```

**id** (string) : Your internal identifier representing the subscription. Must be unique.

**cluster** (string) : The cluster where you want to store the data. values can be : eu, use-new, ca-new, fr.

**Warning : we are still working on this, cluster FR is not FR only, its a replicated worldwide.**

Value *eu* is for Europe, *use-new* is for the United States, *ca-new* is for Canada, and *fr* is for others country not available yet.

**distributor** (object) : The distributor object is explained below

**product** (object) : The product object is explained below

**plan** (object) : The plan object is explained below

**quantity** (int) : The quantity of licences

**customer** (object) : The customer object is explained below


### **Distributor**
```
 "distributor": {
    "id": "distributor_id",
    "name": "Software distribution Corp. USA",
    "email": "purchase@distributor.com"
}
```
**id** (string) : Your internal identifier representing the distributor. Must be unique for each distributor.

**name** (string) : Distributor's name, use an explicit name

**email** (string) : Distributor's email, used for support purpose

### **Product**

```
"product": {
    "id": "product_id",
    "name": "O365 - Letsignit Bundle",
    "is_bundle": true,
    "bundle_id":"bundle_id"
}
```
**id** (string) : Your internal identifier representing the product/SKU. Must be unique for each product.

**name** (string) : Your product's/SKU name

**is_bundle** (boolean) : Determine if the current product/SKU is a bundle or not.

**bundle_id** (string) : Required only if is_bundle set to true.

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
**id** (string) : Your internal identifier representing the plan. Must be unique for each plan.

**name** (string) : Your plan's name, use an explicit name like 'Letsignit full', or 'Letsignit basic'

**letsignit_plan_id** (string) : Letsignit identifier for plan, must be 'full' or 'basic'

**interval** (string) : The interval for the billing, only two values authorized : 'monthly', 'annually'

**is_nfr** (boolean) : Determines if current provisioning is a NFR (not for resel). In this case Owner information and Customer information should be the same. (the customer is the owner)

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
**id** (string) : Your internal identifier representing the owner. Must be unique for each owner.

**admin_name** (string) : The subscription owner name, that will be shown for the customer.

**company_name** (string) : The subscription owner company name, that will be shown for the customer.

**website** (string) : The subscription owner website, that will be shown for the customer.

**email** (string) : The subscription owner email used by Letsignit in case of support.

### **Customer**
```
"customer": {
    "id": "customer_id",
    "company_name": "ACME Corp",
    "language": "en",
    "admin_email": "admin@customer.com",
}
```
**id** (string) : Your internal identifier representing the customer. Must be unique for each customer.

**company_name** (string) : The customer company name.

**language** (string) : The language of the company, (support : fr, en, es, nl)

**admin_email** (string) : Customer email. That will be used to create the customer and the first administrator. Must be unique, an email canot be used twice, otherwise the purchase will fail.

## Modify a subscription
* Http method : PUT
* Url : https://**HOST**/distributorsapi/v1/subscriptions/**SUBSCRIPTION_ID**
* Request Params Subscription_ID : the identifier of the subscription used for the creation.
* Body payload :
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
    }
}
```
To modify a subscription you have to use the same payload you used for the creation, including the modification.

## Cancel a subscription
* Http method : DELETE
* Url : https://**HOST**/distributorsapi/v1/subscriptions/**SUBSCRIPTION_ID**
* Request Params Subscription_ID : the identifier of the subscription used for the creation.