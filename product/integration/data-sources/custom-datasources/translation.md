---
title: "Translation"
description: "Build custom datasources by translating Forest Admin queries into your API's query language"
---

{% hint style="warning" %}
Translation datasources require advanced knowledge of Forest Admin's query interface.
{% endhint %}

The translation strategy is an advanced approach for creating custom datasources that involves translating Forest Admin's query interface into the target API's query language.

## Overview

A full-featured query translation module typically exceeds 1000 lines of code. This approach suits full-featured databases and requires deep understanding of Forest Admin's internals.

### Key steps

Implementing this strategy requires completing three main phases:

1. **Structure Declaration** - Define the data structure
2. **Capabilities Declaration** - Specify API capabilities
3. **Translation Layer Implementation** - Code the actual query translation

### Minimal example

{% tabs %}
{% tab title="Node.js" %}
```javascript
class MyCollection extends BaseCollection {
  constructor(dataSource) {
    super('myCollection', dataSource);
    // Add fields with type, filtering, and sorting capabilities
  }

  async list(caller, filter, projection) {
    // Translate Forest Admin query to API format
    const params = QueryGenerator.generateListQueryString(filter, projection);
    const response = axios.get('https://my-api/my-collection', { params });
    return response.body.items;
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MyCollection < ForestAdminDatasourceToolkit::Collection
  def initialize(datasource)
    super(datasource, 'MyCollection')
    # Add fields with type, filtering, and sorting capabilities
  end

  def list(caller, filter, projection)
    # Translate Forest Admin query to API format
    params = QueryGenerator.generate_list_query_string(filter, projection)
    response = HTTParty.get('https://my-api/my-collection', query: params)
    response.parsed_response['items']
  end
end
```
{% endtab %}
{% endtabs %}

## Structure Declaration

### Columns

Define fields with types, validation, and default values:

{% tabs %}
{% tab title="Node.js" %}
```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');

class MovieCollection extends BaseCollection {
  constructor() {
    // [...]

    this.addField('id', {
      type: 'Column',
      columnType: 'Number',
      isPrimaryKey: true,
    });

    this.addField('title', {
      type: 'Column',
      columnType: 'String',
      validation: [{ operator: 'Present' }],
    });

    this.addField('mpa_rating', {
      type: 'Column',
      columnType: 'Enum',
      enumValues: ['G', 'PG', 'PG-13', 'R', 'NC-17'],
      defaultValue: 'G',
    });

    this.addField('stars', {
      type: 'Column',
      columnType: [{ firstName: 'String', lastName: 'String' }],
    });
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MovieCollection < ForestAdminDatasourceToolkit::Collection
  include ForestAdminDatasourceToolkit::Schema

  def initialize(datasource)
    super(datasource, 'Movie')

    add_field('id', ColumnSchema.new(
      column_type: 'Number',
      is_primary_key: true
    ))

    add_field('title', ColumnSchema.new(
      column_type: 'String',
      filter_operators: [Operators::PRESENT]
    ))

    add_field('mpa_rating', ColumnSchema.new(
      column_type: 'Enum',
      enum_values: %w[G PG PG-13 R NC-17],
      default_value: 'G'
    ))

    add_field('stars', ColumnSchema.new(
      column_type: [
        {
          'firstName' => 'String',
          'lastName' => 'String'
        }
      ]
    ))
  end
end
```
{% endtab %}
{% endtabs %}

### Typing

The typing system for columns is the same as the one used when declaring fields in the agent customization step.

### Validation

Forest Admin permits declaring validation rules on primitive-type fields. These rules validate records during creation/updating in the admin interface.

The validation API mirrors the condition tree structure but excludes a "field" entry. Example validation clause:

```json
{
  "aggregator": "and",
  "conditions": [
    { "operator": "present" },
    { "operator": "like", "value": "found%" },
    { "operator": "today" }
  ]
}
```

### Relationships

**Important:** Only intra-datasource relationships belong at the collection level. For inter-datasource relationships, use jointures during customization.

Data sources using the query translation strategy require careful implementation for relationships.

