{% tabs %} {% tab title="agent.js" %}

The MongoDB data source allows importing collections from a MongoDB datasource, without the need for an ORM like Mongoose

To make everything work as expected, you need to install the package `@forestadmin/datasource-mongo`.

then, you can declare your datasource like so:

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createMongoDataSource } = require('@forestadmin/datasource-mongo');

// Create agent and import collections from mongoose.connection
const agent = createAgent(options).addDataSource(
  createMongoDataSource({
    uri: 'mongodb://localhost:27017',
    dataSource: { flattenMode: 'auto' },
  }),
);
```

## Introspection

Unlike other (SQL or ORM) datasources, MongoDB does not necessarily have a defined schema structuring the data. Therefore, at startup the agent analyses a sample of documents that live in your database to determine its structure. This process is called introspection.
You are able to control how many samples are taken from your database to perform the introspection

```javascript
  createMongoDataSource({
    uri: 'mongodb://localhost:27017',
    introspection: {
      collectionSampleSize: 100, // how many documents are sampled per collection
      referenceSampleSize: 10, // how many references are taken to deduce relationships
      maxPropertiesPerObject: 30, // how many properties should be extracted out as columns
    },
    dataSource: { flattenMode: 'auto' },
  }),
```

{% hint style="warning" %}
Settings those values too high might make your agent slower to startup
{% endhint %}

If the introspection returns some collections that you do not want to see, you may use the same syntax as for all the other datasources to remove them:

```javascript
const agent = createAgent(options).addDataSource(
  createMongoDataSource({ uri: connectionString }),
  { exclude: ['accounts', 'accounts_bills', 'accounts_bills_items'] },
);
```

## Flattening

MongoDB stores its data as BSON documents which can be nested. This format looks great on Forest Admin by default, with correct indentation, but you might also want to flatten it into dedicated rows to improve the presentation.

The `datasource-mongo` uses the same flattening options as the Forest Admin Mongoose datasource, which means that you can control how your nested models should be flattened using [the same syntax](./mongoose.md#dealing-with-deeply-nested-models)

example in manual mode:

```javascript
const agent = createAgent(options).addDataSource(
  createMongoDataSource({
    uri: connectionString,
    dataSource: {
      flattenMode: 'manual',
      flattenOptions: {
        // name of the Mongoose model
        persons: {
          // paths that should be converted to independent collections
          asModels: ['bills'],

          // paths that should be moved to the root of the collection
          asFields: ['address'],
          // or
          // asFields: ['address.streetName', 'address.city', 'address.country'],
          // or
          // asFields: [{ field: 'address', level: 1 }],
        },
      },
    },
  }),
);
```

## Data navigation

When customizing your agent, you will need to navigate through your data efficiently.
This section explains how to structure paths based on different field types.

### Separator behavior

There are two different separators depending on the field type:

- **Nested Fields (`@@@`)** – Used when accessing a nested field.
- **Related Data (`:`)** – Used when accessing related data.

### Example

Consider a `User` collection with an `address` relation that contains a `city` field,
which itself has a nested `name` field.

`User (collection) →  address (relation) →  city (field) →  name (nested field)`

To access the `name` field inside `city`, the path would be:

**`address:city@@@name`**

This structure ensures a clear distinction between relationships and nested properties.
