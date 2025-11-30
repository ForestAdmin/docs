Creating a custom data source always starts with declaring the structure of the data

- Which collections are present?
- What fields do they contain?
- What are their types?

# Columns

## Examples

<details>
<summary><strong>const { BaseCollection } = require('@forestadmin/datasource-toolkit');</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>module App</strong></summary>

```ruby
  module Collections
    class Movie < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      include ForestAdminDatasourceToolkit::Components::Query:: ConditionTree

      def initialize(datasource)
        super(datasource, 'Movie')

        add_field('id', ColumnSchema.new(
          column_type: 'Number',
          is_primary_key: true,
        ))

        add_field('title', ColumnSchema.new(
          column_type: 'String',
          filter_operators: [Operators::PRESENT],
        ))

        add_field('mpa_rating', ColumnSchema.new(
          column_type: 'Enum',
          default_value: 'G',
          enum_values: %w[G PG PG-13 R NC-17],
        ))

        add_field('stars', ColumnSchema.new(
          column_type: [
            {
              'firstName' => 'String',
              'lastName'  => 'String',
            },
          ]
        ))
    end
  end
end
```

</details>

## Typing

The typing system for columns is the same as the one used when declaring fields in the agent customization step.

You can read all about it in ["Under the hood > Data Model > Typing"](../../../under-the-hood/data-model/typing.md).

## Validation

When using primitive type fields, Forest Admin supports declaring a validation clause, which will be imported into the UI of the admin panel to validate records before creating/updating them.

The API for validation is the same as with [condition trees](../../getting-started/queries/filters.md#condition-trees), besides the fact that there is no `"field"` entry.

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

# Relationships

{% hint style='warning' %}
Only **intra**-data source relationships should be declared at the collection level.

For **inter**-data source relationships, you should use [jointures at the customization step](../../../agent-customization/relationships/README.md).
{% endhint %}

You can declare relationships at the collection level, but that means that the data source you are making is responsible for handling them.

This will work out of the box for data sources using the "local-cache" strategy, however, please read ["Intra-data source Relationships"](./relationships.md), before starting with the "query translation" strategy.

## Examples

<details>
<summary><strong>const { BaseCollection } = require('@forestadmin/datasource-toolkit');</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>module App</strong></summary>

```ruby
  module Collections
    class Movie < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      # [...]

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
end

```

</details>

## Typing

The typing system for relationships is the same as the one used when declaring jointures in the agent customization step.

You can read all about it in ["Under the hood > Data Model > Relationships"](../../../under-the-hood/data-model/relationships.md).