{% tabs %}
{% tab title="Node.js" %}
```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');

class MovieCollection extends BaseCollection {
  constructor() {
    // [...]

    this.addField('director', {
      type: 'ManyToOne',
      foreignCollection: 'people',
      foreignKey: 'directorId',
      foreignKeyTarget: 'id',
    });

    this.addField('actors', {
      type: 'ManyToMany',
      foreignCollection: 'people',
      throughCollection: 'actorsOnMovies',

      originKey: 'movieId',
      originKeyTarget: 'id',
      foreignKey: 'actorId',
      foreignKeyTarget: 'id',
    });
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MovieCollection < ForestAdminDatasourceToolkit::Collection
  def initialize(datasource)
    super(datasource, 'Movie')

    add_field('director', ManyToOneSchema.new(
      foreign_key: 'director_id',
      foreign_key_target: 'id',
      foreign_collection: 'People'
    ))

    add_field('actors', ManyToManySchema.new(
      origin_key: 'movie_id',
      origin_key_target: 'id',
      foreign_key: 'actor_id',
      foreign_key_target: 'id',
      foreign_collection: 'People',
      through_collection: 'ActorsOnMovies'
    ))
  end
end
```
{% endtab %}
{% endtabs %}

## Capabilities Declaration

Data source implementers don't need to translate every possible query type. Forest Admin ensures only supported query features are available by having collections declare capabilities on construction.

### Required features

All datasources must support:
- Listing records
- `And` nodes in condition trees
- `Or` nodes in condition trees
- `Equal` operator on primary keys
- Paging (`skip`, `limit`)

**Note:** Translating the `Or` node is a strong constraint, as many backends will not allow it: providing a working implementation may require making multiple queries and recombining the results.

### Optional features (opt-in)

| Unlocked Feature | Required Capabilities |
|---|---|
| Pagination page count display | Count |
| Charts | All field support in Aggregation |
| Relations | `In` on primary/foreign keys |
| Select all for actions/delete | `In` and `NotIn` on primary key |
| Frontend filters, scopes, segments | Per-field operator support |
| Operator emulation | `In` on primary keys |
| Search emulation | `Contains` on strings; `Equal` on numbers/UUIDs/enums |

### UI filter requirements by field type

To unlock GUI filtering:

- **Boolean:** `Equal`, `NotEqual`, `Present`, `Blank`
- **Date:** All date operators
- **Enum:** `Equal`, `NotEqual`, `Present`, `Blank`, `In`
- **Number:** `Equal`, `NotEqual`, `Present`, `Blank`, `In`, `GreaterThan`, `LessThan`
- **String:** `Equal`, `NotEqual`, `Present`, `Blank`, `In`, `StartsWith`, `EndsWith`, `Contains`, `NotContains`
- **UUID:** `Equal`, `NotEqual`, `Present`, `Blank`

### Collection-level capabilities

#### Count

Enables pagination widget to display total page count. Requires implementing the `aggregate` method:

{% tabs %}
{% tab title="Node.js" %}
```javascript
class MyCollection extends BaseCollection {
  constructor() {
    this.enableCount();
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MyCollection < ForestAdminDatasourceToolkit::Collection
  def initialize
    # [...]
    enable_count
  end
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
class MyCollection extends BaseCollection {
  constructor() {
    this.enableSearch();
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MyCollection < ForestAdminDatasourceToolkit::Collection
  def initialize
    # [...]
    enable_search
  end
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
class MyCollection extends BaseCollection {
  constructor() {
    this.addSegments(['Active records', 'Deleted records']);
    // All filter-accepting methods MUST handle segment fields
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MyCollection < ForestAdminDatasourceToolkit::Collection
  def initialize
    # [...]
    add_segments(['Active records', 'Deleted records'])
    # All filter-accepting methods MUST handle segment fields
  end
end
```
{% endtab %}
{% endtabs %}

### Field-level capabilities

#### Write support

Mark fields as read-only:

{% tabs %}
{% tab title="Node.js" %}
```javascript
this.addField('id', {
  isReadOnly: true,
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
add_field('id', {
  is_read_only: true
})
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
this.addField('id', {
  filterOperators: new Set([
    'Equal',
    // additional operators
  ]),
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
add_field('id', {
  filter_operators: ['Equal']
})
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
this.addField('id', {
  isSortable: true,
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
add_field('id', {
  is_sortable: true
})
```
{% endtab %}
{% endtabs %}

