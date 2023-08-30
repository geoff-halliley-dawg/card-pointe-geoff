# Overview

The Card Account Updater service provides merchants the ability to keep their customers' card data up-to-date.

## What's New?

### Date Updated 11/13/2021

**GET Request Support for Profiles**

The GET request now includes an optional query parameter, `profiles=true`, to include corresponding CardPointe Gateway profile details (`profileid`/`acctid` pair) for updates linked to profiles in the GET response.

### Date Updated: 7/20/2021 

- **CONTAC Status Added to GET Response**

    The `status` field in the GET response now supports the `CONTAC` status, which indicates that you should contact the issuer for more information on the status of the card account.

- **Card Account Updater Reporting**

    Card Account Updater reporting is now available in the CardPointe web application. The Card Updates page, available from the Reporting tab, includes an Update History report that displays all Card Account Updater events that occurred within the past year, as well as a Decline Report that displays all declined transactions and the most recent update for each card.

    See the Card Account Updater Reporting topic in the CardPointe Web App User's Guide for detailed information.

## Requirements

To take advantage of the Card Account Updater service, you must meet the following requirements:

- Your merchant account must be enrolled in the Card Account Updater service.

- Your merchant account must be acquired by CardConnect.

- You must use the CardPointe Gateway's Profile service to store customer payment account information, **or** manually enroll specific accounts by making a PUT request to the updater resource of the CardPointe Gateway API.

- If you are using the CardPointe Gateway profile service, profiles must include `"auoptout":"N"` (the default value if not specified). Setting `"auoptout":"y"` opts a profile out of the Card Account Updater service, and the profile will not be checked for updates.

- Whether you are using the CardPointe Gateway's profile service or you are managing your own cardholder profiles using tokens, you must comply with the Visa and Mastercard Stored Credential Transaction Framework Mandate. This card brand mandate includes requirements for obtaining cardholder consent, as well as API and application changes required to appropriately identify transactions using stored customer data. See our support article for detailed information on these requirements.

<!-- theme: warning -->
> Use the inquireMerchant endpoint of the CardPointe Gateway API to determine whether the merchant account is enrolled in the Card Account Updater service.

## Limitations

Consider the following limitations:

- Card Account Updater is only available for merchants on the **First Data North** and **Rapid Connect** back-end processors.
This service only provides updates for **Visa**, **Mastercard**, and **Discover** accounts.
- Additionally, the Card Account Updater API is intended as **alternative solution** to enrolling accounts in the Card Account Updater Service via stored profiles. Using both solutions may cause data integrity issues.

If you currently use the CardPointe Gateway's Profile service to store and manage cardholder profiles, contact isvsupport@fiserv.com before enrolling any accounts using the Card Account Update API.

# Getting Started

To use the Card Account Updater service, you must be enrolled in the service. Contact Support to add the Card Account Updater service to your merchant account.

## How it Works

The Updater service requests daily updates from Visa, Mastercard, and Discover for accounts that are enrolled in this service via CardPointe Gateway profiles, and accounts that have been enrolled using this API. The card brands respond within 3-5 days with any changes to the account, such as:

- new account number
- new expiration date
- account closure

Accounts that are stored in a CardPointe Gateway profile and enrolled in this service are updated automatically when changes to card data are reported by the card brand, while changes to card data for accounts that have been enrolled using the API are stored and can be retrieved at any time using the GET request.

If the card brand reports that a new card has replaced the existing one, or a replacement card was issued with a new expiration date, the new card data is tokenized and takes the place of the previous card data for that account. The new card data is then continually monitored for any further changes.

If the card brand reports that an account has been closed, or a newly enrolled account is invalid, the account is no longer monitored for updates.

If the card brand reports no change in card data for a specific account, the Updater service continues to request updates for that account. Requests for changes in card data are sent daily, but as the card brands typically respond within 3-5 days, an individual account is checked 1-2 times per week for any change.

The following diagram illustrates the Card Account Updater service workflow:

