---
title: Smart Collections
---
### What is a Smart Collection?

A Smart Collection is a Forest Collection based on your API implementation. It allows you to reconcile fields of data coming from different or external sources in a single tabular view (by default), without having to physically store them into your database.

Fields of data could be coming from many other sources such as other B2B SaaS (e.g. Zendesk, Salesforce, Stripe), in-memory database, message broker, etc.

{% hint style="info" %}

This is an **advanced** notion. If you're just starting with Forest Admin, you should skip this for now.

{% endhint %}

In the following example, we have created a **Smart Collection** called `customer_stats`allowing us to see all customers who have placed orders, the number of order placed and the total amount of those orders.

**For an example of advanced customization and featuring an Amazon S3 integration,** you can see [here](examples/amazon-s3-integration-example.md) how we've stored in our live demo the companies' legal documents on Amazon S3 and how we've implemented a **Smart Collection** to access and manipulate them.

### Creating a Smart Collection

<Tabs>
  <Tab title="SQL (Sequelize)">
First, we declare the `customer_stats` collection in the `forest/` directory.

In this Smart Collection, we want to display for each customer its email address, the number of orders made (in a field `orders_count`) and the sum of the price of all those orders (in a field `total_amount`).

You can check out the list of [available field options ](../smart-fields/#available-field-options)if you need it.

{% hint style="warning" %}

You **MUST** declare an `id` field when creating a Smart Collection. The value of this field for each record **MUST** be unique.

As we are using the _customer id_ in this example, we do not need to declare an `id` manually.

{% endhint %}

```javascript
const { collection } = require('forest-express-sequelize');
const models = require('../models');

collection('customer_stats', {
  isSearchable: true,
  fields: [
    {
      field: 'email',
      type: 'String',
    },
    {
      field: 'orders_count',
      type: 'Number',
    },
    {
      field: 'total_amount',
      type: 'Number',
    },
  ],
});
```

{% hint style="info" %}

The option`isSearchable: true` added to your collection allows to display the search bar. Note that you will have to implement the search yourself by including it into your own `get` logic.

{% endhint %}
  </Tab>
  <Tab title="MongoDB (Mongoose)">
_Work in progress - this section will soon be released_
  </Tab>
</Tabs>

First, we declare the `CustomerStat` collection in the `lib/forest-liana/collections/` directory.

In this Smart Collection, we want to display for each customer its email address, the number of orders made (in a field `orders_count`) and the sum of the price of all those orders (in a field `total_amount`).

You can check out the list of [available field options ](../smart-fields/#available-field-options)if you need it.

{% hint style="warning" %}

You **MUST** declare an `id` field when creating a Smart Collection. The value of this field for each record **MUST** be unique.

As we are using the _customer id_ in this example, we do not need to declare an `id` manually.

{% endhint %}

ruby
