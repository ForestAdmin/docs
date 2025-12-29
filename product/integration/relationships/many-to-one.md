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

## Many-to-one relations

Many-to-One relations are by far the most common type of relation: many records from a Collection are attached to another Collection record.

Think about countries and towns: a town belongs to a single country, but each country can have multiple towns.

{% tabs %}
{% tab title="agent.customizeCollection('towns', collection =>" %}
```javascript
collection.addManyToOneRelation('country', 'countries', {
    foreignKey: 'country_id',
    foreignKeyTarget: 'id', // Optional (uses `country` primary key by default)
  }),
);
```
{% endtab %}

{% tab title="@create_agent.customize_collection('town') do |collection|" %}
```ruby
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
{% endtab %}
{% endtabs %}


## One-to-one relations

In a one-to-one relation, there is a one-to-one mapping between records in 2 Collections. The relation can be unset for some records, but no record from the first Collection can be linked to more than one record in the other Collection.

Think about cities and mayors: A city can have at most one mayor, and each mayor belongs to a single city.

{% hint style="warning" %}
Take note that the inverse of a `one-to-one` is a `many-to-one`.

This may seem counter-intuitive: the side of the relation which should be configured as `many-to-one` is the one that carries the foreign key.
{% endhint %}

{% tabs %}
{% tab title="// Configure one side of the relation ..." %}
```javascript
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
{% endtab %}

{% tab title="@create_agent.customize_collection('mayor') do |collection|" %}
```ruby
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
{% endtab %}
{% endtabs %}


