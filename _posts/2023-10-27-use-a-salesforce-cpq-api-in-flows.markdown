---
layout: post
title: Use a Salesforce CPQ API in Flows
description: Learn to extend Salesforce CPQ using Flow and Apex. Follow this guide to create an Apex class with an invocable method, enabling efficient contract amendments. Integrate this solution into Flow for flexibility, suitable for bulk amendments, portals, or programmatic use, all while managing your business logic in Flow.
date: 2023-10-27 15:01:35 +0300
author: admin
image: /images/SalesforceAmenderApiInvoke.jpg
image_caption: 
tags: [code,flow,api]
featured:
video_embed: 
---
# Use a Salesforce CPQ API in Flows

Many organizations encounter the need to extend Salesforce CPQ's functionality beyond its standard user interface. One common request is to enable the amendment of contracts, a process similar to generating contract renewals. Unlike the streamlined renewal process, CPQ doesn't provide a built-in option to initiate an amendment quote and opportunity with a simple checkbox on contracts. Customers also look for the flexibility to handle bulk amendments, manage amendments through a portal, or initiate them programmatically via Apex code.

In this article, we will delve into a pragmatic approach that combines the power of Apex and Flow within Salesforce. We'll guide you through creating an Apex class with an invocable method, which can seamlessly integrate into your Flow. This approach offers the advantages of efficient interaction with the CPQ API through code, all while keeping the core business logic neatly encapsulated within your Flow. Whether you're using it within a Screen Flow, as a Subflow, or integrating it into various parts of your Salesforce codebase, this solution can adapt to your needs.

## Step 1: Create the Apex Class

To start, we need an Apex class with an invocable method, allowing interaction with Salesforce CPQ's API. We'll call this class `ContractAmenderFlowHandler`. Here's a functional example of the class:

```apex
public with sharing class ContractAmenderFlowHandler {
    // Invocable method for amending contracts
    @InvocableMethod(label='Amend Contract' description='Amend a CPQ contract')
    public static List<AmendContractResponse> amendContract(List<AmendContractRequest> requestList) {
        List<AmendContractResponse> responseList = new List<AmendContractResponse>();

        for (AmendContractRequest request : requestList) {
            String contractId = request.uid; // Contract ID to amend

            // Load the contract and perform amendments
            QuoteModel quote = load(contractId);

            AmendContractResponse response = new AmendContractResponse();
            response.quote = quote;

            responseList.add(response);
        }

        return responseList;
    }

    // Method to load the contract using CPQ API
    public static QuoteModel load(String contractId) {
        String quoteJSON = SBQQ.ServiceRouter.load('SBQQ.ContractManipulationAPI.ContractAmender', contractId, null);
        return (QuoteModel) JSON.deserialize(quoteJSON, QuoteModel.class);
    }

    public class AmendContractRequest {
        @InvocableVariable(label='Contract ID' description='The Salesforce Contract ID to amend')
        public String uid;
    }

    public class AmendContractResponse {
        @InvocableVariable(label='Amended Quote' description='The amended quote data')
        public QuoteModel quote;
    }
}
```

## Step 2: Create a Screen Flow

The next step is to create a Screen Flow within Salesforce. This Flow will serve as the user interface for initiating contract amendments. You can design the flow to collect necessary input from the user, such as contract details or the type of amendment.

## Step 3: Add the Apex Action to the Flow

Once you've set up the Screen Flow, you can add the Apex action to it. This action will invoke the `ContractAmenderFlowHandler` class's `amendContract` method. Be sure to populate the contract ID into the input variable for the action.

1. In your Screen Flow, add an element that triggers the Apex action, such as a button or a screen component.

2. Configure the properties of the Apex action to call the `ContractAmenderFlowHandler.amendContract` method.

3. Provide the necessary input variables, including the Contract ID, for the Apex action.

4. Handle the output from the Apex action, which may include details about the success or failure of the amendment.

With these three steps, you'll have a practical solution for amending contracts in Salesforce CPQ using Flow and Apex. This approach allows you to efficiently interact with the CPQ API through code while maintaining your business logic within the Flow, making it adaptable and straightforward to integrate into various aspects of your Salesforce environment.

## What About Other CPQ APIs?

If you're interested in exploring other CPQ APIs and functionality, you can access a comprehensive Postman collection provided by Salesforce [here](https://www.postman.com/salesforce-developers/workspace/salesforce-developers/folder/12721794-18c19817-3a82-4403-92bc-d96276452c77). This collection allows you to explore and interact with various CPQ APIs, making it a valuable resource for extending your Salesforce CPQ capabilities further.
