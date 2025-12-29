---
title: Performance

description: >-
  Loading performance is key to streamlining your operations. Here are a few
  steps we recommend taking to ensure your Forest is optimized.
---
Please find here all the hands-on best practices to keep your admin panel performant. Depending on your user's needs, you might either hide or optimize some fields to limit the number of components, avoid a large datasets display or rework complex logic.

You can display bellow performances improvement tricks in [this video](https://www.youtube.com/watch?v=UC5nH8q5YUI). For any further help to improve admin panel performances, get in touch with [the community](https://community.forestadmin.com).

### Layout optimization

1\. Show only [Smart fields](https://docs.forestadmin.com/documentation/reference-guide/fields/create-and-manage-smart-fields) you absolutely need.

{% hint style="warning" %}
As you can see in the [Loading time benchmark](performance.md#loading-time-benchmark) below, Smart fields can be quite **costly** in terms of loading performance. Limiting them to those you need is key.
{% endhint %}

2\. Reduce the number of records per page

![](</images/legacy/javascript-agents/screenshot 2019-07-01 at 17.47.06.png>)

3\. Reduce the number of fields displayed

![](</images/legacy/javascript-agents/screenshot 2019-07-01 at 17.47.55 (1).png>)

{% hint style="info" %}
You can hide some fields in your table view; this will not prevent you from seeing them in the record details view.
{% endhint %}

Relationship fields are links to other collection records within your table view:

![](</images/legacy/javascript-agents/screenshot 2019-07-01 at 17.49.03.png>)

Having Relationship fields can decrease your performance, especially if your tables have a lot of records. Therefore you should display only those you need and use!

### Optimize smart fields performance

To optimize your smart field performances, please check out [this section](smart-fields/#createadvancedsmartfield).

### Restrict search on specific fields

Sometimes, searching in all fields is not relevant and may even result in big performance issues. You can restrict your search to specific fields only using the `searchFields` option.

<Tabs>
  <Tab title="SQL (Sequelize)">
In this example, we configure Forest Admin to only search on the fields `name` and `industry` of our collection `companies`.

```javascript
const { collection } = require('forest-express-sequelize');
​
collection('companies', {
  searchFields: ['name', 'industry'],
});
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
In this example, we configure Forest to only search on the fields `name` and `industry` of our collection `companies`.

```javascript
const { collection } = require('forest-express-mongoose');
​
collection('companies', {
  searchFields: ['name', 'industry'],
});
```
  </Tab>
</Tabs>

### Disable count queries

To disable the count request in the table of a relationship (Related data section):

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
router.get(
  '/books/:recordId/relationships/companies/count',
  deactivateCountMiddleware
);
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
router.get(
  '/books/:recordId/relationships/companies/count',
  deactivateCountMiddleware
);
```
  </Tab>
</Tabs>

You can also deactivate count queries conditionally based on the user's team:

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
router.get('/books/count', (request, response, next) => {
  // Count is deactivated for the Operations team
  if (request.user.team === 'Operations') {
    deactivateCountMiddleware(request, response);
    // Count is made for all other teams
  } else {
    next();
  }
});
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
router.get('/books/count', (request, response, next) => {
  // Count is deactivated for the Operations team
  if (request.user.team === 'Operations') {
    deactivateCountMiddleware(request, response);
    // Count is made for all other teams
  } else {
    next();
  }
});
```
  </Tab>
</Tabs>

### Database indexing

**Indexes** are a powerful tool used in the background of a database to speed up querying. It power queries by providing a method to quickly lookup the requested data. As Forest Admin generates SQL queries to fetch your data, creating indexes can improve the query response time.

5\. Index the Primary and Unique Key Columns

\
The syntax for creating an index will vary depending on the database. However, the syntax typically includes a `CREATE` keyword followed by the `INDEX` keyword and the name we’d like to use for the index. Next should come the `ON` keyword followed by the name of the table that has the data we’d like to quickly access. Finally, the last part of the statement should be the name(s) of the columns to be indexed.

```
CREATE INDEX <index_name>ON <table_name> (column1, column2, ...)
```

For example, if we would like to index phone numbers from a `customers` table, we could use the following statement:

```
CREATE INDEX customers_by_phoneON customers (phone_number)
```

The users cannot see the indexes, they are just used to speed up searches/queries.

6\. Index the Foreign Key Columns

Foreign key columns should be indexed if they are used intensively in Smart fields. In the table below, you can see how drastically it reduces the loading time of the page.

{% hint style="warning" %}
Updating a table with indexes takes more time than updating a table without (because the indexes also need an update). So, only create indexes on columns that will be frequently searched against.
{% endhint %}

### Loading time benchmark

Below is the outcome of a performance test on page load time of the Table view. It highlights the _importance_ of **using indexes** and **limiting the number of columns and lines**.

![](</images/legacy/javascript-agents/image (210).png>)
