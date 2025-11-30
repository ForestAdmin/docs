Relationships that point to multiple records are displayed in the frontend in the "Related Data" and "Explorer" Tab.

![Explorer Tab](../../assets/relationships-multiple-explorer.png)

## One-to-Many relations

In a one-to-many relationship, one record from a Collection is attached to multiple records of another Collection.

Think about countries and towns: a country has multiple towns, and each town belongs to a country.

<details>
<summary><strong>// Link 'countries' to 'towns'</strong></summary>

```javascript
agent.customizeCollection('countries', collection => {
  collection.addOneToManyRelation('myTowns', 'towns', {
    originKey: 'country_id',
    originKeyTarget: 'id', // Optional (uses primary key of countries by default)
  });
});
```

</details>

<details>
<summary><strong>@create_agent.customize_collection('country') do |collection|</strong></summary>

```ruby
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

</details>


## Many-to-Many relations

In a many-to-many relation, 3 Collections are used instead of 2 to build the relation.

This allows multiple records from one Collection to be attached to multiple records from another Collection.

For instance, on a movie recommendation website, each user can rate many movies, and each movie can be rated by many users.
The 3 Collections that are used are `users` (the "origin" Collection), `ratings` (the "through" Collection), and `movies` (the "foreign" Collection).

<details>
<summary><strong>// Create one side of the relation ...</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>@create_agent.customize_collection('user') do |collection|</strong></summary>

```ruby
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

</details>


## External relations

External relations allow to define Collections which will only be available through the "Related Data" section or a given model.

{% hint style="warning" %}
Note that external relations do not support pagination.
{% endhint %}

<details>
<summary><strong>const states = [</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>@create_agent.customize_collection('address') do |collection|</strong></summary>

```ruby
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

</details>


