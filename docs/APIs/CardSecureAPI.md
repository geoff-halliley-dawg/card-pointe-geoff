# CardSecure API Overview

CardSecure is the CardPointe Gateway's sensitive data encryption and tokenization service. CardSecure allows you to securely accept and tokenize payment card, ACH (eCheck), and mobile wallet data to ensure the safety of your customers' sensitive payment data.

This guide provides information for developers looking to integrate CardSecure's tokenization and encryption methods using the CardSecure API. For information on integrating CardSecure with your iOS or Android mobile application, see [Tokenizing Using the CardPointe Mobile SDKs](?path=docs/documentation/CardSecure.md#tokenizing-using-cardpointe-mobile-sdks).

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](https://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated: 7/24/2021

An update to CardSecure was deployed to the UAT environment on 7/19/2021 and to the production environment on 7/24/2021.

This release includes internal fixes and updates to CardSecure, including the following enhancement:

### Enhanced Apple Pay Support

When tokenizing an Apple Pay payload, CardSecure no longer requires a specific order for the Apple Pay parameters in the `devicedata` string, with the exception of the `data` value, which must be the first parameter in the string. Previously, CardSecure required a specific sequence for all Apple Pay parameters in the `devicedata` string  to correctly parse the string.

See the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for detailed information on integrating Apple Pay support.

## Date Updated: 2/6/2020 

This release includes the following updates:

### Apple Pay Support

The CardSecure API now supports tokenizing Apple Pay payment tokens. Previously, this was only supported using the legacy API HTTP methods. 

To tokenize Apple Pay payment tokens, make a request to the `tokenize` endpoint using the Apple Pay payment token parameters in the `devicedata` field, and specify `EC_APPLE_PAY` in the `encryptionhandler` field, as follows:

```
POST /cardsecure/api/v1/ccn/tokenize HTTP/1.1
Content-Type: application/json

{
  "devicedata" : "<Apple Pay payload>",
  "encryptionhandler" : "EC_APPLE_PAY"
}
```

See the [tokenize] description for a complete example, and see the [Apple Pay Developer Guide](?path=docs/documentation/ApplePayDeveloperGuide.md) for detailed information on integrating support for Apple Pay, and formatting the payment token string for CardSecure. 
