Forest Admin allows creating new Fields on any Collection, either computationally, by fetching data on an external API or based on other data that is available on the connected data sources.

{{#nodejs,php,ruby}}
By default, the fields that you create will be read-only, but you can make them [filterable](./filter.md), [sortable](./sort.md), and [writable](./write.md) by using the relevant methods.
{{/nodejs,php,ruby}

## How does it work?

When creating a new field you will need to provide:

| Field                                                                                           | Description                                                                                                                                                                                  |
| ----------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| {{#nodejs,php}}columnType{{/nodejs,php}}{{#python,ruby}}column_type{{/python,ruby}}             | Type of the new field which can be [any primitive](../../under-the-hood/data-model/typing.md#primitive-types) or [composite type](../../under-the-hood/data-model/typing.md#composite-types) |
| dependencies                                                                                    | List of fields that you need from the source records and linked records in order to run the handler                                                                                          |
| {{#nodejs,php}}getValues{{/nodejs,php}}{{#python}}get_values{{/python}}{{#ruby}}values{{/ruby}} | Handler which computes the new value **for a batch of records**                                                                                                                              |
| {{#nodejs,php}}enumValues{{/nodejs,php}}{{#python,ruby}}enum_values{{/python,ruby}} (optional)  | When columnType is `Enum`, you must specify the values that the field will support                                                                                                           |

## Examples

### Adding a field by concatenating other fields

This example adds a `user.displayName` field, which is computed by concatenating the first and last names.

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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

// "User" Collection has the following structure: { id, firstName, lastName }
$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'displayName',
            new ComputedDefinition(
                // Type of the new field
                columnType: 'String',

                // Dependencies which are needed to compute the new field (must not be empty)
                dependencies: ['firstName', 'lastName'],

                // Compute function for the new field
                // Note that the function computes the new values in batches: the return value must be
                // an array which contains the new values in the same order than the provided records.
                values: fn ($records) => collect($records)->map(fn ($record) => $record['firstName'] . ' ' . $record['lastName']),
            )
        );
    }
);
```

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

```python
from typing import Any, List

from forestadmin.datasource_toolkit.context.collection_context import (
    CollectionCustomizationContext
)
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

def get_display_name(
    records:List[RecordsDataAlias], context: CollectionCustomizationContext
) -> List[Any]:
    return [f"{record['firstName']} {record['lastName']}" for record in records]


# "User" Collection has the following structure: { id, firstName, lastName }
agent.customize_collection("user").add_field(
    "displayName",
    {
        "column_type": "String",
        "dependencies": ["firstName", "lastName"],
        "get_values": get_display_name,
    },
)

```

### Adding a field that depends on another computed field

This example adds a `user.displayName` field, which is computed by concatenating the first and last names, and then another which capitalize it.

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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

// "User" Collection has the following structure: { id, firstName, lastName }
$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        // Create a first field which is computed by concatenating the first and last names
        $builder->addField(
                'displayName',
                new ComputedDefinition(
                    columnType: 'String',
                    dependencies: ['firstName', 'lastName'],
                    values: fn ($records) => collect($records)->map(fn ($record) => $record['firstName'] . ' ' . $record['lastName']),
                )
            );
        // Create a second field which is computed by uppercasing the first field
        ->addField(
            'displayNameCaps',
            new ComputedDefinition(
                columnType: 'String',
                dependencies: ['displayName'], // It is legal to depend on another computed field
                values: fn ($records) => collect($records)->map(fn ($record) => strtoupper($record['displayName'])),
            )
        )
    }
);
```

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

```python
from typing import Any, List

from forestadmin.datasource_toolkit.context.collection_context import (
    CollectionCustomizationContext
)
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

def get_display_name(
    records:List[RecordsDataAlias], context: CollectionCustomizationContext
) -> List[Any]:
    return [f"{record['firstName']} {record['lastName']}" for record in records]


# "User" Collection has the following structure: { id, firstName, lastName }
agent.customize_collection("user").add_field(
  # Create a first field which is computed by concatenating the first and last names
    "displayName",
    {
        "column_type": "String",
        "dependencies": ["firstName", "lastName"],
        "get_values": get_display_name,
    },
).add_field(
    "displayNameCaps",
    {
        "column_type": "String",
        "dependencies": ["displayName"],
        "get_values": lambda records, context: [
            record["displayName"].upper() for record in records
        ],
    },
)
```

### Adding a field that depends on a many-to-one relationship

We can improve the previous example by adding the city of the user to the display name.

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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

// Structure:
// User    { id, addressId, firstName, lastName }
// Address { id, city }
$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'displayName',
            new ComputedDefinition(
                columnType: 'String',

                // We added 'address:city' in the list of dependencies,
                // which tells forest to fetch the related record
                dependencies: ['firstName', 'lastName', 'address:city'],
                values: fn ($records) => collect($records)
                    ->map(fn ($record) => $record['firstName'] . ' ' . $record['lastName'] . ' (from ' . $record['address']['city'] . ' )'),
            )
        );
    }
);
```

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

```python
# Structure:
# User    { id, addressId, firstName, lastName }
# Address { id, city }
agent.customize_collection("user").add_field(
    "displayName",
    {
        "column_type": "String",
        # We added 'address:city' in the list of dependencies,
        # which tells forest to fetch the related record
        "dependencies":["firstName", "lastName", "address:city"],
        "get_values": lambda records, context: [
            f"{record['firstName']} {record['lastName']} (from {record['customer']['city']})"
            for record in records
      ],
    },
)

```

### Adding a field that depends on a one-to-many relationship

Let's now add a `user.totalSpending` field by summing the amount of all `orders`.

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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\ConditionTreeFactory;

// Structure:
// User    { id }
// Order { id, customer_id, amount }
$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'totalSpending',
            new ComputedDefinition(
                columnType: 'Number',
                dependencies: ['id'],
                values: function ($records, $context) {
                    $recordIds = array_map(fn ($r) => $r['id'], $records);

                    // We're using Forest Admin's query interface
                    $filter = new Filter(
                        conditionTree: ConditionTreeFactory::fromArray(
                            ['field' => 'customer_id', 'operator' => 'In', 'value' => $recordIds]
                        )
                    );
                    $aggregation = new Aggregation(operation: 'Sum', field: 'amount', groups: [[ 'field' =>'customer_id' ]]);
                    $rows = $context->getDatasource()->getCollection('Car')->aggregate($filter, $aggregation);

                    return array_map(
                        function ($record) use ($rows) {
                            foreach ($rows as $row) {
                                if ($row['group']['customer_id'] === $record['id']) {
                                    return $row['value'] ?? 0;
                                }
                            }

                            return 0;
                        },
                        $records
                    );
                }
            )
        );
    }
);
```

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

```python
from typing import Any, List
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias
from forestadmin.datasource_toolkit.interfaces.query.aggregation import Aggregation,
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf

# Structure
# Customer  { id }
# Order { id, customerId, amount }
async def get_user_spending_values(
    records: List[RecordsDataAlias], context: CollectionCustomizationContext
) -> List[Any]:
    record_ids = [record["id"] for record in records]
    condition = {
        "conditionTree": [ConditionTreeLeaf("customer_id", "in", record_ids)]
    }
    aggregation = Aggregation(
        {"operation": "Sum", "field": "amount", "groups": [{"field": "customer_id"}]},
    )

    # We're using Forest Admin's query interface (you can use an ORM or a plain SQL query)
    rows = await context.datasource.get_collection("order").aggregate(
        condition, aggregation
    )
    ret = []
    for record in records:
        filtered = list(
            filter(
                lambda row: row["group"]["customer_id"] == record["id"], rows
            )
        )
        row = filtered[0] if len(filtered) == 1 else {}
        ret.append(row.get("value", 0))
    return ret


agent.customize_collection("Customer").add_field(
    "TotalSpending",
    {
        "column_type": "Number",
        "dependencies": ["id"],
        "get_values": get_user_spending_values
    },
)
```

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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

// Fictional verification API.
use Fake\EmailVerificationClient;
$client = new EmailVerificationClient();
$client->setApiKey('MY_FAKE_API_KEY');

// "User" Collection has the following structure: { id, email }
$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'emailDeliverable',
            new ComputedDefinition(
                columnType: 'Boolean',
                dependencies: ['email'],
                values: function ($records) {
                    $response = $client->verifyEmails(collect($records)->map(fn ($record) => $record['email'])->toArray());

                    return collect($records)
                        ->map(function ($record) use ($response) {
                            $check = $response[$record['email']];

                            return $check['domainValid'] && (! $check['usernameChecked'] || $check['usernameValid'])
                        });
                    }
                }
            )
        );
    }
);
```

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

```python
from typing import Any, List

from forestadmin.datasource_toolkit.context.collection_context import (
    CollectionCustomizationContext
)
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias
from fake import EmailVerificationClient

client = EmailVerificationClient()
client.set_api_key("MY_FAKE_API_KEY")

# "User" Collection has the following structure: { id, email }
async def get_user_spending_values(
    records: List[RecordsDataAlias], context: CollectionCustomizationContext
) -> List[Any]:
    emails = [record["email"] for record in records]
    response = client.verifyEmails(emails)
    # Always return values in the same order than the source records
    ret = []
    for record in records:
        check = response[record["email"]]
        ret.append(
            check["domainValid"]
            and (not check["usernameChecked"] or check["usernameValid"])
        )
    return ret


agent.customize_collection("User").add_field(
    "TotalSpending",
    {
        "column_type":"Boolean",
        "dependencies":["email"],
        "get_values":get_user_spending_values
    },
)
```

## Performance

When adding many fields, keep in mind that:

- You should refrain from making queries to external services
  - Use relationships in the `dependencies` array when that is possible
  - Use batch APIs calls instead of performing requests one by one inside of the `records.map` handler.
- Only add fields you need in the `dependencies` list
  - This will reduce the pressure on your data sources (fewer columns to fetch)
  - And increase the probability of reducing the number of records that will be passed to your handler (records are deduplicated).
- Do not duplicate code between handlers of different fields: fields can depend on each other (no cycles allowed).
