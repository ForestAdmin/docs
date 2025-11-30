Relationships that point to multiple records are displayed in the frontend in the "Related Data" and "Explorer" Tab.

![Explorer Tab](../../assets/relationships-multiple-explorer.png)

## One-to-Many relations

In a one-to-many relationship, one record from a Collection is attached to multiple records of another Collection.

Think about countries and towns: a country has multiple towns, and each town belongs to a country.

```javascript
// Link 'countries' to 'towns'
agent.customizeCollection('countries', collection => {
  collection.addOneToManyRelation('myTowns', 'towns', {
    originKey: 'country_id',
    originKeyTarget: 'id', // Optional (uses primary key of countries by default)
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;

// Link 'Country' to 'Town'
$forestAgent->customizeCollection(
    'Country',
    function (CollectionCustomizer $builder) {
        $builder->addOneToManyRelation(
            name: 'myTowns',
            foreignCollection: 'Town',
            originKey: 'country_id',
            originKeyTarget: 'id' // Optional (uses `Country` primary key by default)
        );
    }
);
```

```ruby
@create_agent.customize_collection('country') do |collection|
  collection.add_one_to_many_relation(
    'myTown',
    'town',
    {
      origin_key: 'country_id',
      origin_key_target: 'id' # Optional (uses primary key of countries by default)
    }
  )
end
```

```python
# Link 'countries' to 'towns'
agent.customize_collection('countries').add_one_to_many_relation(
    name='myTowns',
    foreign_collections='towns',
    origin_key='country_id',
    origin_key_target='id', # Optional (uses primary key of countries by default)
)
```

## Many-to-Many relations

In a many-to-many relation, 3 Collections are used instead of 2 to build the relation.

This allows multiple records from one Collection to be attached to multiple records from another Collection.

For instance, on a movie recommendation website, each user can rate many movies, and each movie can be rated by many users.
The 3 Collections that are used are `users` (the "origin" Collection), `ratings` (the "through" Collection), and `movies` (the "foreign" Collection).

```javascript
// Create one side of the relation ...
agent.customizeCollection('users', collection => {
  collection.addManyToManyRelation('ratedMovies', 'movies', 'ratings', {
    originKey: 'user_id',
    foreignKey: 'movie_id',
  });
});

// ... and the other one
agent.customizeCollection('movies', collection => {
  collection.addManyToManyRelation('whoRatedThisMovie', 'users', 'ratings', {
    originKeyTarget: 'id', // Optional (uses primary key of movies by default)
    originKey: 'movie_id',
    foreignKey: 'user_id',
    foreignKeyTarget: 'id', // Optional (uses primary key of users by default)
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;

// Create one side of the relation ...
$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->addManyToManyRelation(
            name: 'ratedMovies',
            foreignCollection: 'Movie',
            throughCollection: 'Rating'
            originKey: 'user_id',
            originKeyTarget: 'id', // Optional (uses primary key of User by default)
            foreignKey: 'movie_id',
            foreignKeyTarget: 'id' // Optional (uses primary key of Movie by default)
        );
    }
)
    // ... and the other one.
    ->customizeCollection(
        'Movie',
        function (CollectionCustomizer $builder) {
            // ⚠️ Not 'OneToOne'
            $builder->addManyToOneRelation(
                name: 'whoRatedThisMovie',
                foreignCollection: 'User',
                throughCollection: 'Rating'
                originKey: 'movie_id',
                originKeyTarget: 'id', // Optional (uses primary key of Movie by default)
                foreignKey: 'user_id',
                foreignKeyTarget: 'id' // Optional (uses primary key of User by default)
            );
        }
    );
```

