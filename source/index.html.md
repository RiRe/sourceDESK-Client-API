---
title: Client API – sourceDESK

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - php

toc_footers:
  - <a href='https://sourceway.de/en/imprint?layout=sourcedesk'>Imprint</a>

search: false
---

# Introduction

Welcome to the Client API of sourceDESK. You can use this API as client to act with your account, domains and products. Therefore, you need your customer ID and your API key. This documentation covers all available API actions.

We provide example codes in Shell and PHP. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

# Authentication & Request

> To make a request, use this code:

```php
<?php
$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domains/list");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domains/list"
```

> Make sure to replace `CUSTOMER_ID` with your customer ID and `API_KEY` with your API key.

sourceDESK uses an API key combined with your customer ID to allow access to the API. You can find your API key in your client area or request it at support.

Parameters can be sent via GET or POST.

sourceDESK expects for the API key to be included in all API requests to the server in the request.

A request URL is formatted as follows:

`/api/CUSTOMER_ID/API_KEY/METHOD/ACTION`

<aside class="notice">
Hereby, <code>CUSTOMER_ID</code> is your customer ID, <code>API_KEY</code> is your API key, <code>METHOD</code> is the API method you want to call and <code>action</code> is the task you want to execute.
</aside>

# Products

## Get contract details

```php
<?php
$req = [
  "id" => 123,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/info");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/info?id=123"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Account query successful.",
  "data": {
    "status": true,
    "description": "Per API erstellt",
    "order_date": "2019-01-13 15:45:17",
    "product": 1,
    "price": 9.99,
    "period": "monthly",
    "next_invoice": "2019-02-12",
    "contract_time": "1 month",
    "notification_period": "14 days",
    "cancellation_date": "0000-00-00",
    "login_data": {
      "username": "c123",
      "password": "2gJlrE86vS"
    },
    "tasks": {
      "ChangePassword": "new_password",
      "RebootServer": "",
      "AssignDomain": "domain"
    }
  }
}
```

Using this endpoint, you can get details for one of your contracts.

### API endpoint

`/hosting/info`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | - | **Required** The ID of the contract

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No contract ID specified
804 | Specified contract is unknown

## Execute task

```php
<?php
$req = [
  "id" => 123,
  "task" => "AssignDomain",
  "domain" => "test.com"
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/task");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/task?id=123&task=AssignDomain&domain=test.com"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Task executed.",
  "data": {
    // Optional task response
  }
}
```

Using this endpoint, you can execute a task for a contract. You can find the available tasks and the required parameters in order to execute it with the endpoint `hosting/info` described before, using the element `tasks`.

### API endpoint

`/hosting/task`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | - | **Required** The ID of the contract
task | - | **Required** The task to execute

There can be other query parameters according to the task being executed.

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No contract ID specified
804 | Specified contract is unknown
805 | No task specified
806 | Invalid task specified
807 | Required task parameter is missing

The executed task can specify more error codes. Please see the corresponding `message` returned to get more details.

## Set note/description

```php
<?php
$req = [
  "id" => 1,
  "note" => "Per API bestellt",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/set");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/set?id=1&note=Per+API+bestellt"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Description set successful.",
  "data": {}
}
```

Using this endpoint, you can update a note for a existing contract.

### API endpoint

`/hosting/set`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | - | **Required** The ID of the contract
note | "" | The note to set for the contract

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No contract ID specified
804 | Specified contract is unknown

## Order a product

```php
<?php
$req = [
  "id" => 1,
  "note" => "Per API bestellt",
  "async" => true,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/order");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/order?id=1&note=Per+API+bestellt&async=1"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Product order successful.",
  "data": {
    "id": 123,
    "username": "c123",
    "password": "2gJlrE86vS"
  }
}
```

Using this endpoint, you can order a new product.

### API endpoint

`/hosting/order`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | - | **Required** The ID of the product to order
note | "" | A note to set for the contract
async | false | Create the contract in the background - if set to `true`, the request is faster, but you will not get the credentials immediately

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No product ID specified
804 | Specified product is unknown
805 | Not enough credit for this order

