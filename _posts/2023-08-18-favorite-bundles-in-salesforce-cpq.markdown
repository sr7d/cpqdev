---
layout: post
title: Templatize Quotes with Favorite Bundles in Salesforce CPQ
description: Create Collections of Favorite Products for Seamless Quoting. Discover how to streamline your quoting process and boost productivity with this powerful feature.
date: 2023-08-18 15:01:35 +0300
author: admin
image: /images/FavoriteGroups.jpg
image_caption: 
tags: [best-practices]
featured:
video_embed: 
---
# Add a Collection of Products Through Favorites

You're probably familiar with the concept of favoriting individual products to simplify your quoting process. But did you know that you can also create collections of favorite products and make them easily accessible in the Quote Line Editor (QLE)? In this blog post, we'll explore how you can leverage this powerful feature to enhance your CPQ experience.

## Why Create Collections of Favorite Products?

While favoriting individual products is a great way to speed up the quote creation process, it's not always efficient when you need to work with a set of related products, such as a product bundle or a package deal. This is where creating collections of favorite products comes into play.

Imagine you frequently offer a specific set of products together or want to streamline the process of adding related items to a quote. Instead of manually selecting each product every time, you can create a collection of these products and add them all to your quote with a two clicks. This can significantly improve your productivity and reduce the chances of errors when configuring complex quotes.

## How to Create a Collection of Favorite Products

Creating a collection of favorite products in Salesforce CPQ is a straightforward process. Here are the steps to follow:

1. **Access the Quote:** Start by opening the quote where you want to add the collection of favorite products.

2. **Add a Group:** Look for the "Add Group" button within the quote interface and click on it. This will create a new group for your favorite products.

3. **Add Products to the Group:** Once you've created the group, you can add products to it. Simply select the products you want to include in the collection and assign them to the group you just created.

4. **Favorite the Group:** Select the star icon (the favorite icon) associated with the group you've just created. Clicking on this icon will prompt you to name your favorites group.

5. **Name Your Favorites Group:** Give your collection of favorite products a meaningful name. This name will make it easy to identify and access your favorites in the QLE.

6. **Access Your Favorites in the QLE:** From now on, you can easily access this group of favorite products by its name in the Quote Line Editor (QLE). This means you can add all the products in the collection to your quote with just a few clicks.

## Sharing Favorites with Other Users

If you're an admin in your Salesforce instance, you have the ability to share these favorites with other users. This can be incredibly useful when multiple team members work on quotes and need access to the same set of favorite products. Here's how you can do it:

1. **Add a Record for Each Share:** As an admin, you'll need to add a record for each user you want to share the favorite group with. This record is named `SBQQ__FavoriteShare__c`.

2. **Grant Access to a User:** Once you've added the appropriate records, the selected users will have access to the favorite group in their QLE. This ensures that your team can work efficiently and consistently when creating quotes with shared collections of favorite products.

In conclusion, Salesforce CPQ offers a powerful feature that allows you to create collections of favorite products and streamline your quoting process. By following the steps outlined in this blog post, you can boost your productivity, reduce errors, and ensure that your team has access to the same set of favorite products when creating quotes. 
