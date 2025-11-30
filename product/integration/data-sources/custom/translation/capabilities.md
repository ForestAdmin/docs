As a data source implementer, **you won't have to translate every possible query**: on most data sources, it is **not feasible**, as you will be restricted by the API that you will be translating queries for.

On construction, each of your collections will declare per-field capabilities.

Forest Admin will then ensure that only queries using features that you have explicitly enabled are used.

# Required features

All data sources need to be able to:

- List records
- Understand `And` nodes in condition trees
- Understand `Or` nodes in conditions trees
- Understand the `Equal` operator on primary keys
- Understand paging (`skip`, `limit`)

{% hint style="warning" %}
Translating the `Or` node is a strong constraint, as many backends will not allow it: providing a working implementation may require making multiple queries and recombining the results.
{% endhint %}

# Optional features

All optional features are opt-in and need to be specified when constructing a data source so that Forest Admin understands that they are available.

## How to unlock features

The more complete your query translator is, the more features will be unlocked

| Unlocked feature                                             | Needed capabilities                                              |
| ------------------------------------------------------------ | ---------------------------------------------------------------- |
| Displaying the number of pages in pagination widget          | Count                                                            |
| Using charts                                                 | Support all field in `Aggregation`                               |
| Using relations                                              | `In` on primary keys and foreign keys                            |
| Using `select all` feature for actions, delete or dissociate | `In` and `NotIn` on the primary key                              |
| Frontend filters, scopes and segments                        | See below                                                        |
| Using operator emulation                                     | `In` on primary keys                                             |
| Using search emulation                                       | `Contains` on string fields, `Equal` on numbers, uuids and enums |

## Unlock filtering, scopes, and segments on the UI

Forest Admin UI implements filtering, scopes, and segments with a "per-field", not on a "per-field-and-operator" granularity.

This means that filtering for a given field is either enabled or not from the UI perspective. Forest Admin admin panel will enable the feature only once enough operators are supported depending on the type of the field.

| Field type | Needed operators to unlock GUI filters, scopes and segments                                       |
| ---------- | ------------------------------------------------------------------------------------------------- |
| Boolean    | `Equal`, `NotEqual`, `Present`, `Blank`                                                           |
| Date       | All dates operators                                                                               |
| Enum       | `Equal`, `NotEqual`, `Present`, `Blank`,`In`                                                      |
| Number     | `Equal`, `NotEqual`, `Present`, `Blank`,`In`, `GreaterThan`, `LessThan`                           |
| String     | `Equal`, `NotEqual`, `Present`, `Blank`,`In`, `StartsWith`, `EndsWith`, `Contains`, `NotContains` |
| Uuid       | `Equal`, `NotEqual`, `Present`, `Blank`                                                           |

# Collection level capabilities

## Count

![Collection Pagination element](../../../assets/customdatasource-count-capability.png)

Enabling this feature allows the pagination widget to display the total number of pages in a collection while browsing records.

Once enabled, your collections must implement the [`aggregate` method](../../getting-started/queries/aggregations.md).

```javascript
class MyCollection extends BaseCollection {
  constructor() {
    // [...]

    // If you have implemented the aggregate method
    this.enableCount();
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self):
        # [...]

        # If you have implemented the aggregate method
        self.enable_count()
```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      def initialize
        # [...]

        # If you have implemented the aggregate method
        enable_count
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        // [...]

        // If you have implemented the aggregate method
        $this->setCountable(true);
    }
}
```

## Search

{% hint style="info" %}
If this feature is not enabled in the data source definition, users of your data source can still use the search bar in their admin panel (Forest Admin will default to building condition trees).
{% endhint %}

Enabling this feature allows you to either:

- Gain control over how search works for your data source instead of relying on the default implementation
- Allow full-text search on data sources where the condition tree implementation is not strong enough

This is relevant mostly for data sources that target data sources that have native full-text search capabilities (ElasticSearch, ...)

```javascript
class MyCollection extends BaseCollection {
  constructor() {
    // [...]

    // If you want to implement search yourself
    this.enableSearch();
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self):
        # [...]