## Get cancellation dates

```php
<?php
$req = [
  "id" => 1,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/cancel");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/cancel?id=1"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Cancellation date query successful.",
  "data": {
    "dates": [
      "2019-02-12",
      "2019-03-12",
      "2019-04-12"
    ]
  }
}
```

Using this endpoint, you can get the possible cancellation dates for a contract.

### API endpoint

`/hosting/cancel`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | - | **Required** The ID of the contract

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No contract ID specified
804 | Specified contract is unknown
805 | No cancellation is possible (product without recurring costs)

## Cancel a contract

```php
<?php
$req = [
  "id" => 1,
  "date" => "2019-02-12",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/cancel");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/hosting/cancel?id=1&date=2019-02-12"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Cancellation date set successful.",
  "data": {
    "date": "2019-02-12"
  }
}
```

Using this endpoint, you can set the cancellation date for a contract. Please make sure that you retrieve the possible dates first, as only valid dates are accepted.

<aside class="notice">
In order to revoke an existing contract cancellation, please submit the date <code>0000-00-00</code>.
</aside>

### API endpoint

`/hosting/cancel`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
id | - | **Required** The ID of the contract
date | - | **Required** Date of cancellation

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No contract ID specified
804 | Specified contract is unknown
805 | No cancellation is possible (product without recurring costs)
806 | Provided cancellation date is invalid

# Domains

## List your domains

```php
<?php
$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/list");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/list"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Domains fetched.",
  "data": [
    {
      "domain": "example.com",
      "recurring": 7.99,
      "created": "2019-01-13",
      "expiration": "2020-01-12",
      "auto_renew": true,
      "transfer_lock": true,
      "privacy": false,
      "privacy_price": 0.00,
      "status": "REG_OK",
      "last_sync": "2019-01-13 16:06:56",
      "trade": 0.00,
      "owner": {
        "firstname": "John",
				"lastname": "Doe",
				"company": "",
				"street": "Example street 1",
				"country": "US",
				"postcode": "12345",
				"city": "Example city",
				"telephone": "0019289999",
				"telefax": "0019289998",
				"email": "john.doe@example.com",
				"remarks": ""
      },
      "admin": {
        "firstname": "John",
				"lastname": "Doe",
				"company": "",
				"street": "Example street 1",
				"country": "US",
				"postcode": "12345",
				"city": "Example city",
				"telephone": "0019289999",
				"telefax": "0019289998",
				"email": "john.doe@example.com",
				"remarks": ""
      },
      "tech": {
        "firstname": "John",
				"lastname": "Doe",
				"company": "",
				"street": "Example street 1",
				"country": "US",
				"postcode": "12345",
				"city": "Example city",
				"telephone": "0019289999",
				"telefax": "0019289998",
				"email": "john.doe@example.com",
				"remarks": ""
      },
      "zone": {
        "firstname": "John",
				"lastname": "Doe",
				"company": "",
				"street": "Example street 1",
				"country": "US",
				"postcode": "12345",
				"city": "Example city",
				"telephone": "0019289999",
				"telefax": "0019289998",
				"email": "john.doe@example.com",
				"remarks": ""
      },
      "ns": [
        "ns1.sourceway.de",
        "ns2.sourceway.de"
      ],
    }
  ]
}
```

This endpoint is used to get listed all your domains.

### API endpoint

`/domain/list`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | Filter by domain name (wildcard)

### Return codes

The global return codes applies.

## Register a domain

