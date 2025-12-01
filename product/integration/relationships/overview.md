When relationships are defined during the customization step, Forest Admin Collections act as if the 2 Collections were natively linked at the data source level.

You may have noticed that relationships within a data source are configured out of the box, so you won't need to define those.

However, you may want to create additional intra and cross data source relationships to:

- help users navigate within your admin panel,
- create charts that use data from multiple data sources,
- let users filter, use scopes, or segment with conditions that cross data source boundaries.

## Minimal example

{% tabs %}
{% tab title="agent.customizeCollection('towns', collection =>" %}
```javascript
collection
    // Towns belong to 1 country
    .addManyToOneRelation('country', 'countries', { foreignKey: 'country_id' })

    // Towns have 1 mayor
    .addOneToOneRelation('mayor', 'mayors', { originKey: 'town_id' })

    // Towns have multiple inhabitants
    .addOneToManyRelation('inhabitants', 'people', { originKey: 'town_id' })

    // Towns electricity is supplied by power plants that are shared with other towns
    .addManyToManyRelation('energyProviders', 'powerPlants', 'utilityContracts', {
      originKey: 'town_id',
      foreignKey: 'power_plant_id',
    })

    // Towns have a list of honorary citizen that is retrievable through a public API
    .addExternalRelation('honoraryCitizen', {
      schema: { firstName: 'String', lastName: 'String' },
      listRecords: async ({ id }) => {
        const response = await axios.get(
          `https://api.mytown.com/cities/${id}/honorary-citizen`,
        );
        return response.body;
      },
    }),
);
```
{% endtab %}

{% tab title="@create_agent.customize_collection('town') do |collection|" %}
```ruby
collection
    # Towns belong to 1 country
    .add_many_to_one_relation('country', 'Country', { foreign_key: 'country_id' })

   # Towns have 1 mayor
    .add_one_to_one_relation('myMayor', 'Mayor', { origin_key: 'town_id' })

    # Towns have multiple inhabitants
    .add_one_to_many_relation('inhabitants', 'Person', { origin_key: 'town_id' })

    # Towns electricity is supplied by power-plants which are shared with other towns
    .add_many_to_many_relation('energyProviders', 'PowerPlant', 'UtilityContract', { origin_key: 'town_id', foreign_key: 'power_plant_id' })

    # Towns have a list of honorary citizen that is retrievable through a public API
    .add_external_relation('honoraryCitizen', {
      # customizer.add_external_relation(
      schema: { 'firstName' => 'String', 'lastName' => 'String' },
      list_records: proc { |record|
        response = HTTParty.get("https://api.mytown.com/cities/#{record['id']}/honorary-citizen")
        response.body
      }
    })
end
```
{% endtab %}
{% endtabs %}


