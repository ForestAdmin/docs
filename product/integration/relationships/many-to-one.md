Relationships that point to a single record are displayed in the GUI as links.

Once configured, they can be used in
[charts](https://docs.forestadmin.com/user-guide/dashboards/charts),
[filters](https://docs.forestadmin.com/user-guide/getting-started/master-your-ui/the-table-view#add-one-or-several-filters),
[scopes](https://docs.forestadmin.com/user-guide/collections/scopes), and
[segments](https://docs.forestadmin.com/user-guide/collections/segments).

![Many-to-One relation in the Table View](../../assets/relationships-single-link.png)

{% hint style="info" %}
Note that, for performance reasons when sorting a Table View on customizer-defined relations, Forest Admin will always sort on the primary key of the related collection.
{% endhint %}

## Many-to-One relations

Many-to-One relations are by far the most common type of relation: many records from a Collection are attached to another Collection record.

Think about countries and towns: a town belongs to a single country, but each country can have multiple towns.

```javascript
agent.customizeCollection('towns', collection =>
  collection.addManyToOneRelation('country', 'countries', {
    foreignKey: 'country_id',
    foreignKeyTarget: 'id', // Optional (uses `country` primary key by default)
  }),
);
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;

$forestAgent->customizeCollection(
    'Town',
    function (CollectionCustomizer $builder) {
        $builder->addManyToOneRelation(
            name: 'country',
            foreignCollection: 'Country',
            foreignKey: 'country_id',
            foreignKeyTarget: 'id' // Optional (uses `Country` primary key by default)
        );
    }
);
```

```ruby
@create_agent.customize_collection('town') do |collection|
  collection.add_many_to_one_relation(
    'country',
    'Country',
    {
      foreign_key: 'country_id',
      foreign_key_target: 'id' # Optional (uses `Country` primary key by default)
    }
  )
end
```

```python
agent.customize_collection('towns').add_many_to_one_relation(
    name='country',
    foreign_collection='Country',
    foreign_key= 'country_id',
    foreign_key_target= 'id', # Optional (uses `country` primary key by default)
)
```

## One-to-One relations

In a one-to-one relation, there is a one-to-one mapping between records in 2 Collections. The relation can be unset for some records, but no record from the first Collection can be linked to more than one record in the other Collection.

Think about cities and mayors: A city can have at most one mayor, and each mayor belongs to a single city.

{% hint style="warning" %}
Take note that the inverse of a `one-to-one` is a `many-to-one`.

This may seem counter-intuitive: the side of the relation which should be configured as `many-to-one` is the one that carries the foreign key.
{% endhint %}

```javascript
// Configure one side of the relation ...
agent.customizeCollection('mayors', collection => {
  collection.addOneToOneRelation('city', 'cities', {
    originKey: 'mayor_id',
    originKeyTarget: 'id', // Optional (uses `mayors` primary key by default)
  });
});

// ... and the other one.
agent.customizeCollection('cities', collection => {
  // ⚠️ Not 'OneToOne'
  collection.addManyToOneRelation('mayor', 'mayors', {
    foreignKey: 'mayor_id',
    foreignKeyTarget: 'id', // Optional (uses `mayors` primary key by default)
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;

// Configure one side of the relation ...
$forestAgent->customizeCollection(
    'Mayor',
    function (CollectionCustomizer $builder) {
        $builder->addOneToOneRelation(
            name: 'city',
            foreignCollection: 'City',
            originKey: 'mayor_id',
            originKeyTarget: 'id' // Optional (uses `Mayor` primary key by default)
        );
    }
)
    // ... and the other one.
    ->customizeCollection(
        'City',
        function (CollectionCustomizer $builder) {
            // ⚠️ Not 'OneToOne'
            $builder->addManyToOneRelation(
                name: 'mayor',
                foreignCollection: 'Mayor',
                foreignKey: 'mayor_id',
                foreignKeyTarget: 'id' // Optional (uses `Mayor` primary key by default)
            );
        }
    );
```

```ruby
@create_agent.customize_collection('mayor') do |collection|
  collection.add_one_to_one_relation(
    'city',
    'City',
    {
      origin_key: 'mayor_id',
      origin_key_target: 'id' # Optional (uses `Mayor` primary key by default)
    }
  )
  # ... and the other one.
  .customize_collection('City') do |collection|
    # ⚠️ Not 'OneToOne'
    collection.add_many_to_one_relation(
      'mayor',
      'Mayor',
      {
        foreign_key: 'mayor_id',
        foreign_key_target: 'id' # Optional (uses `Mayor` primary key by default)
      }
    )
  end
end
```

```python
# Configure one side of the relation ...
agent.customize_collection('mayors').add_one_to_one_relation(
    name='city',
    foreign_collection='cities',
    origin_key='mayor_id',
    origin_key_target='id', # Optional (uses `mayors` primary key by default)
)

# ... and the other one.
agent.customize_collection('cities')
  # ⚠️ Not 'OneToOne'
  collection.add_many_to_one_relation(
    name='mayor',
    foreign_collection='mayors',
    foreign_key='mayor_id',
    foreign_key_target='id', # Optional (uses `mayors` primary key by default)
)
```