```php
<?php
$req = [
  "domain" => "example.com",
  "owner_firstname" => "John",
  "owner_lastname" => "Doe",
  "owner_company" => "",
  "owner_street" => "Example street 1",
  "owner_country" => "US",
  "owner_postcode" => "12345",
  "owner_city" => "Example city",
  "owner_telephone" => "0019289999",
  "owner_telefax" => "0019289998",
  "owner_email" => "john.doe@example.com",
  "admin_firstname" => "John",
  "admin_lastname" => "Doe",
  "admin_company" => "",
  "admin_street" => "Example street 1",
  "admin_country" => "US",
  "admin_postcode" => "12345",
  "admin_city" => "Example city",
  "admin_telephone" => "0019289999",
  "admin_telefax" => "0019289998",
  "admin_email" => "john.doe@example.com",
  "tech_firstname" => "John",
  "tech_lastname" => "Doe",
  "tech_company" => "",
  "tech_street" => "Example street 1",
  "tech_country" => "US",
  "tech_postcode" => "12345",
  "tech_city" => "Example city",
  "tech_telephone" => "0019289999",
  "tech_telefax" => "0019289998",
  "tech_email" => "john.doe@example.com",
  "zone_firstname" => "John",
  "zone_lastname" => "Doe",
  "zone_company" => "",
  "zone_street" => "Example street 1",
  "zone_country" => "US",
  "zone_postcode" => "12345",
  "zone_city" => "Example city",
  "zone_telephone" => "0019289999",
  "zone_telefax" => "0019289998",
  "zone_email" => "john.doe@example.com",
  "ns1" => "ns1.sourceway.de",
  "ns2" => "ns2.sourceway.de",
  "ip" => "8.8.8.8",
  "async" => true,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/register");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/register?domain=example.com&owner_firstname=John&owner_lastname=Doe&owner_company=&owner_street=Example+street+1&owner_country=US&owner_postcode=12345&owner_city=Example+city&owner_telephone=0019289999&owner_telefax=0019289998&owner_email=john.doe%40example.com&admin_firstname=John&admin_lastname=Doe&admin_company=&admin_street=Example+street+1&admin_country=US&admin_postcode=12345&admin_city=Example+city&admin_telephone=0019289999&admin_telefax=0019289998&admin_email=john.doe%40example.com&tech_firstname=John&tech_lastname=Doe&tech_company=&tech_street=Example+street+1&tech_country=US&tech_postcode=12345&tech_city=Example+city&tech_telephone=0019289999&tech_telefax=0019289998&tech_email=john.doe%40example.com&zone_firstname=John&zone_lastname=Doe&zone_company=&zone_street=Example+street+1&zone_country=US&zone_postcode=12345&zone_city=Example+city&zone_telephone=0019289999&zone_telefax=0019289998&zone_email=john.doe%40example.com&ns1=ns1.sourceway.de&ns2=ns2.sourceway.de&ip=8.8.8.8&async=1"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Domain registration successful.",
  "data": {}
}
```

Using this endpoint, you can register a domain. Please make sure that the domain is available first.

### API endpoint

`/domain/register`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain to register
owner_* | - | **Required** The owner contact data - see request for details
admin_* | - | **Required** The admin contact data - see request for details
tech_* | Provider data | The tech contact data - needs special permission
zone_* | Provider data | The zone contact data - needs special permission
ns1..5 | - | **At least first 2 required** The nameserver hostnames to use
ip | - | **Required if provider nameservers are used** The standard IP address to create DNS zone
async | false | Set to `true` in order to execute registration in the background - faster response

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No domain name specified
804 | Invalid domain name specified
805 | TLD not supported
806 | Domain availibility status can not be fetched
807 | Domain not available
808 | Not enough credit
809 | Contact details missing (more information in `message` element)
810 | Nameserver missing
811 | Invalid IP address
812 | Domain registration already in progress
813 | Domain registration failed

## Transfer a domain

```php
<?php
$req = [
  "domain" => "example.com",
  "password" => "AuthCode",
  "owner_firstname" => "John",
  "owner_lastname" => "Doe",
  "owner_company" => "",
  "owner_street" => "Example street 1",
  "owner_country" => "US",
  "owner_postcode" => "12345",
  "owner_city" => "Example city",
  "owner_telephone" => "0019289999",
  "owner_telefax" => "0019289998",
  "owner_email" => "john.doe@example.com",
  "admin_firstname" => "John",
  "admin_lastname" => "Doe",
  "admin_company" => "",
  "admin_street" => "Example street 1",
  "admin_country" => "US",
  "admin_postcode" => "12345",
  "admin_city" => "Example city",
  "admin_telephone" => "0019289999",
  "admin_telefax" => "0019289998",
  "admin_email" => "john.doe@example.com",
  "tech_firstname" => "John",
  "tech_lastname" => "Doe",
  "tech_company" => "",
  "tech_street" => "Example street 1",
  "tech_country" => "US",
  "tech_postcode" => "12345",
  "tech_city" => "Example city",
  "tech_telephone" => "0019289999",
  "tech_telefax" => "0019289998",
  "tech_email" => "john.doe@example.com",
  "zone_firstname" => "John",
  "zone_lastname" => "Doe",
  "zone_company" => "",
  "zone_street" => "Example street 1",
  "zone_country" => "US",
  "zone_postcode" => "12345",
  "zone_city" => "Example city",
  "zone_telephone" => "0019289999",
  "zone_telefax" => "0019289998",
  "zone_email" => "john.doe@example.com",
  "ns1" => "ns1.sourceway.de",
  "ns2" => "ns2.sourceway.de",
  "ip" => "8.8.8.8",
  "async" => true,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/transfer");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/transfer?domain=example.com&password=AuthCode&owner_firstname=John&owner_lastname=Doe&owner_company=&owner_street=Example+street+1&owner_country=US&owner_postcode=12345&owner_city=Example+city&owner_telephone=0019289999&owner_telefax=0019289998&owner_email=john.doe%40example.com&admin_firstname=John&admin_lastname=Doe&admin_company=&admin_street=Example+street+1&admin_country=US&admin_postcode=12345&admin_city=Example+city&admin_telephone=0019289999&admin_telefax=0019289998&admin_email=john.doe%40example.com&tech_firstname=John&tech_lastname=Doe&tech_company=&tech_street=Example+street+1&tech_country=US&tech_postcode=12345&tech_city=Example+city&tech_telephone=0019289999&tech_telefax=0019289998&tech_email=john.doe%40example.com&zone_firstname=John&zone_lastname=Doe&zone_company=&zone_street=Example+street+1&zone_country=US&zone_postcode=12345&zone_city=Example+city&zone_telephone=0019289999&zone_telefax=0019289998&zone_email=john.doe%40example.com&ns1=ns1.sourceway.de&ns2=ns2.sourceway.de&ip=8.8.8.8&async=1"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Domain transfer successful.",
  "data": {}
}
```

Using this endpoint, you can transfer a domain. The request is identical to the `register` request, except requiring the parameter `password`.

### API endpoint

`/domain/transfer`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain to transfer
password | - | **Required** The AuthCode to transfer the domain
owner_* | - | **Required** The owner contact data - see request for details
admin_* | - | **Required** The admin contact data - see request for details
tech_* | Provider data | The tech contact data - needs special permission
zone_* | Provider data | The zone contact data - needs special permission
ns1..5 | - | **At least first 2 required** The nameserver hostnames to use
ip | - | **Required if provider nameservers are used** The standard IP address to create DNS zone
async | false | Set to `true` in order to execute transfer in the background - faster response

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | No domain name specified
804 | Invalid domain name specified
805 | TLD not supported
806 | Domain availibility status can not be fetched
807 | Domain is not registered, transfer not available
808 | Not enough credit
809 | Contact details missing (more information in `message` element)
810 | AuthCode not specified
811 | Nameserver missing
812 | Invalid IP address
813 | Domain transfer already in progress
814 | Domain transfer failed

## Edit a domain

