# Overview

The CardPointe Hosted Payment Page (HPP) is a secure and customizable payment form that can be deployed quickly with little to no development work. As an alternative to redirecting your customers to a standalone checkout page, the HPP allows you to embed the PCI-compliant form in an iFrame on your website. Integrating the HPP with your website allows you to easily accept payments and can reduce your scope of PCI compliance by ensuring that sensitive payment data is never shared with your application.

This guide provides information for integrating the HPP's secure payment features with your web application.

## Features and Benefits

By integrating the HPP with your web application, you can:

- Securely accept credit card and e-check (ACH) payments
- Reduce your scope of PCI compliance
- Authorize and capture payments, or authorize-only and capture later. 
- Customize the look and feel to match your branding, including confirmation pages and receipts.
- Add custom and user-definable fields.
- Establish recurring billing plans and secure, stored customer profiles.
- Use multiple merchant IDs (MIDs) on one hosted plan (helpful for businesses with multiple cost centers).
- Display selectable line items.
- Configure webhooks to automatically send transaction details to your accounting or reporting application.
- Manage advanced settings such as:
    - Maximum number of declined payments
    - Minimum and maximum payment amounts

# Getting Started

Setting up the HPP is quick and easy. Contact integrationdelivery@cardconnect.com to set up your account. 

Once you have Super Admin access to your account, you can configure and customize your HPP to meet your needs. See the CardPointe HPP User Guide for detailed information on setting up your HPP.

## Testing your HPP

To design and test your HPP, you use a sandbox HPP account which is set to **Test Mode (Sandbox)** and configured with a UAT CardPointe merchant ID and credentials. In addition to the initial setup, this allows you to redesign and test your changes on your sandbox HPP without disrupting your live HPP.

## Prefilling Payment Form Fields

When you build your payment form on the HPP Connect Tab, you can prefill the Amount, Invoice #, and Customer ID fields, which are then inserted into the payment form URL as field=value pairs.  

If you want to prefill additional details, you can manually generate or modify HPP URLs to include values for most fields. For example, this can be helpful if your webpage or application already includes a form to gather shipping and billing information, to prevent customers from having to enter their information again on the HPP.

The following example illustrates an HPP URL with the payment amount, invoice ID, customer ID, and customer name  prefilled:
