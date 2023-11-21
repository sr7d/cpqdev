---
layout: post
title: Quote Line Editor Links
description: Learn how to enhance your Salesforce experience by providing direct links to and from the Quote Line Editor (QLE). Streamline your sales processes, save time, and improve user productivity with hyperlink formulas, redirects, dynamic link building, and screen flows. 
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/QLELinks.jpg
image_caption: 
tags: [quote-line-editor]
featured:
video_embed: 
---
# Provide Links Directly to and from the Quote Line Editor

In this blog post, we'll describe how to create links to and from the Quote Line Editor through hyperlink formulas, redirects, links within screen flows, and more.

## Linking Directly to the Quote Line Editor

### Hyperlink Formulas
One of the simplest ways to provide a direct link to the Quote Line Editor is by using hyperlink formulas. These formulas can be used in various Salesforce components, but the example below shows it in use on the Quote object as a formula field. To create a hyperlink formula that leads to the QLE from the Quote, create the following field:

- Object: Quote
- Type: Formula, Text
- Name: QLE, or whatever makes sense
- Syntax:

```java
HYPERLINK("/apex/sbqq__sb?scontrolCaching=1&id=" + Id + "#quote/le?qId=" + Id, "Link to QLE")
```

Replace "Link Text" ("Link to QLE") with the text you want to display for the link. This will save users the hassle of navigating through the Salesforce interface to find the Quote Line Editor manually.

## Linking Out of the Quote Line Editor

While linking to the QLE is essential for efficiency, there are situations where you may need to provide links out of the QLE, such as returning to a specific record or navigating to a related page. Here's how you can achieve this:

### Dynamic Link Building Formula
Building links out of the QLE requires a bit more effort, as it often involves dynamically generating URLs in a way that does not interfere with the settings of the Visualforce page. For example, you can create a formula field for a link inside the QLE (which will require quick save) like this:

```java
HYPERLINK(
//This section is used to build the root URL of the org, in this example when the formula finds "--sbqq," it will remove the rest of the URL. I suggest you adjust this part to fit your org's namespace or use another method to provide the root URL of the org.
SUBSTITUTE(
SUBSTITUTE(LEFT($Api.Partner_Server_URL_260, FIND('/services', $Api.Partner_Server_URL_260)), "--sbqq", ""),
".vf.force.com/",
"") +
//This section is link building to a product-related list in a way that does not interfere with how links are rendered in the QLE.
".lightning.force.com/lightning/r/Product2/" +
SBQQ__Product__c +
"/related/RelationshipName" +
"__r/view"
,
//This section will control the opening of a new tab.
"View", "_blank")
```

### Custom Action Save and Return
The easiest and most limited method is to use a "Save and Navigate" custom action. The instructions to create this are as follows:
1. Navigate to the Custom Actions tab.
2. Clone the Save Action.
3. Update the cloned action with a navigation URL.
4. Save and activate the custom action.

## Conclusion

Providing links directly to and from the Quote Line Editor in Salesforce can significantly improve the user experience and productivity. Whether you're simplifying access to the QLE or enabling users to navigate to related records, these techniques can help streamline your sales processes.
