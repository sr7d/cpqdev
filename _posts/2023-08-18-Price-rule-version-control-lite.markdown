---
layout: post
title: Price Rule Version Control (lite)
description: Learn to implement a lightweight version control system using Field History and Autolaunched Flows to track changes in long text fields, ensuring smoother audits, issue resolutions, and an error-free Salesforce environment.
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/VersionControl.jpg
image_caption: 
tags: [version-control]
featured:
video_embed: 
---
### Download or DIY

*Here's a link to the unmanaged package that does this version control for you --> _____. Or you can read the article below and build it to your own specs.*

# Price Rule Version Control (Lite)

Price Rules are essential records in Salesforce that act as if it were metadata controlling automation and calculations (but it is data). As business requirements evolve and bug fixes are implemented, managing version control and understanding the changes within Price Rules becomes increasingly difficult. This is especially important when facing audits, resolving issues, or reverting to previous configurations when needed. However, maintaining version control for Price Rules can be challenging due to their use of long text fields, hierarchical data, and the potential for data inconsistencies between production and sandbox orgs.

In this tutorial, we will explore how to create a lightweight version control system for Price Rules and their associated objects using Field History and a single Autolaunched Flow. This solution will help you track changes to long text fields, ensuring better control and transparency in your Salesforce environment.

## Understanding the Challenge

Before we dive into building our Price Rule Version Control solution, let's examine the challenges associated with managing Price Rules in Salesforce:

1. **Complexity of Price Rules:** Price Rules are intricate records that govern various aspects of pricing and calculations within Salesforce. As business requirements change, Price Rules may need frequent updates. Without proper version control, it becomes challenging to keep track of these changes and understand their implications.

2. **Maintaining Version Control:** Version control is crucial for any organization, but it can be particularly challenging for Price Rules. Salesforce's native tools do not provide a straightforward way to manage versions of these records, making it difficult to maintain a historical record of changes.

3. **Audit and Issue Resolution:** Inevitably, businesses must undergo audits or deal with issues related to Price Rules. Having a reliable version control system in place can significantly simplify the audit process and streamline issue resolution by providing a clear history of changes.

4. **Data Inconsistencies:** Data inconsistencies between production and sandbox orgs can create confusion and potential errors when working with Price Rules. This discrepancy often occurs due to changes in data between org refreshes.

## Building a Lite Version Control Solution

To address these challenges, we will create a lightweight version control system for Price Rules using Salesforce's Field History and an Autolaunched Flow. Here's a step-by-step guide to implementing this solution:

### Step 1: Enable Field History Tracking

1. Navigate to Setup.
2. Select "Object Manager."
3. Select Price Rules (SBQQ__PriceRule__c).
4. Click on "Fields".
5. Enable field history tracking for all available fields.

### Step 2: Implement a Custom Object (Formula History)

- Create a custom object in Salesforce called "Formula History" to store the historical data of changes made to the "Formulas" field.
- Define fields in this object to capture previous value and new value for long text fields (formulas) on price action and price condition objects

### Step 3: Create an Autolaunched Flow

1. Navigate to Setu.
3. Select "Flows."
4. Create a new Autolaunched Flow.
5. In the Flow Builder, add an element to trigger the flow when a Price Rule record is updated.
6. Use the "Record Changed" event to monitor changes to the selected long text fields (e.g., "Formulas").
7. Add actions to record these changes in a separate object (e.g., "Formula History").

### Step 4: Monitor and Analyze Changes

With this solution in place, you can now monitor and analyze changes to the "Formulas" field of Price Rule records effectively. The "Formula History" object will provide a clear historical record of changes, making it easier to understand and control your Price Rules.

## Considerations

Admins and developers in your org will now be able to refer to the standard Field History related list from either a price rule, price condition, lookup query, or price action (optionally could apply to summary variables). You will see all changes to fields other than long text in this related list as well as who changed it / when. It will show you that a change occurred to a long text field, and that is when the user will navigate to our new object 'Formula History' related list. This related list will show you exactly what changed on that field. This method of version control usually does not migrate well between orgs, however it is totally automated and requires no change from users making changes in your system.
