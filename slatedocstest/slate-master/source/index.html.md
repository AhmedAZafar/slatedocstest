---
title: Comcarde API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - curl
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>

includes:
  - errors

search: true
---

# Introduction

Our API is organised around REST. Our API has predictable, resource-oriented URLs and uses HTTP response codes to indicate API error. We use built-in HTTP features which are understood by off-the-shelf HTTP clients. JSON is returned by all API responses, including errors. 

We do not support cross-origin resource sharing at the moment which means that you will need to integrate our services in your backend (server-side code). Our API relies on token based authentication mechanism, which is explained in the following section.

We have language bindings in curl and JavaScript! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.


# Payment Request

> To make a sample payment request, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl -X POST "https://payment-gateway.do.bridgelabs.co.uk/v1/payments" 
  -H "accept: application/json" 
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ3cG9ubHkifQ.Gg6TgtGcSdE3RJ9IahQSQ28nqKkjfXiLrOR5AV3bp4I" 
  -H "Content-Type: application/json" 
  -d "{ \"orderDescription\": \"Taxi fare\", \"amount\": 1000, \"currencyCode\": \"GBP\", \"paymentInstrument\": { \"type\": \"card\", \"nameOnCard\": \"John F Doe\", \"pan\": \"4444 3333 2222 1111\", \"expiryDate\": \"01-99\", \"startDate\": \"01-00\", \"issueNumber\": 1, \"cv2\": \"000\" }, \"customerFirstName\": \"John\", \"customerLastName\": \"Smith\", \"customerPhoneNumber\": \"+44 123 1110000\", \"customerEmail\": \"a@b.com\", \"customerIpAddress\": \"123.100.100.200\", \"customerSessionId\": 123456, \"customerOrderCode\": \"ABC123\", \"billingAddress\": { \"firstName\": \"John\", \"lastName\": \"Smith\", \"address1\": \"Flat 1\", \"address2\": \"Victoria House\", \"address3\": \"15 Apple Street\", \"town\": \"Edinburgh\", \"county\": \"Lothian\", \"postcode\": \"BH23 6AA\", \"country\": \"GB\", \"phoneNumber\": \"+44 123 1110000\" }, \"deliveryAddress\": { \"firstName\": \"John\", \"lastName\": \"Smith\", \"address1\": \"Flat 1\", \"address2\": \"Victoria House\", \"address3\": \"15 Apple Street\", \"town\": \"Edinburgh\", \"county\": \"Lothian\", \"postcode\": \"BH23 6AA\", \"country\": \"GB\", \"phoneNumber\": \"+44 123 1110000\" }}"
```

```javascript
const rp = require('request-promise');
const util = require('util');

var options = {
    uri: 'https://payment-gateway.do.bridgelabs.co.uk/v1/payments',
    method: 'POST',
    headers: {
        'Content-Type' : 'application/json',
        'Accept' : 'application/json',
        'Authorization' : 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJmaXJzdGdyb3VwIn0.wGTUE_qSDgu9L3wZlwBWVd59yS_3V4vz5VEt0qvRT7c'
    },
    body: {
        'orderDescription' : 'Demo Order',
        'amount' : 1000,
        'currencyCode' : 'GBP',
        'paymentInstrument' : {
            'type' : 'card',
            'nameOnCard' : 'John Smith',
            'pan' : '4444333322221111',
            'expiryDate' : '01-99'
        },
        'customerEmail' : 'johnsmith@doe.com',
        'customerFirstName' : 'John'
    },
    json: true
}

rp(options)
    .then ((body) => {
        console.log(util.inspect(body, false, null, true));
    })
    .catch((err) => {
        console.log(util.inspect(err, false, null, true));
    });
```

> Make sure to replace `Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ3cG9ubHkifQ.Gg6TgtGcSdE3RJ9IahQSQ28nqKkjfXiLrOR5AV3bp4I` with your API key.

> The above command returns JSON structured like this:

```json
{
  "code": "string",
  "id": "string",
  "message": "string"
}
```

Comcarde API uses bearer tokens to allow access to the API. You can register a new Comcarde API bearer token at our [developer portal](http://example.com/developers).

Comcarde API expects for the bearer token to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ3cG9ubHkifQ.Gg6TgtGcSdE3RJ9IahQSQ28nqKkjfXiLrOR5AV3bp4I`

<aside class="notice">
You must replace <code>Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ3cG9ubHkifQ.Gg6TgtGcSdE3RJ9IahQSQ28nqKkjfXiLrOR5AV3bp4I</code> with your personal Bearer Token.
</aside>

