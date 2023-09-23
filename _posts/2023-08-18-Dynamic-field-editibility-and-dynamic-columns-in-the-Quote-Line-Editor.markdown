---
layout: post
title: Dynamic field editibility and dynamic columns in the Quote Line Editor
description: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Sociis natoque penatibus et magnis. Tincidunt eget nullam non nisi est sit.
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/DynamicFieldsandColumns.jpg
image_caption: 
tags: [Salesforce-CPQ,Dynamic-QLE-Columns,Page-Security-Plugin]
featured:
video_embed: 
---
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Sociis natoque penatibus et magnis. Tincidunt eget nullam non nisi est sit. Dictum at tempor commodo ullamcorper a lacus vestibulum sed arcu. Congue quisque egestas diam in arcu cursus. Felis imperdiet proin fermentum leo vel orci porta non pulvinar. Amet consectetur adipiscing elit pellentesque habitant. Adipiscing at in tellus integer feugiat scelerisque varius morbi enim. Dictumst quisque sagittis purus sit amet volutpat consequat mauris. In nibh mauris cursus mattis. Sollicitudin tempor id eu nisl nunc. Sed egestas egestas fringilla phasellus faucibus scelerisque.

Natoque penatibus et magnis dis parturient. Volutpat blandit aliquam etiam erat velit scelerisque in dictum non. Imperdiet massa tincidunt nunc pulvinar sapien et ligula ullamcorper malesuada. Vestibulum morbi blandit cursus risus at ultrices mi tempus. Eget est lorem ipsum dolor sit amet consectetur. Eu turpis egestas pretium aenean pharetra magna ac. Accumsan tortor posuere ac ut consequat semper viverra nam libero. Sit amet porttitor eget dolor morbi non. Tempor nec feugiat nisl pretium fusce. Malesuada bibendum arcu vitae elementum curabitur vitae nunc sed. Nulla facilisi nullam vehicula ipsum. Sollicitudin nibh sit amet commodo nulla. Cras tincidunt lobortis feugiat vivamus. Accumsan sit amet nulla facilisi morbi tempus iaculis. Tincidunt praesent semper feugiat nibh sed pulvinar proin. Ultricies mi eget mauris pharetra et ultrices neque ornare aenean. Odio facilisis mauris sit amet massa vitae tortor condimentum lacinia. Donec ultrices tincidunt arcu non sodales neque sodales. Maecenas sed enim ut sem viverra.

- one
- two
- three

1. one
2. two
3. three

```javascript
export function onAfterPriceRules(quoteModel, quoteLineModels) {
  console.log("onAfterPriceRules start");
  var linesMap = new Map();

  // Iterate over the quoteLineModels
  quoteLineModels.forEach(function (line) {
    var lineNumber = line.record["SBQQ__Number__c"];

    // Store values for SKU_Grouping__c, SBQQ__Discount__c, Metric__c, Unit__c, and Workiva_List_Price__c
    if (line.record["SBQQ__SegmentIndex__c"] === 1) {
      linesMap.set(lineNumber, {
        SBQQ__Discount__c: line.record["SBQQ__Discount__c"],
        SKU_Grouping__c: line.record["SKU_Grouping__c"],
        Metric__c: line.record["Metric__c"],
        Unit__c: line.record["Unit__c"],
        Workiva_List_Price__c: line.record["Workiva_List_Price__c"]
      });
      console.log(
        "Stored values for line " +
          lineNumber +
          ": " +
          "SBQQ__Discount__c: " +
          line.record["SBQQ__Discount__c"] +
          ", SKU_Grouping__c: " +
          line.record["SKU_Grouping__c"] +
          ", Metric__c: " +
          line.record["Metric__c"] +
          ", Unit__c: " +
          line.record["Unit__c"] +
          ", Workiva_List_Price__c: " +
          line.record["Workiva_List_Price__c"]
      );
    } else {
      if (linesMap.has(lineNumber)) {
        var values = linesMap.get(lineNumber);

        // Set values for SKU_Grouping__c, Metric__c, and Unit__c
        line.record["SBQQ__Discount__c"] = values.SBQQ__Discount__c;
        line.record["SKU_Grouping__c"] = values.SKU_Grouping__c;

        // Check if SKU_Grouping__c is 'Other' before setting Workiva_List_Price__c
        if (values.SKU_Grouping__c === "Other") {
          line.record["Workiva_List_Price__c"] = values.Workiva_List_Price__c;
        }

        // Check if Metric__c is populated before setting Unit__c
        if (values.Metric__c) {
          line.record["Unit__c"] = values.Unit__c;
        }
        console.log(
          "Set values for line " +
            lineNumber +
            ": " +
            "SBQQ__Discount__c: " +
            values.SBQQ__Discount__c +
            ", SKU_Grouping__c: " +
            values.SKU_Grouping__c +
            ", Metric__c: " +
            values.Metric__c +
            ", Unit__c: " +
            line.record["Unit__c"] +
            ", Workiva_List_Price__c: " +
            line.record["Workiva_List_Price__c"]
        );
      }
    }
  });
  console.log("onAfterPriceRules end");
  return Promise.resolve();
}

```


