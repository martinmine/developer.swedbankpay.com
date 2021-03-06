---
title: Swedbank Pay Card Payments – Other Features
sidebar:
  navigation:
  - title: Card Payments
    items:
    - url: /payments/card/
      title: Introduction
    - url: /payments/card/redirect
      title: Redirect
    - url: /payments/card/seamless-view
      title: Seamless View
    - url: /payments/card/direct
      title: Direct
    - url: /payments/card/capture
      title: Capture
    - url: /payments/card/mobile-card-payments
      title: Mobile Card Payments
    - url: /payments/card/after-payment
      title: After Payment
    - url: /payments/card/other-features
      title: Other Features
---

{% include jumbotron.html body="Welcome to Other Features - a subsection of
Credit Card. This section has extented code examples and features that were not
covered by the other subsections." %}

{% include payment-resource.md api_resource="creditcard" documentation_section="card"
show_status_operations=true %}

{% include payment-transaction-states.md %}

{% include create-payment.md %}

## Purchase

A `Purchase` payment is a straightforward way to charge the card of the payer.
It is followed up by posting a `capture`, `cancellation` or `reversal` transaction.

An example of a request is provided below. Each individual field of the JSON
document is described in the following section.

{% include alert-risk-indicator.md %}

{% include card-purchase.md full_reference=true %}

{% include complete-url.md %}

## Description

{% include description.md %}

{% include recur.md %}

{% include unscheduled-purchase.md %}

{% include payout.md %}

{% include verify.md %}

{% include one-click-payments.md %}

{% include callback-reference.md api_resource="creditcard" %}

{% include payment-link.md %}

{% include create-authorization-transaction.md %}

{% include payee-info.md api_resource="creditcard" %}

{% include prices.md %}

## Co-badge Card Choice for Dankort

Due to new [EU regulations from 2016-06-09][eu-regulation] regarding cards that have
more than one payment application, we have developed support for the end users
of Dankort to be able to choose their preferred payment application on the
Swedbank Pay payment page. If you are a Dankort user, read more about this
feature at [Dankort][dankort-eu].

As a merchant you are able to set a priority selection of payment application by
contacting [SwedbankPay Support][swedbankpay-support]. The end user will always
be able to override this priority selection on the payment page.

If you want more information about Co-badge Card Choice for Dankort users please
contact [SwedankPay Support][swedbankpay-support]. The example below shows the
payment window where there payer can choose between Dankort or Visa before
completing the payment.

![Co-badge Dankort cards with option to choose between Dankort and Visa before paying][card-badge]{:height="500px" width="425px"}

{% include settlement-reconciliation.md %}

{% include common-problem-types.md %}

{% include card-problem-messages.md %}

### Seamless View Events

### `onPaymentCompleted`

This event triggers when a payment has completed successfully.
The `onPaymentCompleted` event is raised with the following event argument
object:

{:.code-header}
**`onPaymentCompleted` event object**

```js
{
    "id": "/psp/creditcard/payments/{{ page.payment_id }}",
    "redirectUrl": "https://en.wikipedia.org/wiki/Success"
}
```

{:.table .table-striped}
| Field         | Type     | Description                                                     |
| :------------ | :------- | :-------------------------------------------------------------- |
| `id`          | `string` | {% include field-description-id.md %}                           |
| `redirectUrl` | `string` | The URI the user will be redirect to after a completed payment. |

### `onPaymentCanceled`

This event triggers when the user cancels the payment.
The `onPaymentCanceled` event is raised with the following event argument
object:

{:.code-header}
**`onPaymentCanceled` event object**

```js
{
    "id": "/psp/creditcard/payments/{{ page.payment_id }}",
    "redirectUrl": "https://en.wikipedia.org/wiki/Canceled"
}
```

{:.table .table-striped}
| Field         | Type     | Description                                                    |
| :------------ | :------- | :------------------------------------------------------------- |
| `id`          | `string` | {% include field-description-id.md %}                          |
| `redirectUrl` | `string` | The URI the user will be redirect to after a canceled payment. |

### `onPaymentFailed`

This event triggers when a payment has failed, disabling further attempts to
perform a payment. The `onPaymentFailed` event is raised with the following
event argument object:

{:.code-header}
**`onPaymentFailed` event object**

```js
{
    "id": "/psp/creditcard/payments/{{ page.payment_id }}",
    "redirectUrl": "https://en.wikipedia.org/wiki/Failed"
}
```

{:.table .table-striped}
| Field         | Type     | Description                                                  |
| :------------ | :------- | :----------------------------------------------------------- |
| `id`          | `string` | {% include field-description-id.md %}                        |
| `redirectUrl` | `string` | The URI the user will be redirect to after a failed payment. |

### `onPaymentTermsOfService`

This event triggers when the user clicks on the "Display terms and conditions"
link. The `onPaymentTermsOfService` event is raised with the following event
argument object:

{:.code-header}
**`onPaymentTermsOfService` event object**

```js
{
    "origin": "owner",
    "openUrl": "https://example.org/terms.html"
}
```

{:.table .table-striped}
| Field     | Type     | Description                                                                             |
| :-------- | :------- | :-------------------------------------------------------------------------------------- |
| `origin`  | `string` | `owner`, `merchant`. The value is always `merchant` unless Swedbank Pay hosts the view. |
| `openUrl` | `string` | The URI containing Terms of Service and conditions.                                     |

### `onError`

This event triggers during terminal errors or if the configuration fails
validation. The `onError` event will be raised with the following event argument
object:

{:.code-header}
**`onError` event object**

```js
{
    "origin": "creditcard",
    "messageId": "{{ page.transaction_id }}",
    "details": "Descriptive text of the error"
}
```

{:.table .table-striped}
| Field       | Type     | Description                                                    |
| :---------- | :------- | :------------------------------------------------------------- |
| `origin`    | `string` | `creditcard`, identifies the system that originated the error. |
| `messageId` | `string` | A unique identifier for the message.                           |
| `details`   | `string` | A human readable and descriptive text of the error.            |

{% include iterator.html prev_href="after-payment" prev_title="Back: After
payment"  %}

[purchase]: #purchase
[user-agent-definition]: https://en.wikipedia.org/wiki/User_agent
[cancel]: /payments/card/after-payment#cancellations
[capture]: /payments/card/capture
[callback]: /payments/card/other-features#callback
[card-badge]: /assets/img/card-badge.png
[dankort-eu]: https://www.dankort.dk/Pages/Dankort-eller-Visa.aspx
[eu-regulation]: https://ec.europa.eu/commission/presscorner/detail/en/MEMO_16_2162
[mcc]: https://en.wikipedia.org/wiki/Merchant_category_code
[price-resource]: /payments/card/other-features#prices
[redirect]: /payments/card/redirect
[hosted-view]: /payments/card/seamless-view
[one-click-payments]: #one-click-payments
[payee-reference]: #payee-reference
[split-settlement]: #split-settlement
[settlement-and-reconciliation]: #settlement-and-reconciliation
[swedbankpay-support]: https://www.swedbankpay.se/support
[recurrence]: #recur
[verify]: #verify
[payout]: #payout
[card-payment]: /assets/img/payments/card-payment.png