### HTTP Request

`POST https://payment-gateway.do-staging.bridgelabs.co.uk/v1/payments`

### Query Parameters

Parameter | Required | Type | Description
--------- | -------- | ---- | -----------
orderDescription | yes | string | Description of the order for which the transaction needs to be charged
amount | yes | number | The amount in lowest denomination of GBP i.e. pence
currencyCode | yes | string | ISO 4217 compliant currency code (currently allowed value: 'GBP')
paymentInstrument | yes | json | Custom JSON see API reference or swagger specs
customerEmail | yes | string | Customer's valid email address
customerFirstName | yes | string | Customer's first name (no spaces allowed)
customerLastName | no | string | Customer's last name
customerIpAddress | no | string | Customer's IP address
customerSessionID | no | number | Customer's session ID
customerOrderCode | no | string | Customer's order code
billingAddress | no | json | Custom JSON see API reference or swagger specs
deliveryAddress | no | json | Custom JSON see API reference or swagger specs


# Refund Request

## Full Refund


```shell
curl -X POST "https://payment-gateway.do.bridgelabs.co.uk/v1/payments/asjhdaskjdkjashdjaksdh/refund" 
  -H "accept: application/json" 
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ3cG9ubHkifQ.Gg6TgtGcSdE3RJ9IahQSQ28nqKkjfXiLrOR5AV3bp4I" 
  -H "Content-Type: application/json" 
  -d "{}"
```

```javascript

const rp = require('request-promise');
const util = require('util');

var options = {
    uri: `https://payment-gateway.do-staging.bridgelabs.co.uk/v1/payments/${id}/refund`,
    method: 'POST',
    headers: {
        'Content-Type' : 'application/json',
        'Accept' : 'application/json',
        'Authorization' : 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJmaXJzdGdyb3VwIn0.wGTUE_qSDgu9L3wZlwBWVd59yS_3V4vz5VEt0qvRT7c'
    },
    json: true
}

rp(options)
    .then((body) => {
        console.log(util.inspect(body, false, null, true));
    })
    .catch((err) => {
        console.log(util.inspect(err, false, null, true));
    });
```

> The above command returns JSON structured like this:

```json
{
  "code": "string",
  "id": "string",
  "message": "string"
}
```

This endpoint makes a full refund request. 
The transaction ID for the transaction you wish to charge a refund needs to be added in to the URL.

<aside class="notice">
You must replace <code>transaction_ID</code> with the ID of the transaction you want to refund.
</aside>

### HTTP Request

`POST https://payment-gateway.do-staging.bridgelabs.co.uk/v1/payments/<transaction_id>/refund`

<aside class="success">
Remember — a good request is a simple request!
</aside>

## Partial Refund

```shell
curl -X POST "https://payment-gateway.do.bridgelabs.co.uk/v1/payments/<transaction_ID>/refund" 
  -H "accept: application/json" 
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ3cG9ubHkifQ.Gg6TgtGcSdE3RJ9IahQSQ28nqKkjfXiLrOR5AV3bp4I" 
  -H "Content-Type: application/json" 
  -d "{amount: 1000}"
```

```javascript
const rp = require('request-promise');
const util = require('util');

var options = {
    uri: `https://payment-gateway.do-staging.bridgelabs.co.uk/v1/payments/<transaction_ID>/refund`,
    method: 'POST',
    headers: {
        'Content-Type' : 'application/json',
        'Accept' : 'application/json',
        'Authorization' : 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJmaXJzdGdyb3VwIn0.wGTUE_qSDgu9L3wZlwBWVd59yS_3V4vz5VEt0qvRT7c'
    },
    body: {
      'amount' : 1000
    },
    json: true
}

rp(options)
    .then((body) => {
        console.log(util.inspect(body, false, null, true));
    })
    .catch((err) => {
        console.log(util.inspect(err, false, null, true));
    });
```

> The above command returns JSON structured like this:

```json
{
  "code": "string",
  "id": "string",
  "message": "string"
}
```

This endpoint makes a partial refund for the amount specified.

<aside class="notice">
You must replace <code>transaction_ID</code> with the ID of the transaction you want to refund.
</aside>

<aside class="warning">Please not that the amount specified should not be greater than the actual amount of the transaction. <!--<code>&lt;code&gt;</code>--></aside>

### HTTP Request

`POST https://payment-gateway.do-staging.bridgelabs.co.uk/v1/payments/<transaction_ID>/refund`

### URL Parameters

Parameter | Description
--------- | -----------
amount | Lowest denomination number of the currency being used (supported currency: GBP)
