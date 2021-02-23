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
# Transactions

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

Returns list of Transaction objects.If no query parameters are set, this endpoint will return transactions for the current calendar month (see start_date and end_date)

### HTTP Request

`GET https://dev.lunchmoney.app/v1/transactions`

### Query Parameters

Parameter         | Type    | Required | Default | Description
---------         | ----    | -------- | ------- | -----------
tag_id            | number  | false    |    -    | Filter by tag. Only accepts IDs, not names.
recurring_id      | number  | false    |    -    | Filter by recurring expense
plaid_account_id  | number  | false    |    -    | Filter by Plaid account
category_id       | number  | false    |    -    | Filter by category. Will also match category groups.
asset_id          | number  | false    |    -    | Filter by asset
offset            | number  | false    |    -    | Sets the offset for the records returned
limit             | number  | false    |    -    | Sets the maximum number of records to return.Note: the server will not respond with any indication that there are more records to be returned. Please check the response length to determine if you should make another call with an offset to fetch more transactions.
start_date        | string  | false    |    -    | Denotes the beginning of the time period to fetch transactions for. Defaults to beginning of current month. Required if end_date exists. Format: YYYY-MM-DD.
end_date          | string  | false    |    -    | Denotes the end of the time period you'd like to get transactions for. Defaults to end of current month. Required if start_date exists. Format: YYYY-MM-DD.
debit_as_negative | boolean | false    | false   | Pass in true if you’d like expenses to be returned as negative amounts and credits as positive amounts.



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

