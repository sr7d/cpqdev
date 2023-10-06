---
layout: post
title: How to populate values across MDQ segments to reduce data entry
description: Managing multiple segments for subscription-based products can be complex and error-prone. In this article, we'll show you how to intelligently populate values across MDQ segments, reducing manual data entry, saving time, and improving accuracy.
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/MDQPopulateSegments.jpg
image_caption: 
tags: [multi-dimensional-quoting,code]
featured:
video_embed: 
---
# How to Populate Values Across MDQ Segments to Reduce Data Entry

In the world of Configure, Price, Quote (CPQ) and Multi-Dimensional Quoting (MDQ), managing multiple segments for a product can be a challenging task. With each segment representing a different term or dimension, data entry can become cumbersome, prone to errors, and time-consuming. Fortunately, there's a solution – you can conditionally populate values across segments to streamline the process and ensure accuracy. In this article, we'll show you how to make the most of MDQ by intelligently populating values across segments in the Quote Line Editor (QLE).

## Understanding MDQ Segments

Before we dive into the details, let's briefly explain what MDQ and segments are for those new to the concept:

- **MDQ (Multi-Dimensional Quoting)**: MDQ is a powerful feature in CPQ that allows you to configure and price subscription-based products across multiple dimensions or terms. This means you can define different pricing, quantities, and other attributes for each segment, such as a 1-year, 2-year, and 3-year term for a subscription product. [Learn more about MDQ](https://help.salesforce.com/s/articleView?id=sf.cpq_mdq_products.htm&type=5).

- **Segments**: In MDQ, segments are individual divisions or terms within a line item. For instance, if you're configuring a product with three different terms, you'll have three corresponding segments in the QLE.

## The Challenge of Data Entry

MDQ's flexibility in handling multiple segments is undoubtedly advantageous, but it can lead to challenges when it comes to data entry. Users may need to manually fill out various fields for each segment, which not only consumes time but also increases the risk of data entry errors.

## Conditionally Populating Values

To address this challenge, you can implement a solution that allows you to conditionally populate values across segments. This means that when a user enters values in specific segments, these values can automatically populate other segments, reducing the need for manual data entry.

### JavaScript Code Example

Here's a JavaScript code snippet to demonstrate how you can achieve this for Discounts:

```javascript
export function onAfterPriceRules(quoteModel, quoteLineModels) {
  console.log("onAfterPriceRules start");
  var linesMap = new Map();

  // Iterate over the quoteLineModels
  quoteLineModels.forEach(function (line) {
    var lineNumber = line.record["SBQQ__Number__c"];

    // Store values for SBQQ__Discount__c
    if (line.record["SBQQ__SegmentIndex__c"] === 1) {
      linesMap.set(lineNumber, {
        SBQQ__Discount__c: line.record["SBQQ__Discount__c"]
      });
      console.log(
        "Stored values for line " +
          lineNumber +
          ": " +
          "SBQQ__Discount__c: " +
          line.record["SBQQ__Discount__c"]
      );
    } else {
      if (linesMap.has(lineNumber)) {
        var values = linesMap.get(lineNumber);

        // Set values for SBQQ__Discount__c
        line.record["SBQQ__Discount__c"] = values.SBQQ__Discount__c;
        console.log(
          "Set values for line " +
            lineNumber +
            ": " +
            "SBQQ__Discount__c: " +
            values.SBQQ__Discount__c
        );
      }
    }
  });
  console.log("onAfterPriceRules end");
  return Promise.resolve();
}

```
By implementing such logic, you can create a more user-friendly and efficient data entry process in MDQ.

# Conclusion

Multi-Dimensional Quoting can be a game-changer for businesses dealing with subscription-based products. However, managing data entry across segments can be a daunting task. The key to streamlining this process is to conditionally populate values using JavaScript, reducing the manual effort required and minimizing the risk of errors.

With the techniques outlined in this article, you can harness the full power of MDQ while making the data entry process smoother for your users. By implementing these strategies, you'll save time, increase accuracy, and enhance the overall efficiency of your CPQ workflow.

For more tips and tricks on leveraging CPQ and MDQ for your business, stay tuned to our blog.

Disclaimer: This article provides a general overview of the concept of populating values across MDQ segments using JavaScript. Actual implementation may vary depending on your specific CPQ setup and requirements.


Feel free to use this markdown for your blog post, and make any additional adjustments or formatting changes as needed.
