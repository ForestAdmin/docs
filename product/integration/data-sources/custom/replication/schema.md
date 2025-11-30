In all the examples that we've been seeing in the previous sections, we've been using schema autodiscovery.

# Schema auto-discovery

When using the "replication" strategy, the agent needs to know the structure of the data that is being imported.

This is necessary to be able to provide a GUI that is consistent with the data.

When no schema is provided, the agent will try to auto-discover it by looking at the data that is being imported.

This comes with a few limitations:

- Empty collections cannot be imported.
- This has a performance cost, as the agent will need to fetch a significant sample of each collection to be able to discover the schema.
- The primary key of records must be named `id`.
- Composite primary keys are not supported.
- Foreign keys are not automatically detected, hence the GUI will not be able to display relationships between records unless they are [explicitly defined during customization](../../../agent-customization/relationships/README.md).

# Providing a schema

To avoid the limitations of schema auto-discovery, you can provide a schema to the agent.

This is done by providing a `schema` option to the `createReplicaDataSource` function.

## Example

Let's imagine that we are replicating data from an API with a single `books` collection.

The schema can be provided as follows:

```javascript
const myCustomDataSource = createReplicaDataSource({
  // `schema` is an array of collection definitions
  // If the schema needs to be dynamically generated, you can provide a Promise or
  // an async function that returns the schema.
  schema: [
    {
      name: 'books',
      fields: {
        id: { type: 'Integer', isPrimaryKey: true },
        title: { type: 'String' },
      },
    },
  ],

  // ... Implement record synchronization as explained in the previous sections
});
```

## Schema syntax

The schema is an array of collection definitions.

If it needs to be dynamically generated, you can provide a Promise or an async function that returns the schema.

<details>
<summary>Collection Definition</summary>

```javascript
{
  // name of the collection
  name: 'books',

  // fields of the collection
  fields: {
    id: { /* Field Definition */ },
    title: { /* Field Definition */ },

    // Nested object
    editor: {
      name: { /* Field Definition */ },
      address: { /* Field Definition */ },
    },

    // Array of values
    tags: [{ /* Field Definition or Nested object */ }],
  }
}
```

</details>
<details>
<summary>Field Definition</summary>

All fields are optional, except for the `type` field.

The documentation for field validation can be found [here](../../../agent-customization/fields/validation.md).

```javascript
{
  // Type of the field. Can be any of the following:
  // - Boolean, Integer, Number, String
  // - Date, Dateonly, Timeonly
  // - Binary, Enum, Json, Point, Uuid
  type: 'Enum';

  // Default value of the field when creating a new record (will show up in the UI)
  defaultValue: 1,

  // List of possible values for the field.
  // This can only be used when type == 'Enum' and can only contain strings.
  enumValues: ['foo_1', 'foo_2', 'foo_3', 'not_foo'],

  // Whether the field is the primary key of the collection.
  // (Multiple fields can be marked as primary keys)
  isPrimaryKey: false,

  // Whether the field is read-only (will show up in the UI).
  isReadOnly: false;

  // Whether the field has a unique constraint.
  // This must be set on unique foreign keys so that Forest Admin can know if
  // a relationship is a one-to-one or a one-to-many.
  unique: false,

  // An array of validation rule (see documentation note above)
  validation: [{operator: 'StartsWith', value: 'foo'}],

  // Whether the field is a foreign key.
  // If this is set, a relation will automatically be created between this collection
  // and the target collection.
  reference: {
    // Name that will be used to identify the relation in the code and UI.
    // This should be a singular name.
    relationName: 'name_of_the_relation',

    // Name that will be used to identify the inverse relation in the code and UI.
    // This should be a plural name for one-to-many relationships and singular
    // for one-to-one.
    relationInverse: 'name_of_relation_inverse',

    // Name of the target collection.
    targetCollection: 'name_of_the_target_collection',

    // 99% of the time, this will be the primary key of the target collection.
    // If not, you can specify the name of the target field.
    targetField: 'id',
  };
}
```

</details>

# Handling arrays and nested fields

To provide a GUI that is simple to use, Forest Admin makes wide use of tables.
This causes limitations when it comes to displaying deeply nested data.

- References to other collections must be at the root of the record, and cannot be nested.
- If using deeply nested records, those will be displayed in a JSON editor under [certain conditions](../../../under-the-hood/data-model/typing.md#composite-types).

## Flatten mode

To work around those limitations, the agent can transform the records using the `flattenMode` and `flattenOptions` options.

This feature is disabled by default and can be enabled by setting `flattenMode` to `auto` or `manual`.

The working principle is the same as with the configuration exposed by the [Mongoose driver](../../provided/mongoose.md#dealing-with-deeply-nested-models)

## Example

Let's imagine that we are replicating data from an API with two collections: `books` and `authors`.
Each book can have multiple authors, and each author can have multiple books.

The schema that should be provided is the schema of the target API, without any modification: it should match the records that are being imported.

```javascript
const myCustomDataSource = createReplicaDataSource({
  // Set the flatten mode to mode 'auto'
  // This will automatically flatten all records that contain nested fields or arrays
  // and create virtual collections
  flattenMode: 'auto',

  // Describe the schema
  schema: [
    {
      name: 'authors',
      fields: {
        id: { type: 'Integer', isPrimaryKey: true },
        name: { type: 'String' },
      },
    },
    {
      name: 'books',
      fields: {
        id: { type: 'Integer', isPrimaryKey: true },
        title: { type: 'String' },
        editor: {
          name: { type: 'String' },
          address: { type: 'String' },
        },
        authors: [
          {
            type: 'Integer',
            reference: {
              targetCollection: 'authors',
              relationName: 'authors',
              targetField: 'id',
            },
          },
        ],
      },
    },
  ],

  // ... Implement record synchronization as explained in the previous sections
});
```

With that configuration, the records that are imported will be modified as follows before being imported into the cache.

Note that in the handlers used for the configuration of your data source, you will still receive the original records and not the flattened ones.

<table><tr><td valign="top">

```javascript
{
  collection: 'books',
  record: {
    id: 'book:1',
    title: 'The Lord of the Rings',
    editor: {
      name: 'Allen & Unwin',
      address: 'London',
    },
    authors: ['author:1'],
  }
}
```

</td><td valign="top">

```javascript
{
  collection: 'books',
  record: {
    id: 1,
    title: 'The Lord of the Rings',

    // @@@ is used as separator
    editor@@@name: 'Allen & Unwin',
    editor@@@address: 'London',
  }
}
{
  collection: 'books_authors',
  record: {
    // id made from the primary key
    // of the parent record and the
    // name of the field
    _fid: 'book:1.authors',

    // Foreign key to the parent record
    _fpid: 'book:1',

    // Foreign key to the target record
    value: 'author:1'
  }
}
```

</td></tr></table>