```php
<?php
$req = [
  "domain" => "example.com",
  "owner_firstname" => "John",
  "owner_lastname" => "Doe",
  "owner_company" => "",
  "owner_street" => "Example street 1",
  "owner_country" => "US",
  "owner_postcode" => "12345",
  "owner_city" => "Example city",
  "owner_telephone" => "0019289999",
  "owner_telefax" => "0019289998",
  "owner_email" => "john.doe@example.com",
  "admin_firstname" => "John",
  "admin_lastname" => "Doe",
  "admin_company" => "",
  "admin_street" => "Example street 1",
  "admin_country" => "US",
  "admin_postcode" => "12345",
  "admin_city" => "Example city",
  "admin_telephone" => "0019289999",
  "admin_telefax" => "0019289998",
  "admin_email" => "john.doe@example.com",
  "tech_firstname" => "John",
  "tech_lastname" => "Doe",
  "tech_company" => "",
  "tech_street" => "Example street 1",
  "tech_country" => "US",
  "tech_postcode" => "12345",
  "tech_city" => "Example city",
  "tech_telephone" => "0019289999",
  "tech_telefax" => "0019289998",
  "tech_email" => "john.doe@example.com",
  "zone_firstname" => "John",
  "zone_lastname" => "Doe",
  "zone_company" => "",
  "zone_street" => "Example street 1",
  "zone_country" => "US",
  "zone_postcode" => "12345",
  "zone_city" => "Example city",
  "zone_telephone" => "0019289999",
  "zone_telefax" => "0019289998",
  "zone_email" => "john.doe@example.com",
  "ns1" => "ns1.sourceway.de",
  "ns2" => "ns2.sourceway.de",
  "ip" => "8.8.8.8"
  "transfer_lock" => true,
  "auto_renew" => true,
  "privacy" => false,
  "async" => true,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/modify");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/modify?domain=example.com&owner_firstname=John&owner_lastname=Doe&owner_company=&owner_street=Example+street+1&owner_country=US&owner_postcode=12345&owner_city=Example+city&owner_telephone=0019289999&owner_telefax=0019289998&owner_email=john.doe%40example.com&admin_firstname=John&admin_lastname=Doe&admin_company=&admin_street=Example+street+1&admin_country=US&admin_postcode=12345&admin_city=Example+city&admin_telephone=0019289999&admin_telefax=0019289998&admin_email=john.doe%40example.com&tech_firstname=John&tech_lastname=Doe&tech_company=&tech_street=Example+street+1&tech_country=US&tech_postcode=12345&tech_city=Example+city&tech_telephone=0019289999&tech_telefax=0019289998&tech_email=john.doe%40example.com&zone_firstname=John&zone_lastname=Doe&zone_company=&zone_street=Example+street+1&zone_country=US&zone_postcode=12345&zone_city=Example+city&zone_telephone=0019289999&zone_telefax=0019289998&zone_email=john.doe%40example.com&ns1=ns1.sourceway.de&ns2=ns2.sourceway.de&ip=8.8.8.8&transfer_lock=1&auto_renew=1&privacy=0&async=1"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Domain updated.",
  "data": {}
}
```

Using this endpoint, you can modify an existing domain.

### API endpoint

`/domain/modify`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain to edit
owner_* | - | **Required** The owner contact data - see request for details
admin_* | - | **Required** The admin contact data - see request for details
tech_* | Provider data | The tech contact data - needs special permission
zone_* | Provider data | The zone contact data - needs special permission
ns1..5 | - | **At least first 2 required** The nameserver hostnames to use
ip | - | **Required if switching to provider nameservers** The standard IP address to create DNS zone
transfer_lock | false | Set the transfer lock status (if supported by TLD)
auto_renew | false | Set the auto renew status
privacy | false | Set the WHOIS privacy status (if supported by TLD, can be chargeable)
async | false | Set to `true` in order to execute modification in the background - faster response

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Internal error
805 | Contact details missing (more information in `message` element)
806 | Nameserver missing
807 | Missing credit for auto renew
808 | Invalid IP address
809 | Domain update failed

## Change domain owner (trade)