```ruby
@create_agent.customize_collection('user') do |collection|
  collection.add_many_to_many_relation(
    'ratedMovies',
    'movie',
    'rating',
    {
      origin_key: 'user_id',
      foreign_key: 'movie_id',
    }
  )

  # ... and the other one.
  collection.customize_collection('Movie') do |collection|
      # ⚠️ Not 'OneToOne'
    collection.add_many_to_one_relation(
      'whoRatedThisMovie',
      'user',
      'rating',
      {
        origin_key_target: 'id', # Optional (uses primary key of Movie by default)
        origin_key: 'movie_id',
        foreign_key: 'user_id',
        foreign_key_target: 'id', # Optional (uses primary key of User by default)
      }
    )
  end
end
```

```python
# Create one side of the relation ..
agent.customize_collection('users').add_many_to_many_relation(
    name='ratedMovies',
    foreign_collection='Movie',
    through_collection='Rating'
    origin_key='user_id',
    origin_key_target='id', # Optional (uses primary key of User by default)
    foreign_key='movie_id',
    foreign_key_target='id' # Optional (uses primary key of Movie by default)
)
```

## External relations

External relations allow to define Collections which will only be available through the "Related Data" section or a given model.

{% hint style="warning" %}
Note that external relations do not support pagination.
{% endhint %}

```javascript
const states = [
  { code: 'AK', name: 'Alaska', zip: [99501, 99950], closeTo: [] },
  { code: 'AL', name: 'Alabama', zip: [35004, 36925], closeTo: ['TE', 'MI', 'GE'] },
  { code: 'AR', name: 'Arkansas', zip: [71601, 72959], closeTo: ['OK', 'TX', 'LO'] },
  { code: 'AZ', name: 'Arizona', zip: [85001, 86556], closeTo: ['NM', 'CO', 'NE'] },
  { code: 'CA', name: 'California', zip: [90001, 96162], closeTo: ['OR', 'NE'] },
  // ....
];

agent.customizeCollection('address', collection => {
  collection.addExternalRelation('nearStates', {
    // Define schema of the records in the relation.
    schema: { code: 'Number', name: 'String' },

    // Which fields are needed from the parent record to run the handler?
    // Dependencies are optional: by default only the primary key of address would be
    // provided.
    dependencies: ['country', 'zipCode'],

    // Compute list of records from the parent record
    listRecords: async ({ country, zipCode }) => {
      if (country === 'USA') {
        const state = states.find(s => s.zip[0] < zipCode && zipCode < s.zip[1]);
        return states.filter(s => state.closeTo.includes(s.code));
      }

      return [];
    },
  });
});
```

```php
<?php

use ForestAdmin\AgentPHP\Agent\Utils\Env;
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceDoctrine\DoctrineDatasource;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;

return static function (ForestAgent $forestAgent) {
    $forestAgent->agent->addDatasource(
        new DoctrineDatasource($forestAgent->getEntityManager(), [
            'url'      => Env::get('DATABASE_URL'),
        ])
    )->customizeCollection('address', function (CollectionCustomizer $builder) {
            $states = [
                [ 'code' => 'AK', 'name' => 'Alaska', 'zipMin' => 99501, 'zipMax' => 99950, 'closeTo' => [] ],
                [ 'code' => 'AL', 'name' => 'Alabama', 'zipMin' => 35004, 'zipMax' => 36925, 'closeTo' => ['TE', 'MI', 'GE', 'FL'] ],
                [ 'code' => 'AR', 'name' => 'Arkansas', 'zipMin' => 71601, 'zipMax' => 72959, 'closeTo' => ['OK', 'TX', 'LO', 'MI'] ],
                [ 'code' => 'AZ', 'name' => 'Arizona', 'zipMin' => 85001, 'zipMax' => 86556, 'closeTo' => ['NM', 'CO', 'UT', 'NE'] ],
                [ 'code' => 'CA', 'name' => 'California', 'zipMin' => 90001, 'zipMax' => 96162, 'closeTo' => ['OR', 'NE', 'AZ'] ],
                // ....
            ];

            $builder->addExternalRelation(
                'nearStates',
                [
                    // Define schema of the records in the relation.
                    'schema' => ['code' => 'String', 'name' => 'String'],

                    // Which fields are needed from the parent record to run the handler?
                    // Dependencies are optional: by default only the primary key of address would be provided
                    'dependencies' => ['country', 'zipCode'],

                    'listRecords' => function ($record) use ($states) {
                        if ($record['country'] === 'USA') {
                            $state = array_filter($states, fn ($s) => $s['zipMin'] < $record['zipCode'] && $record['zipCode'] < $s['zipMax']);
                            $state = array_shift($state);

                            return array_filter($states, fn ($s) => (in_array($s['code'], $state['closeTo'])));
                        }

                        return [];
                    },
                ]
            );
        }
    );
};
```

