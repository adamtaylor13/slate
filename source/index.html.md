---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - http
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---


# Getting Started

Welcome to the Lunch Money developer API! We created this to enable the user and the community to build rich plug-ins to complement their Lunch Money experience.

---

## Current Status

The developer API is officially in open public beta. During this time, please continue to heed caution and use this API at your own risk as any and all changes are irreversible.

We welcome feedback via email (support@lunchmoney.app). These docs are also on [Github](https://github.com/adamtaylor13/slate), so if you see a mistake or something that could be improved, feel free to open a pull request!

## Connecting to the Lunch Money API

### Connect to the server

The Lunch Money API endpoint is: `https://dev.lunchmoney.app`

<aside class="notice">
For POST requests, ensure you set Content-Type to application/json
</aside>


## Authentication

> Use Bearer Tokens in your requests like this:

```http
GET /v1/categories HTTP/1.1
Host: dev.lunchmoney.app
Authorization: Bearer YOURTOKEN
```

```ruby
require "uri"
require "net/http"

url = URI("https://dev.lunchmoney.app/v1/categories")

https = Net::HTTP.new(url.host, url.port)
https.use_ssl = true

request = Net::HTTP::Get.new(url)
request["Authorization"] = "Bearer YOURTOKEN"

response = https.request(request)
puts response.read_body
```

```python
import requests

url = "https://dev.lunchmoney.app/v1/categories"

payload={}
headers = {
    'Authorization': 'Bearer YOURTOKEN'
}

response = requests.request("GET", url, headers=headers, data=payload)

print(response.text)
```

```shell
  curl --location --request GET 'https://dev.lunchmoney.app/v1/categories' \
       --header 'Authorization: Bearer YOURTOKEN'
```

```javascript
var myHeaders = new Headers();
myHeaders.append("Authorization", "Bearer YOURTOKEN");

var requestOptions = {
    method: 'GET',
    headers: myHeaders,
    redirect: 'follow'
};

fetch("https://dev.lunchmoney.app/v1/categories", requestOptions)
    .then(response => response.text())
    .then(result => console.log(result))
    .catch(error => console.log('error', error));
```

Lunch Money API requests are authenticated using the Bearer Token authentication method.

### Getting an access token

Get your access token by going to this page [in the app](https://my.lunchmoney.app/developers).

## What should I build?

**Great question!** We’ve tried to expose the minimum endpoints needed to enable you to build powerful products and extensions.

Here are a few ideas of what you can build:

### Basic use cases

**Integration with your bank**

Does your bank offer an API? You can build a bridge between Lunch Money and your bank to import transactions automatically.

**Sync Lunch Money data to your personal interface**

If you have your own spreadsheet or other interface, use the API to sync data for personalized viewing and analytics.

### Specific use cases with high demand:

**Amazon receipt matcher**

Do you make a lot of Amazon purchases? I'm sure by now you know how frustrating it is to try to identify exactly what the expense is for! Build an Amazon receipt matcher that pulls in your Amazon purchase history and matches on transactions in Lunch Money and updates the notes.

<aside class="notice">
Note: apparently Amazon’s API doesn't expose consumer transactions, so to achieve this, you may need to employ another method of getting transaction details (ideas: email forwarder, chrome extension/screen scraper…)
</aside>

**Venmo integration via email**

The lack of a Venmo and Plaid integration is frustrating for many of our users and is largely out of our control.  Since Venmo sends an email notification every time you send or receive money, build a service that enables users to forward their Venmo notification emails for parsing and insertion into Lunch Money!

**Companion mobile app**

Build a simple mobile app that allows quick insertion of transactions into your Lunch Money account or quick reviewing.

**Zillow integration**

Create an integration that automatically updates the value of a real estate property in Lunch Money.

---

# Awesome Projects
This is a list of awesome open-sourced projects created by the Lunch Money community!

Description                                                        | Made by                                             | Github Link
-----------                                                        | -------                                             | -----------
JavaScript API client with TypeScript support                      | [@joehoyle](https://twitter.com/joe_hoyle)          | [Link](https://github.com/lunch-money/lunch-money-js)
Sync Monzo transactions to Lunch Money automatically               | [@joehoyle](https://twitter.com/joe_hoyle)          | [Link](https://github.com/joehoyle/monzo-to-lunch-money)
Sync bunq transactions to Lunch Money automatically                | [@markjongkind](https://twitter.com/markjongkind)   | [Link](https://github.com/markjongkind/bunq-to-lunchmoney)
Sync your Delta cryptocurrency portfolio balance to Lunch Money    | [@markjongkind](https://twitter.com/markjongkind)   | [Link](https://github.com/markjongkind/delta-to-lunchmoney)
Lunch Money React Native app (WIP)                                 | [@yuanhaochiang](https://twitter.com/yuanhaochiang) | [Link](https://github.com/yuanworks/bento-money)
Match Amazon transactions and set transaction notes in Lunch Money | [@samwelnella](https://github.com/samwelnella)      | [Link](https://github.com/samwelnella/amazon-transactions-to-lunchmoney)
Go API Client                                                      | [@icco](https://twitter.com/icco)                   | [Link](https://github.com/icco/lunchmoney)

<aside class="notice">
Did you create something with the Lunch Money API? Let us know and we'll add it to this list!
</aside>

---

# Changelog
A log of changes. Breaking changes will be denoted with 🚨
## March 28, 2020

### New
Pagination options for [GET /v1/transactions](#get-all-transactions) (limit and offset)
Filter options for [GET /v1/transactions](#get-all-transactions) (asset_id, recurring_id, plaid_account_id, tag_id, category_id)
New endpoint: [GET /v1/tags](#get-all-tags)

### Changed
* Support for tags in [PUT /v1/transactions/:id](#update-transaction) and [POST /v1/transactions](#insert-transactions)
* 🚨Split object for [splitting transactions](#update-transaction) is moved out of the Transactions object and to a higher-level. We will still support the split property for a few more weeks before removing it completely.

---

# FAQ

## The endpoint I need is not listed. Can you add it?

If you would like a specific endpoint which is not currently supported, please let us know by emailing [support@lunchmoney.app](mailto:support@lunchmoney.app) and stating your use case.

## What currencies are supported?

Check [here](#supported-currencies) for a list of all the currencies we currently support. If a currency is missing, let us know via email and we’ll try to get it added!

## I’ve built something! Now what?

Awesome! Please share with us via email about what you built! If you are willing to share your code, we encourage you to open-source your tool/plug-in so others in the Lunch Money community can benefit and we'll add it to our [Awesome Projects](#awesome-projects) page.

---

# Transactions

## Transaction Object
Attribute Name   | Type    | Description
---------------- | ------- | -----------
id               | number  | Unique identifier for transaction
date             | string  | Date of transaction in ISO 8601 format
payee            | string  | Name of payee If recurring_id is not null, this field will show the payee of associated recurring expense instead of the original transaction payee
amount           | string  | Amount of the transaction in numeric format to 4 decimal places
currency         | string  | Three-letter lowercase currency code of the transaction
notes            | string  | User-entered transaction notes If recurring_id is not null, this field will be description of associated recurring expense
category_id      | number  | Unique identifier of associated category (see Categories)
asset_id         | number  | Unique identifier of associated manually-managed account (see Assets) Note: plaid_account_id and asset_id cannot both exist for a transaction
plaid_account_id | number  | Unique identifier of associated Plaid account (see Plaid Accounts) Note: plaid_account_id and asset_id cannot both exist for a transaction
status           | string  | One of the following: cleared: User has reviewed the transaction uncleared: User has not yet reviewed the transaction recurring: Transaction is linked to a recurring expense recurring_suggested: Transaction is listed as a suggested transaction for an existing recurring expense. User intervention is required to change this to recurring.
parent_id        | number  | Exists if this is a split transaction. Denotes the transaction ID of the original transaction. Note that the parent transaction is not returned in this call.
is_group         | boolean | True if this transaction represents a group of transactions. If so, amount and currency represent the totalled amount of transactions bearing this transaction’s id as their group_id. Amount is calculated based on the user’s primary currency.
group_id         | number  | Exists if this transaction is part of a group. Denotes the parent’s transaction ID
tags             | Tag[]   | Array of Tag objects
external_id      | string  | User-defined external ID for any manually-entered or imported transaction. External ID cannot be accessed or changed for Plaid-imported transactions. External ID must be unique by asset_id. Max 75 characters.

## Get All Transactions

Use this endpoint to retrieve all transactions between a date range.

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response

```json
{
  "transactions": [
    {
      "id": 602,
      "date": "2020-01-01",
      "payee": "Starbucks",
      "amount": "4.5000",
      "currency": "cad",
      "notes": "Frappuccino",
      "category_id": null,
      "recurring_id": null,
      "asset_id": null,
      "plaid_account_id": null,
      "status": "cleared",
      "is_group": false,
      "group_id": null,
      "parent_id": null,
      "external_id": null
    },
    {
      "id": 603,
      "date": "2020-01-02",
      "payee": "Walmart",
      "amount": "20.9100",
      "currency": "usd",
      "notes": null,
      "category_id": null,
      "recurring_id": null,
      "asset_id": 153,
      "plaid_account_id": null,
      "status": "uncleared",
      "is_group": false,
      "group_id": null,
      "parent_id": null,
      "external_id": "jf2r3t98o943"
    }
  ]
}
```

> Example 404 Response

```json
{ "error": "Both start_date and end_date must be specified." }
```

Returns list of Transaction objects. If no query parameters are set, this endpoint will return transactions for the current calendar month (see start_date and end_date)

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions`

## Get Single Transaction

Use this endpoint to retrieve details about a specific transaction by ID.

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response

```json
{
    "id": 31,
    "date": "2019-02-04",
    "payee": "Shell",
    "amount": "960.0000",
    "currency": "jpy",
    "notes": null,
    "category_id": 22,
    "recurring_id": null,
    "asset_id": null,
    "plaid_account_id": null,
    "status": "cleared",
    "is_group": false,
    "group_id": null,
    "parent_id": null,
    "has_children": null,
    "external_id": null
}
```

> Example 404 Response

```json
{ "error": "Transaction ID not found." }
```

Returns a single Transaction object

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions/:transaction_id`

### Query Parameters

Parameter         | Type    | Required | Default | Description
---------         | ----    | -------- | ------- | -----------
debit_as_negative | boolean | false    |  false  | Filter by tag. Only accepts IDs, not names.

## Insert Transactions

Use this endpoint to insert many transactions at once.

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response
>
> Upon success, IDs of inserted transactions will be returned in an array.

```json
{
    "ids": [54, 55, 56, 57]
}
```

> Example 404 Response
>
> An array of errors will be returned denoting reason why parameters were deemed invalid.

```json
{ "error":
   [ "Transaction 0 is missing date.",
     "Transaction 0 is missing amount.",
     "Transaction 1 status must be either cleared or uncleared: null" ] }
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/transactions`

### Query Parameters
Parameter           | Type    | Required | Default | Description
---------           | ----    | -------- | ------- | -----------
transactions        | array   | true     | -       | List of transactions to insert (see below)
apply_rules         | boolean | false    | false   | If true, will apply account’s existing rules to the inserted transactions. Defaults to false.
check_for_recurring | boolean | false    | false   | If true, will check new transactions for occurrences of new monthly expenses. Defaults to false.
debit_as_negative   | boolean | false    | false   | If true, will assume negative amount values denote expenses and positive amount values denote credits. Defaults to false.


### Transaction Object to Insert
Key          | Type                            | Required | Description
---          | ----                            | -------- | -----------
date         | string                          | true     | Must be in ISO 8601 format (YYYY-MM-DD).
amount       | number/string                   | true     | Numeric value of amount. i.e. $4.25 should be denoted as 4.25.
category_id  | number                          | false    | Unique identifier for associated category_id. Category must be associated with the same account and must not be a category group.
payee        | string                          | false    | Max 140 characters
currency     | string                          | false    | Three-letter lowercase currency code must exist in our database. Defaults to user account's primary currency.
asset_id     | number                          | false    | Unique identifier for associated asset (manually-managed account). Asset must be associated with the same account.
recurring_id | number                          | false    | Unique identifier for associated recurring expense. Recurring expense must be associated with the same account.
notes        | string                          | false    | Max 350 characters
status       | string                          | false    | Must be either cleared or uncleared. If recurring_id is provided, the status will automatically be set to recurring or recurring_suggested depending on the type of recurring_id.  Defaults to uncleared.
external_id  | string                          | false    | User-defined external ID for transaction. Max 75 characters. External IDs must be unique within the same asset_id.
tags         | Array of numbers and/or strings | false    | Passing in a number will attempt to match by ID. If no matching tag ID is found, an error will be thrown. Passing in a string will attempt to match by string. If no matching tag name is found, a new tag will be created.

## Update Transaction

Use this endpoint to update a single transaction. You may also use this to split an existing transaction.

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response
>
> Upon success, IDs of inserted transactions will be returned in an array.

```json
{
    "updated": true,
    "split": [58, 59]
}
```

> Example 404 Response
>
> An array of errors will be returned denoting reason why parameters were deemed invalid.

```json
{ "error":
   [ "This transaction doesn't exist or you don't have access to it." ] }
```
```json
{ "error":
   [ "You cannot change the amount for this transaction because it was automatically imported.",
     "You cannot assign an asset_id for this transaction because it was automatically imported." ] }
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/transactions/:transaction_id`

### Body Parameters
Parameter         | Type    | Required | Default | Description
---------         | ----    | -------- | ------- | -----------
split             | object  | false    | -       | Defines the split of a transaction. You may not split an already-split transaction, recurring transaction, or group transaction. (see Split object below)
transaction       | object  | true     | -       | Updates to transaction matching ID (see Update Transaction object below)
debit_as_negative | boolean | false    | false   | If true, will assume negative amount values denote expenses and positive amount values denote credits. Defaults to false.

### Update Transaction Object
Key          | Type                            | Description
---          | ----                            | -----------
date         | string                          | Must be in ISO 8601 format (YYYY-MM-DD).
category_id  | number                          | Unique identifier for associated category_id. Category must be associated with the same account and must not be a category group.
payee        | string                          | Max 140 characters
amount       | number or  string               | You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id
currency     | string                          | You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id. Defaults to user account's primary currency.
asset_id     | number                          | Unique identifier for associated asset (manually-managed account). Asset must be associated with the same account. You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id
recurring_id | number                          | Unique identifier for associated recurring expense. Recurring expense must be associated with the same account.
notes        | string                          | Max 350 characters
status       | string                          | Must be either cleared or uncleared. Defaults to uncleared If recurring_id is provided, the status will automatically be set to recurring or recurring_suggested depending on the type of recurring_id. Defaults to uncleared.
external_id  | string                          | User-defined external ID for transaction. Max 75 characters. External IDs must be unique within the same asset_id. You may only update this if this transaction was not created from an automatic import, i.e. if this transaction is not associated with a plaid_account_id
tags         | array of numbers and/or strings | Passing in a number will attempt to match by ID. If no matching tag ID is found, an error will be thrown.  Passing in a string will attempt to match by string. If no matching tag name is found, a new tag will be created.


### Split Object
Key         | Type          | Required | Description
---         | ----          | -------- | -----------
date        | string        | true     | Must be in ISO 8601 format (YYYY-MM-DD).
category_id | number        | true     | Unique identifier for associated category_id. Category must be associated with the same account.
notes       | string        | false    |
amount      | number/string | true     | Individual amount of split. Currency will inherit from parent transaction. All amounts must sum up to parent transaction amount.

---

# Recurring Expenses

## Recurring Expenses Object
Attribute        | Type   | Description
---------        | ----   | -----------
id               | number | Unique identifier for recurring expense
start_date       | string | Denotes when recurring expense starts occurring in ISO 8601 format. If null, then this recurring expense will show up for all time before end_date
end_date         | string | Denotes when recurring expense stops occurring in ISO 8601 format. If null, then this recurring expense has no set end date and will show up for all months after start_date
cadence          | string | One of:<br> <ul> <li>monthly</li> <li>twice a month</li> <li>once a week</li> <li>every 3 months</li> <li>every 3 months</li> <li>every 4 months</li> <li>twice a year</li> <li>yearly</li></ul>
payee            | string | Payee of the recurring expense
amount           | number | Amount of the recurring expense in numeric format to 4 decimal places
currency         | string | Three-letter lowercase currency code for the recurring expense
description      | string | If any, represents the user-entered description of the recurring expense
billing_date     | string | Expected billing date for this recurring expense for this month in ISO 8601 format
type             | string | This can be one of two values: <br><ul> <li>cleared: The recurring expense has been reviewed by the user</li> <li>suggested: The recurring expense is suggested by the system; the user has yet to review/clear it</li> </ul>
original_name    | string | If any, represents the original name of the recurring expense as denoted by the transaction that triggered its creation
source           | string | This can be one of three values:<br> <ul> <li>manual: User created this recurring expense manually from the Recurring Expenses page</li> <li>transaction: User created this by converting a transaction from the Transactions page</li> <li>system: Recurring expense was created by the system on transaction import</li> <li>Some older recurring expenses may not have a source.</li> </ul>
plaid_account_id | number | If any, denotes the plaid account associated with the creation of this recurring expense (see Plaid Accounts)
asset_id         | number | If any, denotes the manually-managed account (i.e. asset) associated with the creation of this recurring expense (see Assets)
transaction_id   | number | If any, denotes the unique identifier for the associated transaction matching this recurring expense for the current time period
category_id      | number | If any, denotes the unique identifier for the associated category to this recurring expense

## Get Recurring Expenses

Use this endpoint to retrieve a list of recurring expenses to expect for a specified period.

Every month, a different set of recurring expenses is expected. This is because recurring expenses can be once a year, twice a year, every 4 months, etc.

If a recurring expense is listed as “twice a month”, then that recurring expense will be returned twice, each with a different billing date based on when the system believes that recurring expense transaction is to be expected. If the recurring expense is listed as “once a week”, then that recurring expense will be returned in this list as many times as there are weeks for the specified month.

In the same vein, if a recurring expense that began last month is set to “Every 3 months”, then that recurring expense will not show up in the results for this month.

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response
>
> Returns a list of Recurring Expense objects


```json
{
  "recurring_expenses": [
    {
      "id": 264,
      "start_date": "2020-01-01",
      "end_date": null,
      "cadence": "twice a month",
      "payee": "Test 5",
      "amount": "-122.0000",
      "currency": "cad",
      "created_at": "2020-01-30T07:58:43.944Z",
      "description": null,
      "billing_date": "2020-01-01",
      "type": "cleared",
      "original_name": null,
      "source": "manual",
      "plaid_account_id": null,
      "asset_id": null,
      "transaction_id": null
    },
    {
      "id": 262,
      "start_date": "2020-01-01",
      "end_date": null,
      "cadence": "monthly",
      "payee": "Test 2",
      "amount": "-32.4500",
      "currency": "usd",
      "created_at": "2020-01-30T07:58:43.921Z",
      "description": "Test description 2",
      "billing_date": "2020-01-03",
      "type": "cleared",
      "original_name": null,
      "source": "manual",
      "plaid_account_id": null,
      "asset_id": null,
      "transaction_id": null
    },
    {
      "id": 264,
      "start_date": "2020-01-01",
      "end_date": null,
      "cadence": "twice a month",
      "payee": "Test 5",
      "amount": "-122.0000",
      "currency": "cad",
      "created_at": "2020-01-30T07:58:43.944Z",
      "description": null,
      "billing_date": "2020-01-15",
      "type": "cleared",
      "original_name": null,
      "source": "manual",
      "plaid_account_id": null,
      "asset_id": null,
      "transaction_id": null
    }
  ]
}
```

> Example 404 Response

```json
{ "error": "Invalid start_date. Must be in format YYYY-MM-DD" }
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/recurring_expenses`

Parameter         | Type    | Required | Default                        | Description
---------         | ----    | -------- | -------                        | -----------
start_date        | string  | false    | First day of the current month | Accepts a string in ISO-8601 short format. Whatever your start date, the system will automatically return recurring expenses expected for that month. For instance, if you input 2020-01-25, the system will return recurring expenses which are to be expected between 2020-01-01 to 2020-01-31.
debit_as_negative | boolean | false    | false                          | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts.

---

# Categories

## Categories Object
Attribute Name      | Type    | Description
------------------- | ----    | -----------
id                  | number  | A unique identifier for the category.
name                | string  | The name of the category. Must be between 1 and 40 characters.
description         | string  | The description of the category. Must not exceed 140 characters.
is_income           | boolean | If true, the transactions in this category will be treated as income.
exclude_from_budget | boolean | If true, the transactions in this category will be excluded from the budget.
exclude_from_totals | boolean | If true, the transactions in this category will be excluded from totals.
updated_at          | string  | The date and time of when the category was last updated (in the ISO 8601 extended format).
created_at          | string  | The date and time of when the category was created (in the ISO 8601 extended format).
is_group            | boolean | If true, the category is a group that can be a parent to other categories.
group_id            | number  | The ID of a category group (or null if the category doesn't belong to a category group).

## Get All Categories
Use this endpoint to get a list of all categories associated with the user's account.

```json
{
  "categories": [
    {
      "id": 83,
      "name": "Test 1",
      "description": "Test description",
      "is_income": false,
      "exclude_from_budget": true,
      "exclude_from_totals": false,
      "updated_at": "2020-01-28T09:49:03.225Z",
      "created_at": "2020-01-28T09:49:03.225Z",
      "is_group": true,
      "group_id": null
    },
    {
      "id": 84,
      "name": "Test 2",
      "description": null,
      "is_income": true,
      "exclude_from_budget": false,
      "exclude_from_totals": true,
      "updated_at": "2020-01-28T09:49:03.238Z",
      "created_at": "2020-01-28T09:49:03.238Z",
      "is_group": false,
      "group_id": 83
    }
  ]
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/categories`

## Create Category
Use this endpoint to create a single category

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response
>
> Returns the ID of the newly-created category


```json
{
    "category_id": 26213
}
```

> Example 400 Response

```json
{ "error": "Missing category name." }
```
```json
{ "error": "Category name must be less than 40 characters." }
```
```json
{ "error": "Category description must be less than 140 characters." }
```
```json
{ "error": "Operation error occurred. Please try again or contact support@lunchmoney.app for assistance." }
```

### HTTP Request

`POST https://dev.lunchmoney.app/v1/categories`

Parameter           | Type    | Required   | Default   | Description
----------          | ------  | ---------- | --------- | ------------
name                | string  | true       | -         | Name of category. Must be between 1 and 40 characters.
description         | string  | false      | -         | Description of category. Must be less than 140 categories.
is_income           | boolean | false      | false     | Whether or not transactions in this category should be treated as income.
exclude_from_budget | boolean | false      | false     | Whether or not transactions in this category should be excluded from budgets.
exclude_from_totals | boolean | false      | false     | Whether or not transactions in this category should be excluded from calculated totals.

---

# Tags

## Tags Object
Attribute Name      | Type    | Description
------------------- | ----    | -----------
id                  | number  | Unique identifier for tag
name                | string  | User-defined name of tag
description         | string  | User-defined description of tag

## Get All Tags
Use this endpoint to get a list of all tags associated with the user's account.

```json
[
    {
        "id": 1807,
        "name": "Wedding",
        "description": "All wedding-related expenses"
    },
    {
        "id": 1808,
        "name": "Honeymoon",
        "description": "All honeymoon-related expenses"
    }
]
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/tags`

---

# Assets

## Assets Object
Attribute Name      | Type   | Description
------------------- | ----   | -----------
id                  | number | Unique identifier for asset
type_name           | string | Primary type of the account. Must be one of:<br><ul> <li>employee compensation</li> <li>cash</li> <li>vehicle</li> <li>loan</li> <li>cryptocurrency</li> <li>investment</li> <li>other</li> <li>credit</li> <li>real estate</li> </ul>
subtype_name        | string | Optional account subtype. Examples include:<br><ul> <li>retirement</li> <li>checking</li> <li>savings</li> <li>prepaid credit card</li><ul>
name                | string | Name of the asset
balance             | string | Current balance of the account in numeric format to 4 decimal places
balance_as_of       | string | Date/time the balance was last updated in ISO 8601 extended format
currency            | string | Three-letter lowercase currency code of the balance
institution_name    | string | Name of institution holding the account
created_at          | string | Date/time the asset was created in ISO 8601 extended format

## Get All Assets
Use this endpoint to get a list of all manually-managed assets associated with the user's account.

> Example 200 Response

```json
{
    "assets": [
        {
            "id": 72,
                "type_name": "cash",
                "subtype_name": "physical cash",
                "name": "Test Asset 1",
                "balance": "1201.0100",
                "balance_as_of": "2020-01-26T12:27:22.000Z",
                "currency": "cad",
                "status": "active",
                "institution_name": "Bank of Me",
                "created_at": "2020-01-26T12:27:22.726Z"
        },
        {
            "id": 73,
            "type_name": "credit",
            "subtype_name": "credit card",
            "name": "Test Asset 2",
            "balance": "0.0000",
            "balance_as_of": "2020-01-26T12:27:22.000Z",
            "currency": "usd",
            "status": "active",
            "institution_name": "Bank of You",
            "created_at": "2020-01-26T12:27:22.744Z"
        },
        {
            "id": 74,
            "type_name": "vehicle",
            "subtype_name": "automobile",
            "name": "Test Asset 3",
            "balance": "99999999999.0000",
            "balance_as_of": "2020-01-26T12:27:22.000Z",
            "currency": "jpy",
            "status": "active",
            "institution_name": "Bank of Mom",
            "created_at": "2020-01-26T12:27:22.755Z"
        },
        {
            "id": 75,
            "type_name": "loan",
            "subtype_name": null,
            "name": "Test Asset 4",
            "balance": "10101010101.0000",
            "balance_as_of": "2020-01-26T12:27:22.000Z",
            "currency": "twd",
            "status": "active",
            "institution_name": null,
            "created_at": "2020-01-26T12:27:22.765Z"
        }
    ]
}
```

### HTTP Request

`GET https://dev.lunchmoney.app/v1/assets`

## Update Asset

Use this endpoint to update a single transaction. You may also use this to split an existing transaction.

```ruby
```

```python
```

```shell
```

```javascript
```

> Example 200 Response

```json
{
    "id": 12,
    "type_name": "cash",
    "subtype_name": "savings",
    "name": "TD Savings Account",
    "balance": "28658.5300",
    "balance_as_of": "2020-03-10T05:17:23.856Z",
    "currency": "cad",
    "institution_name": "TD Bank",
    "created_at": "2019-08-10T22:46:19.486Z"
}
```

> Example 400 Response

```json
{ "errors": [ "type_name must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation" ] }
```

### HTTP Request

`PUT https://dev.lunchmoney.app/v1/assets/:id`

### Body Parameters
Parameter        | Type   | Required | Default | Description
---------        | ----   | -------- | ------- | -----------
type_name        | string | false    | -       | Must be one of: cash, credit, investment, other, real estate, loan, vehicle, cryptocurrency, employee compensation
subtype_name     | string | false    | -       | Max 25 characters
name             | string | false    | -       | Max 45 characters
balance          | string | false    | -       | Numeric value of the current balance of the account. Do not include any special characters aside from a decimal point!
balance_as_of    | string | false    | -       | Has no effect if balance is not defined. If balance is defined, but balance_as_of is not supplied or is invalid, current timestamp will be used.
currency         | string | false    | -       | Three-letter lowercase currency code must exist in our database. Defaults to asset's currency.
institution_name | string | false    | -       | Max 50 characters

---

# Plaid Accounts

## Plaid Accounts Object
Attribute Name      | Type   | Description
------------------- | ----   | -----------
id                  | number | Unique identifier of Plaid account
date_linked         | string | Date account was first linked in ISO 8601 extended format
name                | string | Name of the account. Can be overridden by the user. Field is originally set by Plaid
type                | string | Primary type of account. Typically one of:<br><ul> <li>credit</li> <li>depository</li> <li>brokerage</li> <li>cash</li> <li>loan</li> <li>Investment</li><ul><br> This field is set by Plaid and cannot be altered
subtype             | string | Optional subtype name of account. This field is set by Plaid and cannot be altered
mask                | string | Mask (last 3 to 4 digits of account) of account. This field is set by Plaid and cannot be altered
institution_name    | string | Name of institution associated with account. This field is set by Plaid and cannot be altered
status              | string | Denotes the current status of the account within Lunch Money. Must be one of:<br><ul> <li>active: Account is active and in good state</li> <li>inactive: Account marked inactive from user. No transactions fetched or balance update for this account.</li> <li>relink: Account needs to be relinked with Plaid.</li> <li>syncing: Account is awaiting first import of transactions</li> <li>error: Account is in error with Plaid</li> <li>not found: Account is in error with Plaid</li> <li>not supported: Account is in error with Plaid</li><ul>
last_import         | string | Date of last imported transaction in ISO 8601 extended format (not necessarily date of last attempted import)
balance             | string | Current balance of the account in numeric format to 4 decimal places. This field is set by Plaid and cannot be altered
currency            | string | Currency of account balance. This field is set by Plaid and cannot be altered
balance_last_update | string | Date balance was last updated in ISO 8601 extended format. This field is set by Plaid and cannot be altered
limit               | number | Optional credit limit of the account. This field is set by Plaid and cannot be altered

## Get All Plaid Accounts
Use this endpoint to get a list of all synced Plaid accounts associated with the user's account.

> Example 200 Response

```json
{
  "plaid_accounts": [
    {
      "id": 91,
      "date_linked": "2020-01-28T14:15:09.111Z",
      "name": "401k",
      "type": "brokerage",
      "subtype": "401k",
      "mask": "7468",
      "institution_name": "Vanguard",
      "status": "inactive",
      "last_import": "2019-09-04T12:57:09.190Z",
      "balance": "12345.6700",
      "currency": "usd",
      "balance_last_update": "2020-01-27T01:38:11.862Z",
      "limit": null
    },
    {
      "id": 89,
      "date_linked": "2020-01-28T14:15:09.111Z",
      "name": "Freedom",
      "type": "credit",
      "subtype": "credit card",
      "mask": "1973",
      "institution_name": "Chase",
      "status": "active",
      "last_import": "2019-09-04T12:57:03.250Z",
      "balance": "0.0000",
      "currency": "usd",
      "balance_last_update": "2020-01-27T01:38:07.460Z",
      "limit": 15000
    }
  ]
}
```

Plaid Accounts are individual bank accounts that you have linked to Lunch Money via Plaid. You may link one bank but one bank might contain 4 accounts. Each of these accounts is a Plaid Account.

### HTTP Request

`GET https://dev.lunchmoney.app/v1/plaid_accounts`

---



# Appendix

## Supported Currencies

> Supported Currencies in Lunch Money

```text
aed
afn
all
amd
ang
aoa
ars
aud
awg
azn
bam
bbd
bdt
bgn
bhd
bif
bmd
bnd
bob
brl
bsd
btc
btn
bwp
byn
bzd
cad
cdf
chf
clp
cny
cop
crc
cuc
cup
cve
czk
djf
dkk
dop
dzd
egp
ern
etb
eur
fjd
fkp
gbp
gel
ggp
ghs
gip
gmd
gnf
gtq
gyd
hkd
hnl
hrk
htg
huf
idr
ils
imp
inr
iqd
irr
isk
jep
jmd
jod
jpy
kes
kgs
khr
kmf
kpw
krw
kwd
kyd
kzt
lak
lbp
lkr
lrd
lsl
ltl
lvl
lyd
mad
mdl
mga
mkd
mmk
mnt
mop
mro
mur
mvr
mwk
mxn
myr
mzn
nad
ngn
nio
nok
npr
nzd
omr
pab
pen
pgk
php
pkr
pln
pyg
qar
ron
rsd
rub
rwf
sar
sbd
scr
sdg
sek
sgd
shp
sll
sos
srd
std
svc
syp
szl
thb
tjs
tmt
tnd
top
try
ttd
twd
tzs
uah
ugx
usd
uyu
uzs
vef
vnd
vuv
wst
xaf
xcd
xof
xpf
yer
zar
zmw
zwl
```

Here you'll find a list of currently supported currencies in Lunch Money. If your currency is missing, please let us know via email at [support@lunchmoney.app](mailto:support@lunchmoney.app) and we'll work on getting it added.





# Below this line are just things that came with slate

---




# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, Python, and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

# Kittens

## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

## Delete a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.delete(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted" : ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to delete

