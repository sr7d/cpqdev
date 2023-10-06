---
layout: post
title: Preview advanced approvals inside of the quote line editor
description: Learn how to preview approval requirements right within the Quote Line Editor (QLE) without the need to save and return to a quote. This guide will show you how to implement this feature using a Visualforce (VF) page, custom action, and Apex controller class.
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/PreviewApproval.jpg
image_caption: 
tags: [code]
featured:
video_embed: 
---
# Preview Advanced Approvals Inside the QLE

With the right configuration, you can empower your users to preview the approvals required for a quote directly within the Quote Line Editor (QLE), eliminating the need to save and return to the quote. In this instructional article, we will guide you through embedding a custom action in the QLE, allowing users to preview approvals immediately after a quick save. Here's how you can achieve this with a VF page, custom action, and apex controller class. Keep in mind that users must also have the appropriate permissions to view the new VF page.

## The Challenge

By default, Salesforce users need to save a quote and return to it to preview the approvals required for that quote. This process can be time-consuming and may disrupt the flow of your QTC operations. However, with the advanced approval preview feature, you can provide a seamless experience right inside the QLE.

## The Solution

### Components

- **Visualforce (VF) Page**: Create a VF page that will display the approval preview information.
- **Custom Action**: Define a custom action that calls the VF page.
- **Apex Controller Class**: Build an Apex controller class to handle the logic for the VF page.
- **Permissions**: Ensure that users have the necessary permissions to access the new VF page.

### Step-by-Step Implementation

#### 1. Create a Visualforce Page

Start by designing a Visualforce page that will display the approval preview information. This page will serve as the interface for users to view the approvals associated with a quote.

```html
<apex:page standardController="SBQQ__Quote__c" extensions="PreviewApprovalsCtrl"
                title="Preview Approvals" lightningStylesheets="false">

    <style type="text/css">
        textarea{
            height: 80px;
            width:80%;
        }
    </style>
    
    <apex:outputPanel id="messages">
        <apex:pageMessages />
    </apex:outputPanel> 

    <apex:pageBlock title="Approval Preview">
        <sbaa:approvalPreview approvals="{!approvals}"/>
    </apex:pageBlock>
</apex:page>
```

#### 2. Develop an Apex Controller Class

Next, create an Apex controller class that will handle the logic for the VF page. This class will fetch the relevant approval information and display it on the VF page.

```java
public with sharing class PreviewApprovalsCtrl {
    private SBQQ__Quote__c sbQuote;

    public PreviewApprovalsCtrl(ApexPages.StandardController stdController) {
        this.sbQuote = (SBQQ__Quote__c)stdController.getRecord();
    }

    public List<SBAA__Approval__c> approvals {
        get {
            if (approvals == null) {
                approvals = SBAA.ApprovalAPI.preview(sbQuote.Id, SBAA__Approval__c.Quote__c);
            }
            return approvals;
        } 
        set;
    }
}
```

```java
//Don't forget to write your own test class
```

#### 3. Define a Custom Action

Now, define a custom action that will call the VF page. Remember this action must execute after a quick save within the QLE to be updated.
- Name: Preview Approval
- Order: 999 (or whereever you prefer)
- Type: Button
- Page: Quote Line Editor
- Location: Quote
- Active: TRUE
- URL Target: Popup
- URL: ```/apex/c__PreviewApprovalQLE?Id={!SBQQ__Quote__c.Id}```

#### 4. Set Permissions

Ensure that users have the necessary permissions to access the new VF page and perform the custom action. This may involve adjusting profiles and permission sets accordingly.

## Conclusion

By embedding a custom action in the Quote Line Editor, you can enhance the user experience and streamline the approval preview process. With this configuration, your Salesforce users can quickly and efficiently assess the approvals required for a quote without the need to save and return to it. This improvement can lead to increased productivity and a smoother Quote-to-Cash workflow for your organization.

Remember to follow best practices for Salesforce development and testing to ensure a successful implementation. With these steps, you can empower your team to make informed decisions and expedite the approval process, ultimately driving better results for your business.
