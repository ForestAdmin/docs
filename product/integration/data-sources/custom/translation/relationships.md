We've seen that when building data sources, collections must fulfill the contract they defined [when declaring their capabilities](./capabilities.md).

The same is also true for [Intra-data source relationships](./structure.md#relationships) which are declared on the structure of a collection: declared relations must be handled when processing `filters`, `projection`, and `aggregations`.

# In practice

`one-to-many` and `many-to-many` relationships do not require additional work: Forest Admin will automatically call the destination collection with a valid filter.

On the other hand, `many-to-one` and `one-to-one` relationships require the implementer to make all fields from the target collection available on the source collection (under a prefix).

## Example

If a structure declaration contains the following statement

{% tabs %}
{% tab title="class MovieCollection extends BaseCollection {" %}
```javascript
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
{% endtab %}

{% tab title="module App" %}
```ruby
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
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="// The following call is using both fields from the "movies" and "people" collection" %}
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
    }.
    sort: [{ field: 'director:birthDate', ascending: true }]
  },
  ['id', 'title', 'director:firstName', 'director:lastName']
);
```
{% endtab %}

{% tab title="datasource.collection('Movie').list(" %}
```ruby
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
{% endtab %}
{% endtabs %}

should return

```json
{
  "id": 34,
  "title": "E.T",
  "director": { "firstName": "Steven", "lastName": "Spielberg" }
}
```