## Read implementation

### Emulation strategy

Emulation enables rapid development by allowing features to be tested in Node.js before optimization. This approach trades performance for faster iteration.

### Basic list implementation

{% tabs %}
{% tab title="Node.js" %}
```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');
const axios = require('axios');

class MyCollection extends BaseCollection {
  async list(caller, filter, projection) {
    // Fetch all records
    const response = await axios.get('https://my-api/my-collection');
    const result = response.data.items;

    // Apply in-process emulation
    if (filter.conditionTree)
      result = filter.conditionTree.apply(result, this, caller.timezone);
    if (filter.sort) result = filter.sort.apply(result);
    if (filter.page) result = filter.page.apply(result);

    return projection.apply(result);
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MyCollection < ForestAdminDatasourceToolkit::Collection
  def list(caller, filter, projection)
    # Fetch all records on all requests (inefficient approach)
    response = HTTParty.get('https://my-api/my-collection')
    result = response.parsed_response['items']

    # Apply filtering, sorting, pagination sequentially
    result = filter.condition_tree.apply(result, self, caller.timezone) if filter.condition_tree
    result = filter.sort.apply(result) if filter.sort
    result = filter.page.apply(result) if filter.page

    # Handle unsupported operations
    raise 'Unsupported feature' if filter.segment || filter.search

    projection.apply(result)
  end
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
async aggregate(caller, filter, aggregation, limit) {
  const records = await this.list(caller, filter, aggregation.projection);
  return aggregation.apply(records, caller.timezone, limit);
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
def aggregate(caller, filter, aggregation, limit = nil)
  records = list(caller, filter, aggregation.projection)
  aggregation.apply(records, caller.timezone, limit)
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
async aggregate(caller, filter, aggregation, limit) {
  if (aggregation.operation === 'Count' && aggregation.groups.length === 0) {
    return [{ value: await this.count(caller, filter) }];
  }
  // Handle general case
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
def aggregate(caller, filter, aggregation, limit = nil)
  # Optimize count-only queries
  if aggregation.operation == 'Count' && aggregation.groups.empty? && !aggregation.field
    return [{ 'value' => count(caller, filter) }]
  end
  # Handle general aggregation case
end
```
{% endtab %}
{% endtabs %}

## Write implementation

Making your records editable is achieved by implementing the `create`, `update` and `delete` methods.

**Important:** The three write methods accept filter parameters, but unlike the `list` method, pagination support is unnecessary.

