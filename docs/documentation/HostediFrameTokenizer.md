# Overview

The Hosted iFrame Tokenizer helps to reduce your Payment Card Industry (PCI) Data Security Standards (DSS) audit scope by providing a secure iframe that can be embedded in your online checkout page to accept and transmit sensitive payment data to CardSecure for tokenization.

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](https://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated 8/20/2021

The changes below were deployed to the UAT environment on 6/25/2021 and later deployed to the production environment on 7/24/2021.

### New Optional Parameters

A new `sendcardtypingevent` parameter is available in this release. If **true**, events are sent to the parent page when input to the Card Number field is detected and when the input has completed or timed out.

Additionally, a new `selectinputdelay` parameter is available and intended for mobile implementations where user input unexpectedly causes already populated fields to be cleared.

| Parameter | Default Value | Description | 
| --- | --- | --- |
| sendcardtypingevent | false | If true, an event is sent to the parent page with 'cardTyping':true when the user begins entering a value into the Card Number field. When an onBlur event occurs or the inactivityto value has been met, the card number value is submitted for tokenization and an event is sent to the parent page with 'cardTyping':false. <br> <br> An example of this event can viewed in the web browser's console on the following example page: <br> https://fts-uat.cardconnect.com/itoke/outer-page.html?sendcardtypingevent=true |
| selectinputdelay | 0 | **Note**: This parameter is intended for iOS implementations where user input unexpectedly causes already populated fields to be cleared. <br> <br> Controls how long (in milliseconds) to ignore input to a newly selected field after changing focus. The default value of 0 represents no delay in accepting input to a newly selected field, whereas the maximum value of 1000 represents a 1 second delay before accepting input to a newly selected field. A value of 100 is typically sufficient to avoid unintended clearing without interfering with the user experience. |

### Accessibility Enhancement

The iFrame Tokenizer now includes the `for` HTML attribute for all `label` HTML elements, containing a value that matches the name of the respective input field. This enhancement allows clicking on the label to focus the cursor on the label's respective input field.

Additionally, all `input` and `select` HTML elements now contain the `aria-label` attribute, containing a value that matches the value of the element's `title` for enhanced accessibility.

<!-- theme: warning -->
> When no optional parameters are included to specify a custom title for input or select elements, the element's default title is used for the `aria-label` attribute. See the Input Labels section for information on the default values used.

You can view an example of the new for and aria-label attributes for the Card Number label and input field in a snippet of the iFrame source code below:

```
<label for="ccnumfield" id="cccardlabel">Card Number</label>
<input id="ccnumfield" type="tel" name="ccnumfield" size="19" maxlength="2000" autocomplete="off" title="Credit Card Number" aria-label="Credit Card Number">
```

### Support for RSA Encryption

The Hosted iFrame Tokenizer now supports RSA encryption to encrypt account data before sending the tokenize request to CardSecure.

If you already have had a CardSecure RSA key pair generated for you, the iFrame Tokenizer now automatically obtains the public key and encrypts the data during the tokenization request to CardSecure. You can use the browser's developer tools to verify that the data is encrypted using your RSA key when `"encryptionhandler" : "RSA"` is included in the tokenize request.

If you do not have a CardSecure RSA key pair already generated, the iFrame Tokenizer is unable to obtain a public key for encryption and you will now encounter a 'No public key found' error when viewing the request within the browser's developer tools. You may disregard this error, as the iFrame Tokenizer continues to transmit the account data over a secure connection to CardSecure.

If you would like to utilize RSA encryption with the Hosted iFrame Tokenizer, contact CardPointe Support.

## Date Updated 10/31/2020

The changes below were deployed to the UAT environment on 10/15/2020 and to the production environment on 10/31/2020. 

This release includes the changes described in the 3/6/2020 Update, which were previously deployed to the UAT environment only. The changes that are new to this release are outlined in the section below.

### New Optional Parameters for Field Validation

The following optional URL parameters have been added to validate and return error messages for input to the Card Number, CVV, and Expiration Date fields:

| Parameter | Default Value | Description | 
| --- | --- | --- |
| invalidcreditcardevent | false | If **true**, a `'message'` event is sent to the parent page when the iFrame determines that the value for the Card Number field is invalid or empty. <br> <br> The `'data'` property in the event will contain an empty string, and the `'validationError'` property will contain a description of the validation failure. <br> <br> **Note**: _This parameter should be used in place of_ `invalidinputevent` _when enabling the_ `usecvv` _or_ `useexpiry` _parameters_. |
| invalidcvvevent | false | If **true**, a `'message'` event is sent to the parent page when the iFrame determines that the value for the CVV field is invalid or empty. <br> <br> The `'data'` property in the event will contain an empty string, and the `'validationError'` property will contain a description of the validation failure. <br> <br> **Note**: _Requires_ `usecvv=true`. |
| invalidexpiryevent | false | If **true**, a `'message'` event is sent to the parent page when the iFrame determines that the value for the Expiration Date field is invalid or empty. <br> <br> The `'data'` property in the event will contain an empty string, and the `'validationError'` property will contain a description of the validation failure. <br> <br> **Note**: _Requires_ `useexpiry=true`. |

<!-- type: row -->

<!-- type: card
title: Hosted iFrame Tokenizer Changelog
description: Visit the Changelog for more information on recent updates to the Hosted iFrame Tokenizer
-->

<!-- type: row-end -->

# Understanding the Hosted iFrame Tokenizer

The Hosted iFrame Tokenizer is a secure, hosted web form that contains the input fields necessary for your site visitors to enter their sensitive payment data. This page is incorporated into your own web page using an HTML iframe element.

Your site visitors enter sensitive payment data into the fields of the embedded iframe and this data is then passed securely to CardSecure for tokenization. The tokenized value generated by CardSecure is returned to your web page through the iframe, and is not considered sensitive in nature. You may store this token and use it in a subsequent call to the CardPointe Gateway to process the payment.

The following diagram illustrates the Hosted iFrame Tokenizer data flow at a high level:


