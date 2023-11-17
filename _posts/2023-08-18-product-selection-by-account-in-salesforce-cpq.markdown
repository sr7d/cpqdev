---
layout: post
title: Limit Selection of Products by Account in Salesforce CPQ
description: Learn how to control product access by account type in Salesforce CPQ with search filters, multi-select pick lists, and formula fields.
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/SearchFilter.jpg
image_caption: 
tags: [search-filters]
featured:
video_embed: 
---
# How to Limit Adding Specific Products to Your Quote by Account

Managing product availability for different types of accounts is a common request, often not all accounts are eligible for the same products. 
Whether you want to restrict certain products to specific account types like Resellers, Customers, Government entities, or Non-profits, you can efficiently achieve this by using search filters. In this guide, we'll explore how to set up search filters powered by a multi-select pick list field on the product, the account lookup field on the quote, and the search filter contains operator.

## Understanding the Components

### Product Multi-Select Picklist Field

The multi-select pick list field on your products serves as the key to categorizing your products according to the types of accounts that can access them. For example, you might have values like Reseller, Customer, Government, Non-profit, and more. These values will help you identify which products are available to which account types.

`Salesforce Object: Product`<br>
`Field: Eligible Account Types (or whatever name makes sense)`<br>
`Type: Multi-Select Picklist`<br>
`Values: Account Type A, Account Type B, etc`<br>

### Quote Account Type Formula

The account lookup field on the quote is used to establish a connection between the quote and the associated account (field comes standard with CPQ). This linkage enables you to create a formula field, which we'll call `Quote Account Type`, with the formula `SBQQ__Account__r.Eligible_Account_Types__c`.

### Search Filter Logic

Next we're going to create our search filter record in CPQ. Here's how the search filter works:

- **Comparison**: The search filter compares the product's multi-select pick list field values to the `Quote_Account_Type__c` formula field.
- **Filtering Logic**: If `Quote_Account_Type__c` contains any of the values from the multi-select pick list, the search filter will include the product in the search results for that quote.
- **Exclusion**: If `Quote_Account_Type__c` does not match any of the values from the multi-select pick list, the product will be excluded from the search results, ensuring that it cannot be added to the quote for that specific account type.

## Steps to Set Up the Search Filter Record

Configuring this search filter record involves a few steps:

1. **Create Record**: Menu --> Search Filters --> New.

5. **Field Data Entry**:
- Filter Name: 'Product Account Type Filter'
- Target Object: Product
- Target Field: Eligible_Account_Types__c (Don't see that option? Click the gear icon in top right, select edit object, go to fields, select 'Target Field', and add this as an available picklist value)
- Operator: Contains
- Filter Source Object: Quote
- Quote_Account_Types__c (Don't see that option? Click the gear icon in top right, select edit object, go to fields, select 'Target Field', and add this as an available picklist value)
- the remaining fields are optional, use them at your dicretion

## Considerations
While implementing search filters is a fantastic way to manage product availability by account type, here are a few additional tips to enhance your product management strategy in Salesforce CPQ:

### Regularly Update Account Type Values

As your business evolves, you may introduce new account types or modify existing ones. Ensure that you regularly update the multi-select pick list field on your products to reflect these changes. This ensures that your product catalog stays aligned with your evolving business needs.

### Leverage Price Books

Integrate price books with your product management strategy. By assigning specific price books based upon your product data, you can further customize availability on account characteristics. This adds another layer of flexibility to your quoting process, but creating too many price books quickly becomes difficult to manage.

### Report Upon and Analyze Product Usage

Regularly analyze which products are most popular among different account types. This can also serve for audit purposes to ensure that certain products are actually only being sold to the eligible accounts.
