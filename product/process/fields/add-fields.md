---
title: Add fields
---

Forest Admin allows creating new Fields on any Collection, either computationally, by fetching data on an external API or based on other data that is available on the connected data sources.

By default, the fields that you create will be read-only, but you can make them filterable, sortable, and writable by using the relevant methods.

## How does it work?

When creating a new field you will need to provide:

| Field                                                                                           | Description                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `columnType` (`column_type` in Ruby)             | Type of the new field (String, Number, Boolean, Date, Enum, etc.) |
| `dependencies`                                                                                    | List of fields that you need from the source records and linked records in order to run the handler                                                                                          |
| `getValues` (`values` in Ruby) | Handler which computes the new value **for a batch of records**                                                                                                                              |
| `enumValues` (`enum_values` in Ruby) (optional)  | When columnType is `Enum`, you must specify the values that the field will support                                                                                                           |

## Examples

### Adding a field by concatenating other fields

This example adds a `user.displayName` field, which is computed by concatenating the first and last names.

{% tabs %}
{% tab title="Node.js" %}
```javascript
// "user" Collection has the following structure: { id, firstName, lastName }
agent.customizeCollection('user', collection => {
  collection.addField('displayName', {
    // Type of the new field
    columnType: 'String',

    // Dependencies which are needed to compute the new field (must not be empty)
    dependencies: ['firstName', 'lastName'],

    // Compute function for the new field
    // Note that the function computes the new values in batches: the return value
    // must be an array which contains the new values in the same order than the
    // provided records.
    getValues: (records, context) =>
      records.map(r => `${r.firstName} ${r.lastName}`),
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed

# User Collection has the following structure: { id, firstName, lastName }
@create_agent.customize_collection('user') do |collection|
  collection.add_field(
    'displayName',
    ComputedDefinition.new(
      # Type of the new field
      column_type: 'String',
      # Dependencies which are needed to compute the new field (must not be empty)
      dependencies: %w[firstName lastName],
      # Compute function for the new field
      # Note that the function computes the new values in batches: the return value must be
      # an array which contains the new values in the same order than the provided records.
      values: proc { |records| records.map { |record| "#{record["firstName"]} #{record["lastName"]}" } }
    )
  )
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
// "user" Collection has the following structure: { id, firstName, lastName }
agent.customizeCollection('user', collection => {
  collection
    // Create a field which is computed by concatenating the first and last names
    .addField('displayName', {
      columnType: 'String',
      dependencies: ['firstName', 'lastName'],
      getValues: (records, context) =>
        records.map(r => `${r.firstName} ${r.lastName}`),
    })

    // Create another field which is computed by uppercasing the first field
    .addField('displayNameCaps', {
      columnType: 'String',
      dependencies: ['displayName'], // You can depend on other computed fields
      getValues: (records, context) => records.map(r => r.displayName.toUpperCase()),
    });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed

# User Collection has the following structure: { id, firstName, lastName }
@create_agent.customize_collection('user') do |collection|
  collection
    # Create a first field which is computed by concatenating the first and last names
    .add_field(
      'displayName',
      ComputedDefinition.new(
        column_type: 'String',
        dependencies: %w[firstName lastName],
        values: proc { |records| records.map { |record| "#{record['firstName']} #{record['lastName']}" } }
      )
    )
    # Create a second field which is computed by uppercasing the first field
    .add_field(
      'displayNameCaps',
      ComputedDefinition.new(
        column_type: 'String',
        dependencies: ['displayName'], # It is legal to depend on another computed field
        values: proc { |records| records.map { |record| record['displayName'].upcase } }
      )
    )
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
// Structure:
// User    { id, addressId, firstName, lastName }
// Address { id, city }

agent.customizeCollection('user', collection => {
  collection.addField('displayName', {
    columnType: 'String',

    // We added 'address:city' in the list of dependencies,
    // which tells forest to fetch the related record
    dependencies: ['firstName', 'lastName', 'address:city'],

    // The address is now available in the parameters
    getValues: (records, context) =>
      records.map(r => `${r.firstName} ${r.lastName} (from ${r.address.city})`),
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed

# Structure:
# User    { id, addressId, firstName, lastName }
# Address { id, city }
@create_agent.customize_collection('user') do |collection|
  collection.add_field(
    'displayName',
    ComputedDefinition.new(
      column_type: 'String',

      # We added 'address:city' in the list of dependencies,
      # which tells forest to fetch the related record
      dependencies: %w[firstName lastName address:city],
      values: proc { |records| records.map { |record| "#{record['firstName']} #{record['lastName']} (from #{record['address']['city']})" } }
    )
  )
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
// Structure
// User  { id }
// Order { id, customer_id, amount }

agent.customizeCollection('user', collection => {
  collection.addField('totalSpending', {
    columnType: 'Number',
    dependencies: ['id'],
    getValues: async (records, context) => {
      const recordIds = records.map(r => r.id);

      // We're using Forest Admin's query interface
      // (you can use an ORM or a plain SQL query)
      const filter = {
        conditionTree: { field: 'customer_id', operator: 'In', value: recordIds },
      };
      const aggregation = {
        operation: 'Sum',
        field: 'amount',
        groups: [{ field: 'customer_id' }],
      };
      const rows = await context.dataSource
        .getCollection('order')
        .aggregate(filter, aggregation);

      return records.map(record => {
        const row = rows.find(r => r.group.customer_id === record.id);
        return row?.value ?? 0;
      });
    },
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed
include ForestAdminDatasourceToolkit::Components::Query
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

# Structure:
# User  { id }
# Order { id, customer_id, amount }
@create_agent.customize_collection('user') do |collection|
  collection.add_field(
    'totalSpending',
    ComputedDefinition.new(
      column_type: 'Number',
      dependencies: ['id'],
      values: proc do |records, context|
        record_ids = records.map { |record| record['id'] }

        # We're using Forest Admin's query interface
        filter = Filter.new(condition_tree: Nodes::ConditionTreeLeaf.new('customer_id', Operators::IN, record_ids))
        aggregation = Aggregation.new(operation: 'Sum', field: 'amount', groups: [ { field: 'customer_id' } ])
        rows = context.datasource.get_collection('order').aggregate(filter, aggregation)

        records.map do |record|
          filtered = rows.select { |row| row['group']['customer_id'] == record['id'] }
          filtered.empty? ? 0 : filtered[0]['value']
        end
      end
    )
  )
end
```
{% endtab %}
{% endtabs %}

