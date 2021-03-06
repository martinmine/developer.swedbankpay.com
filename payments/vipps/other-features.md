---
title: Swedbank Pay Payments Vipps Other Features
sidebar:
  navigation:
  - title: Vipps Payments
    items:
    - url: /payments/vipps
      title: Introduction
    - url: /payments/vipps/redirect
      title: Redirect
    - url: /payments/vipps/seamless-view
      title: Seamless View
    - url: /payments/vipps/after-payment
      title: After Payment
    - url: /payments/vipps/other-features
      title: Other Features
---


{% include settlement-reconciliation.md %}

{% include payment-link.md %}

{% include payment-resource.md  api_resource="vipps" documentation_section="vipps"
show_status_operations=true%}

{% include payment-transaction-states.md %}

### Create Payment

To create a Vipps payment, you perform an HTTP `POST` against the
`/psp/vipps/payments` resource.

An example of a payment creation request is provided below.
Each individual field of the JSON document is described in the following
section.
Use the [expand][technical-reference-expansion] request parameter to get a
response that includes one or more expanded sub-resources inlined.

{:.code-header}
**Request**

```http
POST /psp/vipps/payments HTTP/1.1
Host: {{ page.api_host }}
Authorization: Bearer <AccessToken>
Content-Type: application/json

{
    "payment": {
        "operation": "Purchase",
        "intent": "Authorization",
        "currency": "NOK",
        "prices": [
            {
                "type": "Vipps",
                "amount": 1500,
                "vatAmount": 0
            }
        ],
        "description": "Vipps Test",
        "payerReference": "ABtimestamp",
        "userAgent": "Mozilla/5.0",
        "language": "nb-NO",
        "urls": {
            "hostUrls": [ "https://example.com", "https://example.net" ],
            "completeUrl": "https://example.com/payment-completed",
            "cancelUrl": "https://example.com/payment-canceled",
            "paymentUrl": "https://example.com/perform-payment",
            "callbackUrl": "{{ page.api_url }}/psp/payment-callback",
            "logoUrl": "https://example.com/path/to/logo.png",
            "termsOfServiceUrl": "https://example.com/terms.pdf"
        },
        "payeeInfo": {
            "payeeId": "{{ page.merchant_id }}",
            "payeeReference": "Postmantimestamp",
            "payeeName": "Merchant1",
            "productCategory": "A123",
            "orderReference": "or-12456",
            "subsite": "MySubsite"
        },
        "prefillInfo": {
            "msisdn": "+4793000001"
        }
    }
}
```

{:.table .table-striped}
|     Required     | Field                        | Type         | Description                                                                                                                                                                                                                                               |
| :--------------: | :--------------------------- | :----------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| {% icon check %}︎ | `payment`                    | `object`     | The `payment` object.                                                                                                                                                                                                                                     |
| {% icon check %}︎ | └➔&nbsp;`operation`          | `string`     | `Purchase`                                                                                                                                                                                                                                                |
| {% icon check %}︎ | └➔&nbsp;`intent`             | `string`     | `Authorization`                                                                                                                                                                                                                                           |
| {% icon check %}︎ | └➔&nbsp;`currency`           | `string`     | NOK                                                                                                                                                                                                                                                       |
| {% icon check %}︎ | └➔&nbsp;`prices`             | `object`     | The [`prices`][prices] object.                                                                                                                                                                                                                            |
| {% icon check %}︎ | └─➔&nbsp;`type`              | `string`     | `vipps`                                                                                                                                                                                                                                                   |
| {% icon check %}︎ | └─➔&nbsp;`amount`            | `integer`    | {% include field-description-amount.md currency="NOK" %}                                                                                                                                                                                                  |
| {% icon check %}︎ | └─➔&nbsp;`vatAmount`         | `integer`    | {% include field-description-vatamount.md currency="NOK" %}                                                                                                                                                                                               |
| {% icon check %}︎ | └➔&nbsp;`description`        | `string(40)` | {% include field-description-description.md documentation_section="vipps" %}                                                                                                                                                                                 |
|                  | └➔&nbsp;`payerReference`     | `string`     | The reference to the payer (consumer/end-user) from the merchant system, like mobile number, customer number etc.                                                                                                                                         |
| {% icon check %}︎ | └➔&nbsp;`userAgent`          | `string`     | The user agent reference of the consumer's browser - [see user agent][user-agent]]                                                                                                                                                                        |
| {% icon check %}︎ | └➔&nbsp;`language`           | `string`     | {% include field-description-language.md api_resource="vipps" %}                                                                                                                                                                                                                              |
| {% icon check %}︎ | └➔&nbsp;`urls`               | `object`     | The object containing URLs relevant for the `payment`.                                                                                                                                                                                                    |
| {% icon check %}︎ | └─➔&nbsp;`hostUrls`          | `array`      | The array of URIs valid for embedding of Swedbank Pay Hosted Views.                                                                                                                                                                                       |
| {% icon check %}︎ | └─➔&nbsp;`completeUrl`       | `string`     | The URI that Swedbank Pay will redirect back to when the payment page is completed. This does not indicate a successful payment, only that it has reached a completion state. A `GET` request needs to be performed on the payment to inspect it further. |
|                  | └─➔&nbsp;`cancelUrl`         | `string`     | The URI to redirect the payer to if the payment is canceled, either by the payer or by the merchant trough an `abort` request of the `payment`.                                                                                                           |
|                  | └─➔&nbsp;`paymentUrl`        | `string`     | The URI that Swedbank Pay will redirect back to when the payment menu needs to be loaded, to inspect and act on the current status of the `payment`.                                                                                                      |
|                  | └─➔&nbsp;`callbackUrl`       | `string`     | The URI that Swedbank Pay will perform an HTTP `POST` request against every time a transaction is created on the payment. See [callback][callbackreference] for details.                                                                                  |
|                  | └─➔&nbsp;`logoUrl`           | `string`     | The URI that will be used for showing the customer logo. Must be a picture with at most 50px height and 400px width. Require https.                                                                                                                       |
|                  | └─➔&nbsp;`termsOfServiceUrl` | `string`     | {% include field-description-termsofserviceurl.md %}                                                                                                                                                                                                      |
| {% icon check %}︎ | └➔&nbsp;`payeeInfo`          | `object`     | The object containing information about the payee.                                                                                                                                                                                                        |
| {% icon check %}︎ | └─➔&nbsp;`payeeId`           | `string`     | This is the unique id that identifies this payee (like merchant) set by Swedbank Pay.                                                                                                                                                                            |
| {% icon check %}︎ | └─➔&nbsp;`payeeReference`    | `string(30)` | A unique reference from the merchant system. It is set per operation to ensure an exactly-once delivery of a transactional operation. See [`payeeReference`][technical-reference-payeeReference] for details.                                             |
|                  | └─➔&nbsp;`payeeName`         | `string`     | The payee name (like merchant name) that will be displayed to consumer when redirected to Swedbank Pay.                                                                                                                                                          |
|                  | └─➔&nbsp;`productCategory`   | `strin`      | A product category or number sent in from the payee/merchant. This is not validated by PayEx, but will be passed through the payment process and may be used in the settlement process.                                                                   |
|                  | └─➔&nbsp;`orderReference`    | `string(50)` | The order reference should reflect the order reference found in the merchant's systems.                                                                                                                                                                   |
|                  | └─➔&nbsp;`prefillInfo`       | `string`     | The mobile number that will be pre-filled in the Swedbank Pay Payments. The consumer may change this number in the UI.                                                                                                                                    |
|                  | └─➔&nbsp;`subsite`           | `string(40)` | The `subsite` field can be used to perform split settlement on the payment. The `subsites` must be resolved with Swedbank Pay reconciliation before being used.                                                                                           |

{:.code-header}
**Response**

