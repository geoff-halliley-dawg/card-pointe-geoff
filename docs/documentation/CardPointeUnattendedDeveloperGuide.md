<!-- type: tab -->

<!-- type: card
description: This solution is currently in development and the information in this guide is subject to change.
-->

<!-- type: row-end -->

# Overview

The CardPointe Unattended solution extends CardPointe Integrated Payments to unattended kiosk applications.

The CardPointe Unattended solution includes the following components:

Unattended card reader device - The unattended device accepts EMV (chip), NFC (contactless), and MSR (magnetic stripe) payments, and connects via serial/USB to the host system and your client application.
Payment Application Engine - The Payment Application Engine (PAE) provides the interface between your client application and the unattended device. Your application sends commands to the device to initiate the card reader, and securely capture the card data.
Web Payment Application - The web payment application retrieves the encrypted card data from the device and initiates an authorization request to the CardPointe Gateway, via HTTP/Ethernet.
App Loader - The app loader runs on the unattended device to check for and automatically download and install updates to the firmware, PAE, and SDK installed on the device.
CardPointe Gateway API - The CardPointe Gateway API provides methods for managing and reporting on your transactions. Your application uses the CardPointe Gateway API to void, refund, or check the status of a transaction.
