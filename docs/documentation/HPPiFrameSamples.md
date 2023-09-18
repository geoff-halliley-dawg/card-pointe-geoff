# Overview

This page demos the CardPointe Hosted Payment Page (HPP) embedded into a web page within an iFrame. The HPP offers a simple, PCI-compliant means for embedding a secure checkout form in your website. Payment card data entered in the HPP iFrame is securely transmitted to the CardPointe Gateway, and is never exposed to your application.

See the [CardPointe Hosted Payment Page User Guide](?path=docs/documentation/HostedPaymentPageDeveloperGuide.md) for information on setting up a new HPP.

<!-- theme: warning -->
> The sample HPP examples in this guide are functioning examples associated with a UAT merchant account. To run a test transaction, use a UAT test card, and enter your email address to receive a receipt. For a list of test cards, see [UAT Test Card Data](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md#uat-test-card-date) in the [CardPointe Gateway Developer Guides](?path=docs/documentation/CardPointeGatewayDeveloperGuides.md).

# HPP iFrame Examples

The HPP can be generated in two formats, a minimal form used to capture only the amount and type of the sale and the payment card details, or a more detailed checkout form, including the cardholder's billing information.

## Minimal HPP Example

The following example illustrates the minimal format.

#### Minimal HPP iFrame Example Code

```HTML
<iframe src="https://cphppdemo.securepayments.cardpointe.com/pay?&mini=1" frameborder="0" width="100%" height="700"></iframe>
```

#### Mini HPP Checkout Form Example



## Detailed HPP Example

Expand the following accordion for a more detailed HPP example, including fields for the cardholder's billing information.

#### Detailed HPP iFrame Example Code

```HTML
<iframe src="https://cphppdemo.securepayments.cardpointe.com/pay?" frameborder="0" width="100%" height="700"></iframe>
```

#### Full HPP Checkout Form Example

