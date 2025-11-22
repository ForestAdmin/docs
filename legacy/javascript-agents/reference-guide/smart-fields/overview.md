---
title: Smart Fields
---
### What is a Smart Field?

A field that displays a computed value in your collection.

<img src="/images/legacy/javascript-agents/the_smart_fields.jpg" alt="" />

A Smart Field is a column that displays processed-on-the-fly data. It can be as simple as concatenating attributes to make them human friendly, or more complex (e.g. total of orders).

### Creating a Smart Field

<Tabs>
  <Tab title="SQL (Sequelize)">
On our Live Demo, the very simple Smart Field `fullname` is available on the `customers` collection.

```javascript
const { collection } = require('forest-express-sequelize');

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
    },
  ],
});
```

\
Very often, the business logic behind the Smart Field is more complex and must be asynchronous. To do that, please have a look at [this section](./#createadvancedsmartfield).
  </Tab>
  <Tab title="MongoDB (Mongoose)">
On our Live Demo, the very simple Smart Field `fullname` is available on the `customers` collection.

```javascript
const { collection } = require('forest-express-mongoose');

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
    },
  ],
});
```

\
Very often, the business logic behind the Smart Field is more complex and must be asynchronous. To do that, please have a look at [this section](./#createadvancedsmartfield).
  </Tab>
</Tabs>

On our Live Demo, the very simple Smart Field `fullname` is available on the `Customer` collection.

Very often, the business logic behind the Smart Field is more complex and must interact with the database. Here’s an example with the Smart Field `full_address` on the `Customer` collection.

Ensure the file app/forest/\_\_init\_\_.py exists and contains the import of the previous defined class :

Very often, the business logic behind the Smart Field is more complex and must interact with the database. Here’s an example with the Smart Field `full_address` on the `Customer` model.

{% hint style="warning" %}

The collection name must be the same as the **model name**.

{% endhint %}

![](</images/legacy/javascript-agents/screenshot 2019-07-01 at 12.16.05.png>)

### Updating a Smart Field

<Tabs>
  <Tab title="SQL (Sequelize)">
By default, your Smart Field is considered as read-only. If you want to update a Smart Field, you just need to write the logic to “unzip” the data. **Note that the `set` method should always return the object it’s working on**. In the example hereunder, the `customer` object is returned including only the modified data.

```javascript
const { collection } = require('forest-express-sequelize');

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      set: (customer, fullname) => {
        let names = fullname.split(' ');
        customer.firstname = names[0];
        customer.lastname = names[1];

        // Don't forget to return the customer.
        return customer;
      },
    },
  ],
});
```

Working with the actual record can be done this way:

```javascript
const { collection, ResourceGetter } = require('forest-express-sequelize');
const { customers } = require('../models');

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      set: async (customer, fullname) => {
        const customerBeforeUpdate = await customers.findOne({
          where: { id: customer.id },
        });

        const names = fullname.split(' ');
        customer.firstname = `${names[0]} ${customerBeforeUpdate.pseudo}`;
        return customer;
      },
    },
  ],
});
```

{% hint style="info" %}

For security reasons, the `fullname` Smart field will remain **read-only**, even after you implement the `set` method. To edit it, disable read-only mode in the field settings.

{% endhint %}
  </Tab>
  <Tab title="MongoDB (Mongoose)">
By default, your Smart Field is considered as read-only. If you want to update a Smart Field, you just need to write the logic to “unzip” the data. **Note that the `set` method should always return the object it’s working on**. In the example hereunder, the `customer` record is returned.

```javascript
const { collection } = require('forest-express-mongoose');

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      set: (customer, fullname) => {
        let names = fullname.split(' ');
        customer.firstname = names[0];
        customer.lastname = names[1];

        // Don't forget to return the customer.
        return customer;
      },
    },
  ],
});
```

Working with the actual record can be done this way:

```javascript
const { collection, ResourceGetter } = require('forest-express-mongoose');
const { customers } = require('../models');

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      set: async (customer, fullname) => {
        const customerBeforeUpdate = await customers.findById(customer.id);

        const names = fullname.split(' ');
        customer.firstname = `${names[0]} ${customerBeforeUpdate.pseudo}`;
        return customer;
      },
    },
  ],
});
```

{% hint style="info" %}

For security reasons, the `fullname` Smart field will remain **read-only**, even after you implement the `set` method. To edit it, disable read-only mode in the field settings.

{% endhint %}
  </Tab>
</Tabs>

By default, your Smart Field is considered as read-only. If you want to update a Smart Field, you just need to write the logic to “unzip” the data. **Note that the set method should always return the object it’s working on**. In the example hereunder, the `user_params` is returned is returned including only the modified data.

{% hint style="info" %}

For security reasons, the `fullname` Smart field will remain **read-only**, even after you implement the `set` method. To edit it, disable read-only mode in the field settings.

{% endhint %}

### Searching, Sorting and Filtering on a Smart Field

To perform a search on a Smart Field, you also need to write the logic to “unzip” the data, then the search query which is specific to your zipping. In the example hereunder, the `firstname` and `lastname` are searched separately after having been unzipped.

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
const { collection } = require('forest-express-sequelize');
const models = require('../models/');
const _ = require('lodash');
const Op = models.objectMapping.Op;

collection('customers', {
  fields: [
    {
      field: 'fullname',
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      search: function (query, search) {
        let split = search.split(' ');

        var searchCondition = {
          [Op.and]: [
            { firstname: { [Op.like]: `%${split[0]}%` } },
            { lastname: { [Op.like]: `%${split[1]}%` } },
          ],
        };

        query.where[Op.and][0][Op.or].push(searchCondition);

        return query;
      },
    },
  ],
});
```

{% hint style="info" %}

For **case insensitive** search using PostgreSQL database use `iLike` operator. See [Sequelize operators documentation](https://sequelize.org/docs/v6/core-concepts/model-querying-basics/#operators).

{% endhint %}
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
const { collection } = require('forest-express-mongoose');
const models = require('../models/');
const _ = require('lodash');

collection('customers', {
  fields: [{
    field: 'fullname',
    type: 'String',
    get: (customer) => {
      return customer.firstname + ' ' + customer.lastname;
    },
    search(search) {
      let names = search.split(' ');
    ​
      return {
        firstname: names[0],
        lastname: names[1]
      };
    }
  }]
});
```
  </Tab>
</Tabs>

#### Filtering

{% hint style="warning" %}

This feature is only available on agents version **6.7+** (version **6.2+** for Rails).

{% endhint %}

To perform a filter on a Smart Field, you need to write the filter query logic, which is specific to your use case.

In the example hereunder, the `fullname` is filtered by checking conditions on the `firstname` and `lastname` depending on the filter operator selected.

### SQL

```javascript
const { collection } = require('forest-express-sequelize');
const models = require('../models/');

const { Op } = models.Sequelize;

collection('customers', {
  fields: [
    {
      field: 'fullname',
      isFilterable: true,
      type: 'String',
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      filter({ condition, where }) {
        const firstWord = !!condition.value && condition.value.split(' ')[0];
        const secondWord = !!condition.value && condition.value.split(' ')[1];

        switch (condition.operator) {
          case 'equal':
            return {
              [Op.and]: [
                { firstname: firstWord },
                { lastname: secondWord || '' },
              ],
            };
          case 'ends_with':
            if (!secondWord) {
              return {
                lastName: { [Op.like]: `%${firstWord}` },
              };
            }
            return {
              [Op.and]: [
                { firstName: { [Op.like]: `%${firstWord}` } },
                { lastName: secondWord },
              ],
            };

          // ... And so on with the other operators not_equal, starts_with, etc.

          default:
            return null;
        }
      },
    },
  ],
  segments: [],
});
```

### MongoDB

```javascript
const { collection } = require('forest-express-mongoose');
const models = require('../models');

collection('customer', {
  actions: [],
  fields: [
    {
      field: 'fullName',
      type: 'String',
      isFilterable: true,
      get: (customer) => {
        return customer.firstname + ' ' + customer.lastname;
      },
      filter({ condition, where }) {
        const firstWord = !!condition.value && condition.value.split(' ')[0];
        const secondWord = !!condition.value && condition.value.split(' ')[1];

        switch (condition.operator) {
          case 'equal':
            return {
              $and: [{ firstname: firstWord }, { lastname: secondWord || '' }],
            };
          case 'ends_with':
            if (!secondWord) {
              return {
                lastname: { $regex: `.*${firstWord}` },
              };
            }
            return {
              $and: [
                { firstname: { $regex: `.*${firstWord}` } },
                { lastname: secondWord },
              ],
            };

          // ... And so on with the other operators not_equal, starts_with, etc.

          default:
            return null;
        }
      },
    },
  ],
  segments: [],
});
```

{% hint style="info" %}

Make sure you set the option `isFilterable: true` in the field definition of your code. Then, you will be able to toggle the "Filtering enabled" option in the browser, in your **Fields Settings**.

{% endhint %}

![](</images/legacy/javascript-agents/image (405).png>)

#### Sorting

{% hint style="warning" %}

**Sorting** on a Smart Field is not _natively supported_ in Forest Admin. However you can check out those guides:

- [Sort by Smart field](smart-field-examples/sort-by-smart-field.md)
- [Sort by Smart field that includes value from a belongsTo relationship](smart-field-examples/sort-by-smart-field-that-includes-value-from-a-belongsto-relationship.md)

{% endhint %}

### Available Field Options

Here are the list of available options to customize your Smart Field:

| Name        | Type             | Description                                                                                                                    |
| ----------- | ---------------- | ------------------------------------------------------------------------------------------------------------------------------ |
| field       | string           | The name of your Smart Field.                                                                                                  |
| type        | string           | Type of your field. Can be `Boolean`, `Date`, `Json`,`Dateonly`, `Enum`, `File`, `Number, ['String']` or `String` .            |
| enums       | array of strings | (optional) Required only for the `Enum` type. This is where you list all the possible values for your input field.             |
| description | string           | (optional) Add a description to your field.                                                                                    |
| reference   | string           | (optional) Configure the Smart Field as a [Smart Relationship](../models/relationships/#what-is-a-smart-relationship).         |
| isReadOnly  | boolean          | (optional) If `true`, the Smart Field won’t be editable in the browser. Default is `true` if there’s no `set` option declared. |
| isRequired  | boolean          | (optional) If true, your Smart Field will be set as required in the browser. Default is false.                                 |

{% hint style="info" %}

You can define a widget for a smart field from the [settings of your collection](https://docs.forestadmin.com/user-guide/collections/customize-your-fields).

{% endhint %}

### Building Performant Smart Fields

To optimize your smart field performance, we recommend using a mechanism of batching and caching data requests.

Implement them using the DataLoader which is a generic utility to be used as part of your application's data fetching layer to provide a simplified and consistent API over various remote data sources.

#### Smart field declaration

### SQL

```javascript
const DataLoader = require('dataloader');

const authorLoader = new DataLoader(async (authorKeys) => {
  const authors = await users.findAll({
    where: { id: authorKeys },
  });

  const authorsById = new Map(authors.map((user) => [user.id, user]));

  return authorKeys.map((authorKey) => authorsById.get(authorKey));
});

collection('posts', {
  actions: [],
  fields: [
    {
      field: 'author_name',
      type: 'String',
      get: async (record) => {
        const author = await authorLoader.load(record.authorKey);

        return author.name;
      },
    },
  ],
  segments: [],
});
```

### MongoDB

```javascript
const { collection } = require('forest-express-mongoose');
const { Address } = require('../models');
const Dataloader = require('dataloader');

const addressLoader = new Dataloader((customerIds) => {
  const addresses = await models.addresses.find({
    customer_id: {
      $in: customerIds
    }
  });

  const addressesByCustomerId = new Map(addresses.map(
    address => [address.customer_id, address]
  ));

  return customerIds.map(customerId => addressesByCustomerId.get(customerId));
})

collection('customers', {
  fields: [{
    field: 'full_address',
    type: 'String',
    get: (customer) => {
      return addressLoader.load(customer.id)
        .then((address) => {
          return address.address_line_1 + '\n' +
            address.address_line_2 + '\n' +
            address.address_city + ' ' + address.country;
        });
    }
  }]
});
```

####