### Adding a field fetching data from an API

Let's imagine that we want to check if the email address of our users is deliverable.
We can use a verification API to perform that work.

The API we're using is fictional, and the structure of the response is:

```json
{
  "username1@domain.com": {
    "usernameChecked": false,
    "usernameValid": null,
    "domainValid": true
  },
  "username2@domain.com": {
    "usernameChecked": false,
    "usernameValid": null,
    "domainValid": true
  }
}
```

{% tabs %}
{% tab title="Node.js" %}
```javascript
const emailVerificationClient = require('@sendchimplio/client');
emailVerificationClient.setApiKey(process.env.SENDCHIMPLIO_API_KEY);

// "User" Collection has the following structure: { id, email }
agent.customizeCollection('user', collection => {
  collection.addField('emailDeliverable', {
    columnType: 'Boolean',
    dependencies: ['email'],
    getValues: async (records, context) => {
      // Call the API to verify emails
      const response = await emailVerificationClient.verifyEmails(
        records.map(r => r.email),
      );

      // Return values in the same order than the source records
      return records.map(r => {
        const check = response[r.email];
        return check.domainValid && (!usernameChecked || usernameValid);
      });
    },
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed
include ForestAdminDatasourceToolkit::Components::Query
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

# Fictional verification API.
include Fake::EmailVerificationClient

client = EmailVerificationClient.new
client.api_key = 'MY_FAKE_API_KEY'

# "User" Collection has the following structure: { id, email }
@create_agent.customize_collection('user') do |collection|
  collection.add_field(
    'emailDeliverable',
    ComputedDefinition.new(
      column_type: 'Boolean',
      dependencies: ['email'],
      values: proc do |records|
        response = client.verify_emails(records.map { |record| record['email'] })

        records.map do |record|
          check = response[record['email']]
          check['domainValid'] && (!check['usernameChecked'] || check['usernameValid'])
        end
      end
    )
  )
end
```
{% endtab %}
{% endtabs %}

## Performance

When adding many fields, keep in mind that:

- You should refrain from making queries to external services
  - Use relationships in the `dependencies` array when that is possible
  - Use batch APIs calls instead of performing requests one by one inside of the `records.map` handler.
- Only add fields you need in the `dependencies` list
  - This will reduce the pressure on your data sources (fewer columns to fetch)
  - And increase the probability of reducing the number of records that will be passed to your handler (records are deduplicated).
- Do not duplicate code between handlers of different fields: fields can depend on each other (no cycles allowed).