```php
<?php
$req = [
  "domain" => "example.com",
  "owner_firstname" => "John",
  "owner_lastname" => "Doe",
  "owner_company" => "",
  "owner_street" => "Example street 1",
  "owner_country" => "US",
  "owner_postcode" => "12345",
  "owner_city" => "Example city",
  "owner_telephone" => "0019289999",
  "owner_telefax" => "0019289998",
  "owner_email" => "john.doe@example.com",
  "admin_firstname" => "John",
  "admin_lastname" => "Doe",
  "admin_company" => "",
  "admin_street" => "Example street 1",
  "admin_country" => "US",
  "admin_postcode" => "12345",
  "admin_city" => "Example city",
  "admin_telephone" => "0019289999",
  "admin_telefax" => "0019289998",
  "admin_email" => "john.doe@example.com",
  "tech_firstname" => "John",
  "tech_lastname" => "Doe",
  "tech_company" => "",
  "tech_street" => "Example street 1",
  "tech_country" => "US",
  "tech_postcode" => "12345",
  "tech_city" => "Example city",
  "tech_telephone" => "0019289999",
  "tech_telefax" => "0019289998",
  "tech_email" => "john.doe@example.com",
  "zone_firstname" => "John",
  "zone_lastname" => "Doe",
  "zone_company" => "",
  "zone_street" => "Example street 1",
  "zone_country" => "US",
  "zone_postcode" => "12345",
  "zone_city" => "Example city",
  "zone_telephone" => "0019289999",
  "zone_telefax" => "0019289998",
  "zone_email" => "john.doe@example.com",
  "async" => true,
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/trade");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/trade?domain=example.com&owner_firstname=John&owner_lastname=Doe&owner_company=&owner_street=Example+street+1&owner_country=US&owner_postcode=12345&owner_city=Example+city&owner_telephone=0019289999&owner_telefax=0019289998&owner_email=john.doe%40example.com&admin_firstname=John&admin_lastname=Doe&admin_company=&admin_street=Example+street+1&admin_country=US&admin_postcode=12345&admin_city=Example+city&admin_telephone=0019289999&admin_telefax=0019289998&admin_email=john.doe%40example.com&tech_firstname=John&tech_lastname=Doe&tech_company=&tech_street=Example+street+1&tech_country=US&tech_postcode=12345&tech_city=Example+city&tech_telephone=0019289999&tech_telefax=0019289998&tech_email=john.doe%40example.com&zone_firstname=John&zone_lastname=Doe&zone_company=&zone_street=Example+street+1&zone_country=US&zone_postcode=12345&zone_city=Example+city&zone_telephone=0019289999&zone_telefax=0019289998&zone_email=john.doe%40example.com&async=1"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Trade successful.",
  "data": {}
}
```

Using this endpoint, you can execute a domain owner change (trade). This is a chargeable action required if you want to change the domain owner data for a TLD with chargeable domain owner change. You are not able to change the domain owner via the `domain/modify` endpoint in this case.

### API endpoint

`/domain/trade`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain to execute the trade for
owner_* | - | **Required** The owner contact data - see request for details
admin_* | - | **Required** The admin contact data - see request for details
tech_* | Provider data | The tech contact data - needs special permission
zone_* | Provider data | The zone contact data - needs special permission
async | false | Set to `true` in order to execute trade in the background - faster response

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Internal error
805 | Contact details missing (more information in `message` element)
806 | Trade not possible for this TLD
807 | Not enough credit
808 | Trade failed

## Request authcode

```php
<?php
$req = [
  "domain" => "example.com",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/password");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/password?domain=example.com"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Authcode fetched.",
  "data": {
    "code": "q1w2e3r4t5"
  }
}
```

Using this endpoint, you can request the authcode for your domain.

### API endpoint

`/domain/password`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain to request the authcode for

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Internal error

## Delete/return a domain

```php
<?php
$req = [
  "domain" => "example.com",
  "type" => "0",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/delete");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/domain/delete?domain=example.com&type=0"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Domain deleted/returned to registry.",
  "data": {}
}
```

Using this endpoint, you can delete a domain or return it to registry.

### API endpoint

`/domain/delete`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain to request the authcode for
type | - | **Required** The type of deletion (0 = normal delete, 1 = connected return, 2 = disconnected return)

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Internal error
805 | Invalid type specified
806 | Deletion/Returnation failed

# DNS zones

## List your zones

```php
<?php
$req = [
  "domain" => "example.com",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/list");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/list?domain=example.com"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Zones fetched.",
  "data": [
    "example.com",
    "example.de"
  ]
}
```

Using this endpoint, you can get all DNS zones existing for domains using the nameservers of the provider.

### API endpoint

`/dns/list`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | Filter by domain (wildcard)

