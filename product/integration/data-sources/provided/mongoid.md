The Mongoid data source allows importing collections from a Mongoid instance gem.

To make everything work as expected, you need to install the package `"forest_admin_datasource_mongoid", ">= 1.0.0"`.

```ruby
module ForestAdminRails
  class CreateAgent
    def self.setup!
      datasource = ForestAdminDatasourceMongoid::Datasource.new(
        options: {
          flatten_mode: 'auto',
        }
      )
      @create_agent = ForestAdminAgent::Builder::AgentFactory.instance.add_datasource(datasource, {})
      customize
      @create_agent.build
    end
  end
end
```

## Dealing with deeply nested models

![One Mongoose collection split into four Forest-Admin collections](../../assets/datasource-mongo.png)

When using `flatten_mode: 'none'` with the Mongoid data source, each Mongoid model is mapped directly to a single Forest Admin collection without any structural transformations.

Since Mongoid allows deeply nested embedded documents, this behavior may not always be ideal:

Embedded documents (embeds_one, embeds_many) will not be treated as separate collections in Forest Admin. Instead, they will be displayed as raw JSON inside the parent document.
Relations between embedded documents and their parent will not be visible as structured relationships.
Forest Admin will use a JSON editor to display and modify embedded fields, rather than providing dedicated collections for them.

### Understanding `flattenMode`

|                         | Description                                                                                                                                                                                                                                                       |
| ----------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `flattenMode: 'auto'`   | Automatic conversion: Embedded documents (embeds_one, embeds_many) are turned into separate collections in Forest Admin. This aligns with Mongoid’s approach, where embedded documents are tightly coupled to their parent but still modeled as distinct objects. |
| `flattenMode: 'manual'` | You are in full control on which virtual collections are created, and which fields are moved                                                                                                                                                                      |
| `flattenMode: 'none'`   | No transformation: Mongoid models are displayed as-is, with embedded objects appearing as raw JSON instead of separate collections.                                                                                                                               |
| not defined             | Equivalent to `auto` but a warning is displayed at server startup                                                                                                                                                                                                 |

### Example

Supposing that `persons` records have the following format:

```json
{
  "name": "Sherlock Holmes",
  "age": 54,
  "address": {
    "street_name": "Baker Street",
    "city": "London",
    "country": "Great Britain"
  },
  "bills": [
    {
      "title": "Rent",
      "amount": 0,
      "issue_date": "1887-04-17",
      "payed_by": ["Sherlock", "John", "Mrs Hudson"]
    }
  ]
}
```

With `flatten_mode: 'auto'`, Forest Admin will automatically generate two separate collections:

1. `persons`

   - fields: `name`, `age`, `address->street_name`, `address->city`, `address->country`.
   - the `address` fields are flattened into separate attributes of `persons`

2. `persons_bills`
   - fields: `title`, `amount`, `issue_date`, `payed_by`
   - the `bills` array is extracted as a relation, where each bill is a separate record

```javascript
// Automatic mode
const dataSource = createMongooseDataSource(mongoose.connection, {
  flattenMode: 'auto',
});

// Manual mode
const dataSource = createMongooseDataSource(mongoose.connection, {
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
});
```

```ruby
# Automatic mode
datasource = ForestAdminDatasourceMongoid::Datasource.new(
  options: {
    flatten_mode: 'auto',
  }
)

# Manual mode
datasource = ForestAdminDatasourceMongoid::Datasource.new(
  options: {
    flatten_mode: 'manual',
    flatten_options: {
      # name of the Mongoid model
      person: {
        # paths that should be converted to independent collections
        as_models: ['bills'],

        # paths that should be moved to the root of the collection
        as_fields: ['address'],
        # or
        # as_fields: ['address.street_name', 'address.city', 'address.country'],
        # or
        # as_fields: [{ field: 'address', level: 1 }],
      },
    },
  }
)
```

## Data Navigation

When customizing your agent, you need a consistent way to navigate through your data.  
This section explains how to structure paths based on different field types.

### Separator Behavior

Forest Admin uses two different separators depending on the field type:

- **Nested Fields (`@@@`)** – Used when accessing a nested field.
- **Related Data (`:`)** – Used when accessing related data.

### Example

Consider a `User` collection with an `address` relation that contains a `city` field,
which itself has a nested `name` field.

`User (collection) →  address (relation) →  city (field) →  name (nested field)`

To access the `name` field inside `city`, the path would be:

**`address:city@@@name`**

This structure ensures a clear distinction between relationships (`:`) and nested properties (`@@@`), allowing for precise and predictable data navigation