        # If you want to implement search yourself
        self.enable_search()
```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      def initialize
        # [...]

        # If you want to implement search yourself
        enable_search
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        // [...]

        // If you want to implement search yourself
        $this->setSearchable(true);
    }
}
```

## Segments

{% hint style="info" %}
If this feature is not enabled in the data source definition, users of your data source can still create segments in both their admin panels and agent customization.
{% endhint %}

Defining segments from your data sources can be relevant in 3 situations:

- Implementing segments in the data source can be more efficient than using the default condition tree-based segments (i.e. using complex SQL queries which cannot be expressed as a condition tree)
- The underlying data source has a concept that maps to Forest Admin segments (i.e. ["scopes" in Sequelize](https://sequelize.org/master/manual/scopes.html))
- Your data source is used in multiple Forest Admin projects, and the segment should be shared across all deployments (i.e. segments coming from a third-party SaaS)

```javascript
class MyCollection extends BaseCollection {
  constructor() {
    // [...]

    // If you want to implement segments at the data source level
    this.addSegments(['Active records', 'Deleted records']);

    // From now on, all methods that take a filter as parameter *MUST* not ignore
    // its segment field.
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self):
        # [...]

        # If you want to implement segments at the data source level
        self.enable_segments(['Active records', 'Deleted records'])

        # From now on, all methods that take a filter as parameter *MUST* not ignore
        # its segment field.
```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      def initialize
        # [...]

        # If you want to implement segments at the data source level
        add_segments(['Active records', 'Deleted records'])

        # From now on, all methods that take a filter as parameter *MUST* not ignore
        # its segment field.
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        // [...]

        // If you want to implement segments at the data source level
        $this->setSegments(['Active records', 'Deleted records']);

        // From now on, all methods that take a filter as parameter *MUST* not ignore
        // its segment field.
    }
}
```

# Field level capabilities

## Write support

Fields may or may not be writable. To make a readonly use the {{#nodejs,php}}`isReadOnly`{{/nodejs,php}}{{#python,ruby}}`is_read_only`{{/python,ruby}} flag.

```javascript
class MyCollection extends BaseCollection {
  constructor() {
    // [...]

    this.addField('id', {
      // [...]
      isReadOnly: true,
    });
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self):
      # [...]

      self.add_field('id', {
          # [...]
          "is_read_only": True,
      })

```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      def initialize
        # [...]

        add_field('id', {
            # [...]
            is_read_only: true,
        })
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;
use ForestAdmin\AgentPHP\DatasourceToolkit\Schema\ColumnSchema;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        // [...]
    }

    $this->addField('id', new ColumnSchema(
        // [...]
        isReadOnly: true,
    ));
}
```

## Filtering operators

When declaring a field, the {{#nodejs,php}}`filterOperators`{{/nodejs,php}}{{#python,ruby}}`filter_operators`{{/python,ruby}} set allows telling Forest Admin which operators are supported by any given field.

Operators which are not explicitly enabled in that declaration won't be available.

```javascript
class MyCollection extends BaseCollection {
  constructor() {
    // [...]

    this.addField('id', {
      // [...]
      filterOperators: new Set([
        'Equal', // Tell Forest Admin the equal operator is available on the id field
        // ...
      ]),
    });
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self):
      # [...]

      self.add_field('id', {
          # [...]
          "filter_operators": {
              'equal', # Tell Forest Admin the equal operator is available on the id field
              # ...
          },
      })

```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      def initialize
        # [...]

        add_field('id', {
            # [...]
            filter_operators: ['Equal'], # Tell Forest Admin the equal operator is available on the id field
            # ...
        })
      end
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Schema\ColumnSchema;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        // [...]
    }

    $this->addField('id', new ColumnSchema(
        // [...]
        filterOperators: [
            Operators::EQUAL, // Tell Forest Admin the equal operator is available on the id field
            // ...
        ],
    ));
}
```

## Sort

Not all fields need to be sortable. Sortable fields should be flagged in the following way.

```javascript
class MyCollection extends BaseCollection {
  constructor() {
    // [...]

    this.addField('id', {
      // [...]
      isSortable: true,
    });
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self):
      # [...]

      self.add_field('id', {
          # [...]
          "is_sortable": True
      })

```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      def initialize
        # [...]

        add_field('id', {
            # [...]
            is_sortable: true
        })
      end
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Schema\ColumnSchema;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        // [...]
    }

    $this->addField('id', new ColumnSchema(
        // [...]
        isSortable: true,
    ));
}
```