### Return codes

The global return codes applies.

## Get zone records

```php
<?php
$req = [
  "domain" => "example.com"
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/show");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/show?domain=example.com"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Zone fetched.",
  "data": {
    "262": {
      "name": "",
      "type": "A",
      "content": "8.8.8.8",
      "ttl": 3600,
      "priority": 0
    },
    "263": {
      "name": "",
      "type": "MX",
      "content": "example.com",
      "ttl": 3600,
      "priority": 10
    }
  }
}
```

Using this endpoint, you can get all records existing in the specified DNS zone.

### API endpoint

`/dns/show`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain/zone name you want to get

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Zone not found

## Add a record

```php
<?php
$req = [
  "domain" => "example.com",
  "name" => "www",
  "type" => "A",
  "content" => "8.8.8.8",
  "ttl" => "3600",
  "priority" => "0",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/add");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/add?domain=example.com&name=www&type=A&content=8.8.8.8&ttl=3600&priority=0"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Record added.",
  "data": {}
}
```

Using this endpoint, you can create a new record in a DNS zone.

### API endpoint

`/dns/add`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain/zone name you want to add a record in
name | - | **Required** The record name (can be empty)
type | - | **Required** The record type
content | - | **Required** The content of the record
ttl | - | **Required** Time-to-live (e.g. 3600)
priority | - | **Required** Priority (can be 0)

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Zone not found
805 | Error at adding record (invalid record data)

## Edit a record

```php
<?php
$req = [
  "domain" => "example.com",
  "record" => "262",
  "name" => "www",
  "type" => "A",
  "content" => "8.8.8.8",
  "ttl" => "3600",
  "priority" => "0",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/edit");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/edit?domain=example.com&record=262&name=www&type=A&content=8.8.8.8&ttl=3600&priority=0"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Record edited.",
  "data": {}
}
```

Using this endpoint, you can edit an existing record in a DNS zone.

### API endpoint

`/dns/edit`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain/zone name you want to edit a record in
record | - | **Required** The ID of the record to be edited (find in `dns/show` as key)
name | - | **Required** The record name (can be empty)
type | - | **Required** The record type
content | - | **Required** The content of the record
ttl | - | **Required** Time-to-live (e.g. 3600)
priority | - | **Required** Priority (can be 0)

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Zone not found
805 | No valid record ID specified
806 | Record not found
807 | Error at editing record (invalid record data)

## Delete a record

```php
<?php
$req = [
  "domain" => "example.com",
  "record" => "262",
];

$ch = curl_init("https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/delete");
curl_setopt($ch, CURLOPT_RETURNTRANSFER, true);
curl_setopt($ch, CURLOPT_POSTFIELDS, http_build_query($req));
$res = curl_exec($ch);

if (curl_errno($ch)) {
  die(curl_error($ch));
}

curl_close($ch);
$res = json_decode($res, true);

print_r($res);
```

```shell
curl "https://sourceway.de/api/CUSTOMER_ID/API_KEY/dns/delete?domain=example.com&record=262"
```

> The above command returns JSON structured like this:

```json
{
  "code": 100,
  "message": "Record deleted.",
  "data": {}
}
```

Using this endpoint, you can delete an existing record from a DNS zone.

### API endpoint

`/dns/delete`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
domain | - | **Required** The domain/zone name you want to delete a record in
record | - | **Required** The ID of the record to be deleted (find in `dns/show` as key)

### Return codes

This are the additional return codes for this action. The global return codes applies.

Return Code | Meaning
---------- | -------
803 | Domain not found
804 | Zone not found
805 | No valid record ID specified
806 | Record not found

# Return codes

The sourceDESK API uses the following return codes, they are contained in the `code` element in the JSON response:

Return Code | Meaning
---------- | -------
100 | Request was successful
800 | Authorization failed, please check customer ID and API key
801 | Method unknown
802 | Action unknown

There are additional return codes in failure case for each action, please see the corresponding documentation.

<aside class="notice">
The response also contains the <code>message</code> element, which contains a textual description of the error code and more error details.
</aside>
