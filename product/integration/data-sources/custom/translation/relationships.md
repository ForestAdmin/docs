We've seen that when building data sources, collections must fulfill the contract they defined [when declaring their capabilities](./capabilities.md).

The same is also true for [Intra-data source relationships](./structure.md#relationships) which are declared on the structure of a collection: declared relations must be handled when processing `filters`, `projection`, and `aggregations`.

# In practice

`one-to-many` and `many-to-many` relationships do not require additional work: Forest Admin will automatically call the destination collection with a valid filter.

On the other hand, `many-to-one` and `one-to-one` relationships require the implementer to make all fields from the target collection available on the source collection (under a prefix).

## Example

If a structure declaration contains the following statement

```javascript
class MovieCollection extends BaseCollection {
  constructor() {
    super('movies', null);

    // [...]

    this.addField('director', {
      type: 'ManyToOne',
      foreignCollection: 'people',
      foreignKey: 'directorId',
      foreignKeyTarget: 'id',
    });
  }
}
```

```python
from forestadmin.datasource_toolkit.collections import Collection

class MyCollection(Collection):
    def __init__(self, datasource):
        super().__init__("movies", datasource)

        #  [...]

        self.add_field("director", {
            "type": "ManyToOne",
            "foreign_collection": "people",
            "foreign_key": "directorId",
            "foreign_key_target": "id",
        })
```

```php
class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        parent::__construct(
            $datasource,
            'Movie'
        );

        // [...]

        $this->addField('director', new ManyToOneSchema(
            foreignKey: 'directorId',
            foreignKeyTarget: 'id',
            foreignCollection: 'People'
        ));
    }
}
```

```ruby
module App
  module Collections
    class Movie < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      include ForestAdminDatasourceToolkit::Components::Query:: ConditionTree

      def initialize(datasource)
        super(datasource, 'Movie')

        # [...]

        add_field('director', ManyToOneSchema.new(
          foreign_key: 'director_id',
          foreign_key_target: 'id',
          foreign_collection: 'People'
        ))
      end
    end
  end
end

```

Then the collection must accept references to fields from the `people` collection under the `director` prefix in all method parameters.

```javascript
// The following call is using both fields from the "movies" and "people" collection
await dataSource.getCollection('movies').list(
  caller,
  {
    conditionTree: {
      aggregator: 'And',
      conditions: [
        { field: 'title', operator: 'Equal', value: 'E.T.' },
        { field: 'director:firstName', operator: 'Equal', value: 'Steven' },
      ]
    }.
    sort: [{ field: 'director:birthDate', ascending: true }]
  },
  ['id', 'title', 'director:firstName', 'director:lastName']
);
```

```python
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.branch import ConditionTreeBranch
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf
from forestadmin.datasource_toolkit.interfaces.query.filter.paginated import PaginatedFilter
from forestadmin.datasource_toolkit.interfaces.query.projections import Projection

# The following call is using both fields from the "movies" and "people" collection
await datasource.get_collection("movies").list(
    caller,
    PaginatedFilter(
        {
            "condition_tree": ConditionTreeBranch(
                aggregator="and",
                conditions=[
                    ConditionTreeLeaf(field="title", operator="Equal", value="E.T."),
                    ConditionTreeLeaf("director:firstName", "Equal", "Steven"),
                ],
            ),
            "sort": [{"field": "director:birthDate", "ascending": True}],
        }
    ),
    Projection("id", "title", "director:firstName", "director:lastName"),
)
```

```php
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Caller;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Nodes\ConditionTreeBranch;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Nodes\ConditionTreeLeaf;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\PaginatedFilter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Sort;

$dataSource->getCollection('Movie')->list(
    $caller,
    new PaginatedFilter(
        conditionTree: new ConditionTreeBranch(
            aggregator: 'and',
            conditions: [
                new ConditionTreeLeaf(
                    field: 'title',
                    operator: 'Equal',
                    value: 'E.T.'
                ),
                new ConditionTreeLeaf(
                    field: 'director:firstName',
                    operator: 'Equal',
                    value: 'Steven'
                ),
            ],
        ),
        sort: new Sort(
            [
               [
                  'field'     => 'director:birthDate',
                  'ascending' => true,
               ],
            ]
        )
    )
);
```

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
        ),
      ],
    ),
    sort: ForestAdminDatasourceToolkit::Components::Query::Sort.new(
      [
        {
            field: 'director:birthDate',
            ascending: true
        }
      ]
    )
  )
)
```

should return

```json
{
  "id": 34,
  "title": "E.T",
  "director": { "firstName": "Steven", "lastName": "Spielberg" }
}
```
