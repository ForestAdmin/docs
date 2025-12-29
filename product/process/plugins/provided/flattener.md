Forest Admin user interface is designed with "flat" Collection structure in mind. This paradigm may not be the best fit for your data model if you are using a NoSQL database with nested documents.

In this case, you can use the provided flattener plugin to flatten your data model before sending it to Forest Admin.

## Installation

```bash
npm install @forestadmin/plugin-flattener
```

## Usage

Three methods are available to flatten your data model: `flattenColumn`, `flattenJsonColumn` and `flattenRelation`.

### `flattencolumn`

This method allows you to flatten deeply nested columns of your models.

Note that when using this method, the collection won't be filterable nor sortable on the target columns.

```javascript
const { flattenColumn } = require('@forestadmin/plugin-flattener');

agent.customizeCollection('customer', collection => {
  collection.use(flattenColumn, {
    // Name of the column to flatten into the root model
    columnName: 'address',

    // Subfields to flatten (defaults to all subfields up to the `level` setting)
    include: ['street', 'city', 'country:name', 'country:isoCode'],

    // Subfields to exclude from flattening (defaults to none)
    exclude: [],

    // When flattening a subfield, the plugin will flatten all subfields up to the
    // `level` setting.
    // Defaults to 1 and is ignored if `include` is specified.
    level: 1,

    // Either if the created columns should be readonly or not (defaults to false)
    readonly: true,
  });
});
```

### `flattenjsoncolumn`

{% hint style="warning" %}
Please note that this plugin is meant for SQL data sources with primitive JSON columns.
{% endhint %}

This method allows you to flatten deeply nested primitives JSON columns of your models.

Note that when using this method, the collection won't be filterable nor sortable on the target columns.

```javascript
const { flattenJsonColumn } = require('@forestadmin/plugin-flattener');

agent.customizeCollection('customer', collection => {
  collection.use(flattenJsonColumn, {
    // Name of the column to flatten into the root model
    columnName: 'address',

    // A json object representing a subset of the shape of the data in
    // the json column, with keys the column names, and values either json (to flatten further)
    // or the primitive type.
    columnType: {
      street: 'String',
      city: 'String',
      country: { name: 'String', isoCode: 'String' },
    },

    // Either if the created columns should be readonly or not (defaults to false)
    readonly: true,

    // Either if original column should be kept or not (defaults to false)
    keepOriginalColumn: true,
  });
});
```

### `flattenrelation`

This method allows you to import fields from a given [one-to-one](../../../agent-customization/relationships/single-record.md#one-to-one-relations) or [many-to-one](../../../agent-customization/relationships/single-record.md#many-to-one-relations) relations.

It is useful when you want to repatriate multiple fields inside one collection.

```javascript
const { flattenRelation } = require('@forestadmin/plugin-flattener');

agent.customizeCollection('customer', collection => {
  return collection.use(flattenRelation, {
    relationName: 'address',

    // Subfields to flatten (defaults to all subfields of the relation)
    include: ['street', 'city', 'country:name', 'country:isoCode'],

    // Subfields to exclude from flattening (defaults to an empty array)
    exclude: [],

    // Either if the created columns should be readonly or not (defaults to false)
    readonly: true,
  });
});
```

Note that nested relations are supported using the `:` separator.

```javascript
const { flattenRelation } = require('@forestadmin/plugin-flattener');

agent.customizeCollection('customer', collection => {
  return collection.use(flattenRelation, { relationName: 'address:country' });
});
```