```http
HTTP/1.1 200 OK
Content-Type: application/json

{
    "payment": {
        "id": "/psp/vipps/payments/{{ page.payment_id }}",
        "number": 72100003079,
        "created": "2018-09-05T14:18:44.4259255Z",
        "instrument": "Vipps",
        "operation": "Purchase",
        "intent": "Authorization",
        "state": "Ready",
        "currency": "NOK",
        "prices": {
            "id": "/psp/vipps/payments/{{ page.payment_id }}/prices"
        },
        "amount": 0,
        "description": "Vipps Test",
        "payerReference": "AB1536157124",
        "initiatingSystemUserAgent": "PostmanRuntime/7.2.0",
        "userAgent": "Mozilla/5.0 weeeeee",
        "language": "nb-NO",
        "urls": {
            "id": "/psp/vipps/payments/{{ page.payment_id }}/urls"
        },
        "payeeInfo": {
            "id": "/psp/vipps/payments/{{ page.payment_id }}/payeeinfo"
        },
        "metadata": {
            "id": "/psp/vipps/payments/{{ page.payment_id }}/metadata"
        }
    },
    "operations": [
        {
            "method": "PATCH",
            "href": "{{ page.api_url }}/psp/vipps/payments/{{ page.payment_id }}",
            "rel": "update-payment-abort"
        },
        {
            "method": "GET",
            "href": "{{ page.front_end_url }}/vipps/payments/authorize/afccf3d0016340620756d5ff3e08f69b555fbe2e45ca71f4bd159ebdb0f00065",
            "rel": "redirect-authorization"
        }
    ]
}
```

### Purchase

Posting a payment (operation `Purchase`) returns the options of aborting the
payment altogether or creating an authorization transaction through the
`redirect-authorization` hyperlink.

{:.code-header}
**Request**

```js
{
    "payment": {
        "operation": "Purchase"
    }
}
```

{% include complete-url.md %}

## Payment Url

{% include payment-url.md full_reference=true %}

{% include prices.md api_resource="vipps" %}

## Description

{% include description.md %}

{% include payee-info.md api_resource="vipps" %}

{% include payee-reference.md %}

{% include transactions-reference.md api_resource="vipps"
documentation_section="vipps" %}

{% include callback-reference.md api_resource="vipps" %}

## Problem messages

When performing unsuccessful operations, the eCommerce API will respond with a
problem message.
We generally use the problem message type and status code to identify the
nature of the problem.
The problem name and description will often help narrow down the specifics of
the problem.

{% include common-problem-types.md %}

### Error types from Vipps (Init-call)

All Vipps error types will have the following URI in front of type:
`https://api.payex.com/psp/errordetail/vipps/<error-type>`

{:.table .table-striped}
| Type          | Status | Note       |
| :------------ | :----- | :--------- |
| *VIPPS_ERROR* | 403    | All errors |

### Error types from Vipps (Callback)

All Vipps error types will have the following URI in front of type:
`https://api.payex.com/psp/errordetail/vipps/<error-type>`

{:.table .table-striped}
| Type             | Status | Note                       |
| :--------------- | :----- | :------------------------- |
| *VIPPS_DECLINED* | 400    | Any status that is not YES |

### Error types from Acquirer

All Vipps error types will have the following URI in front of type:
`https://api.payex.com/psp/errordetail/vipps/<error-type>`

{:.table .table-striped}
| Type                            | Status |
| :------------------------------ | :----- |
| *`CARD_BLACKLISTED`*            | `400`  |
| *`PAYMENT_TOKEN_ERROR`*         | `403`  |
| *`CARD_DECLINED`*               | `403`  |
| *`ACQUIRER_ERROR`*              | `403`  |
| *`ACQUIRER_CARD_BLACKLISTED`*   | `403`  |
| *`ACQUIRER_CARD_EXPIRED`*       | `403`  |
| *`ACQUIRER_CARD_STOLEN`*        | `403`  |
| *`ACQUIRER_INSUFFICIENT_FUNDS`* | `403`  |
| *`ACQUIRER_INVALID_AMOUNT`*     | `403`  |
| *`ACQUIRER_POSSIBLE_FRAUD`*     | `403`  |
| *`FRAUD_DETECTED`*              | `403`  |
| *`BAD_REQUEST`*                 | `500`  |
| *`INTERNAL_SERVER_ERROR`*       | `500`  |
| *`BAD_GATEWAY`*                 | `502`  |
| *`ACQUIRER_GATEWAY_ERROR`*      | `502`  |
| *`ACQUIRER_GATEWAY_TIMEOUT`*    | `504`  |
| *`UNKNOWN_ERROR`*               | `500`  |

{% include iterator.html
        prev_href="after-payment"
        prev_title="Back: After Payment" %}

[callbackreference]: #callback
[payment-order]: #payment-order
[prices]: #prices
[problems-reference]: #problem-messages
[technical-reference-expansion]: /home/technical-information#expansion
[user-agent]: https://en.wikipedia.org/wiki/User_agent
[technical-reference-payeeReference]: #payee-reference