{% tabs %}
{% tab title="Node.js" %}
```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');
const axios = require('axios'); // client for the target API

/** Naive implementation of create, update and delete on a REST API */
class MyCollection extends BaseCollection {
  constructor() {
    this.addField('id', { /* ... */ isReadOnly: true });
    this.addField('title', { /* ... */ isReadOnly: false });
  }

  async create(caller, records) {
    const promises = records.map(async record => {
      const response = await axios.post('https://my-api/my-collection', record);
      return response.data;
    });

    return Promise.all(promises); // Must return newly created records
  }

  async update(caller, filter, patch) {
    const recordIds = await this.list(caller, filter, ['id']); // Retrieve ids
    const promises = recordIds.map(async ({ id }) => {
      await axios.patch(`https://my-api/my-collection/${id}`, patch);
    });

    await Promise.all(promises);
  }

  async delete(caller, filter) {
    const recordIds = await this.list(caller, filter, ['id']); // Retrieve ids
    const promises = recordIds.map(async ({ id }) => {
      await axios.delete(`https://my-api/my-collection/${id}`);
    });

    await Promise.all(promises);
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MyCollection < ForestAdminDatasourceToolkit::Collection
  include ForestAdminDatasourceToolkit::Schema

  def initialize(datasource)
    super(datasource, 'MyCollection')
    add_field('id', ColumnSchema.new(is_read_only: true))
    add_field('title', ColumnSchema.new(is_read_only: false))
  end

  def create(caller, records)
    records.map do |record|
      response = HTTParty.post('https://my-api/my-collection', body: record.to_json)
      response.parsed_response
    end
  end

  def update(caller, filter, patch)
    record_ids = list(caller, filter, ForestAdminDatasourceToolkit::Components::Query::Projection.new(['id']))
    record_ids.each do |record|
      HTTParty.patch("https://my-api/my-collection/#{record['id']}", body: patch.to_json)
    end
  end

  def delete(caller, filter)
    record_ids = list(caller, filter, ForestAdminDatasourceToolkit::Components::Query::Projection.new(['id']))
    record_ids.each do |record|
      HTTParty.delete("https://my-api/my-collection/#{record['id']}")
    end
  end
end
```
{% endtab %}
{% endtabs %}

### Method details

- **create()**: Must return the newly created records with all fields populated
- **update()**: Receives filter and patch object; updates matching records
- **delete()**: Receives filter; deletes all matching records

## Intra-datasource relationships

When building custom datasources using the translation strategy, collections must handle intra-datasource relationships that are declared in their structure.

### Relationship types and requirements

**Automatic handling:**
- `one-to-many` relationships
- `many-to-many` relationships

For these types, Forest Admin will automatically call the destination collection with a valid filter, requiring no additional implementation work.

**Manual implementation required:**
- `many-to-one` relationships
- `one-to-one` relationships

These require developers to make all fields from the target collection available on the source collection (under a prefix).

### Handling prefixed fields

When a `many-to-one` relationship exists, the collection must accept references using dot notation throughout its operations.

#### Structure declaration example

{% tabs %}
{% tab title="Node.js" %}
```javascript
class MovieCollection extends BaseCollection {
  constructor() {
    super('movies', null);

    this.addField('director', {
      type: 'ManyToOne',
      foreignCollection: 'people',
      foreignKey: 'directorId',
      foreignKeyTarget: 'id',
    });
  }
}
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class MovieCollection < ForestAdminDatasourceToolkit::Collection
  def initialize(datasource)
    super(datasource, 'Movie')

    add_field('director', ManyToOneSchema.new(
      foreign_key: 'director_id',
      foreign_key_target: 'id',
      foreign_collection: 'People'
    ))
  end
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
await dataSource.getCollection('movies').list(
  caller,
  {
    conditionTree: {
      aggregator: 'And',
      conditions: [
        { field: 'title', operator: 'Equal', value: 'E.T.' },
        { field: 'director:firstName', operator: 'Equal', value: 'Steven' },
      ]
    },
    sort: [{ field: 'director:birthDate', ascending: true }]
  },
  ['id', 'title', 'director:firstName', 'director:lastName']
);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
datasource.collection('Movie').list(
  caller,
  ForestAdminDatasourceToolkit::Components::Query::Filter.new(
    condition_tree: ForestAdminDatasourceToolkit::Components::Query::ConditionTree::ConditionTreeBranch.new(
      aggregator: 'and',
      conditions: [
        ForestAdminDatasourceToolkit::Components::Query::ConditionTree::ConditionTreeLeaf.new(
          field: 'title',
          operator: 'Equal',
          value: 'E.T.'
        ),
        ForestAdminDatasourceToolkit::Components::Query::ConditionTree::ConditionTreeLeaf.new(
          field: 'director:firstName',
          operator: 'Equal',
          value: 'Steven'
        )
      ]
    ),
    sort: ForestAdminDatasourceToolkit::Components::Query::Sort.new(
      [{ field: 'director:birthDate', ascending: true }]
    )
  ),
  ForestAdminDatasourceToolkit::Components::Query::Projection.new(
    ['id', 'title', 'director:firstName', 'director:lastName']
  )
)
```
{% endtab %}
{% endtabs %}

#### Expected response structure

```json
{
  "id": 34,
  "title": "E.T",
  "director": { "firstName": "Steven", "lastName": "Spielberg" }
}
```

### Implementation scope

Developers implementing custom datasources must handle prefixed field references in:
- **Filters** (condition trees)
- **Projections** (field selections)
- **Aggregations** (calculation operations)

{% hint style="info" %}
Want to share your custom datasource with the community? Check out [Forest Admin experimental](/guides/forestadmin-experimental) to contribute.
{% endhint %}
