# Overview

The Hosted iFrame Tokenizer helps to reduce your Payment Card Industry (PCI) Data Security Standards (DSS) audit scope by providing a secure iframe that can be embedded in your online checkout page to accept and transmit sensitive payment data to CardSecure for tokenization.

# What's New?

<!-- theme: warning -->
> Visit [Statuspage](https://status.cardconnect.com/) and click **subscribe to updates** to receive important release and status notifications.

## Date Updated 8/20/2021

The changes below were deployed to the UAT environment on 6/25/2021 and later deployed to the production environment on 7/24/2021.

### New Optional Parameters

A new sendcardtypingevent parameter is available in this release. If true, events are sent to the parent page when input to the Card Number field is detected and when the input has completed or timed out.

Additionally, a new selectinputdelay parameter is available and intended for mobile implementations where user input unexpectedly causes already populated fields to be cleared.