```ruby
@create_agent.customize_collection('address') do |collection|
  states = [
    { 'code' => 'AK', 'name' => 'Alaska', 'zip' => [99501, 99950], 'closeTo' => [] },
    { 'code' => 'AL', 'name' => 'Alabama', 'zip' => [35004, 36925], 'closeTo' => ['TE', 'MI', 'GE'] },
    { 'code' => 'AR', 'name' => 'Arkansas', 'zip' => [71601, 72959], 'closeTo' => ['OK', 'TX', 'LO'] },
    { 'code' => 'AZ', 'name' => 'Arizona', 'zip' => [85001, 86556], 'closeTo' => ['NM', 'CO', 'NE'] },
    { 'code' => 'CA', 'name' => 'California', 'zip' => [90001, 96162], 'closeTo' => ['OR', 'NE'] },
  # ....
  ]

  collection.add_external_relation(
    'nearStates',
    {
      # Define schema of the records in the relation.
      schema: { 'code' => 'Number', 'name' => 'String' },

      # Which fields are needed from the parent record to run the handler?
      # Dependencies are optional: by default only the primary key of address would be provided.
      dependencies: %w[country zipCode],

      # Compute list of records from the parent record
      list_records: proc { |record|
        if record['country'] == 'USA'
          state = states.find { |s| s['zip'][0] < record['zipCode'] && record['zipCode'] < s['zip'][1] }
          return states.filter { |s| state['closeTo'].include?(s['code']) }
        end

        return []
      }
    }
  )
end

```

```python
from forestadmin.datasource_toolkit.context.collection_context import (
    CollectionCustomizationContext
)
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

STATES = [
    {"code": "AK", "name": "Alaska", "zip": [99501, 99950], "closeTo": []},
    {
        "code": "AL",
        "name": "Alabama",
        "zip": [35004, 36925],
        "closeTo": ["TE", "MI", "GE"],
    },
    {
        "code": "AR",
        "name": "Arkansas",
        "zip": [71601, 72959],
        "closeTo": ["OK", "TX", "LO"],
    },
    {
        "code": "AZ",
        "name": "Arizona",
        "zip": [85001, 86556],
        "closeTo": ["NM", "CO", "NE"],
    },
    {
        "code": "CA",
        "name": "California",
        "zip": [90001, 96162],
        "closeTo": ["OR", "NE"]
    },
    # ....
]

async def near_states_list_records(
    record: RecordsDataAlias, context: CollectionCustomizationContext
) -> Any:
    ret = []
    if record["country"] == "USA":
        zip_code = record["zipCode"]
        for state in STATES:
            if not (state["zip"][0] < zip_code and zip_code < state["zip"][1]):
                continue
            ret.extend(filter(lambda s: s["code"] in state["closeTo"], STATES))

    return ret

# Create one side of the relation ..
agent.customize_collection('address').add_external_relation(
    "nearStates",
    {
        # Define schema of the records in the relation.
        "schema": { "code": "Sting", "name": "String" },

        # Which fields are needed from the parent record to run the handler?
        # Dependencies are optional: by default only the primary key of address
        # would be provided.
        "dependencies": ['country', 'zipCode'],

        # Compute list of records from the parent record
        "list_records": near_states_list_records
    }
)
```
