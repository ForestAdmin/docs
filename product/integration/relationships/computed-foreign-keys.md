You may want to create a relationship between 2 Collections, but you don't have a foreign key that is ready to use to connect them.

{{#nodejs,php,python}}
To solve that use case, you should use both [computed fields](../fields/computed.md) and relationships.
{{/nodejs,php,python}}

This is done with the following steps:

- create a new Field containing a foreign key,
- make the Field filterable for the `In` operator (see [Under the hood](./under-the-hood.md) as to why this is required),
- create a relation using it.

## Displaying a link to the last message sent by a customer

We have 2 Collections: `Customers` and `Messages`, linked together by a `one-to-many` relationship.

We want to create a `ManyToOne` relationship with the last message sent by a given customer.

```javascript
agent.customizeCollection('customers', collection => {
  // Create foreign key
  collection.addField('lastMessageId', {
    columnType: 'Number',
    dependencies: ['id'],
    getValues: async (customers, context) => {
      // We're using Forest Admin's Query Interface (you can use an ORM or plain SQL)
      const messages = context.dataSource.getCollection('messages');
      const conditionTree = {
        field: 'customer_id',
        operator: 'In',
        value: customers.map(c => c.id),
      };

      const rows = await messages.aggregate(
        { conditionTree },
        { operation: 'Max', field: 'id', groups: [{ field: 'customer_id' }] },
      );

      return customers.map(record => {
        return rows.find(row => row.group.customer_id === record.id)?.value ?? null;
      });
    },
  });

  // Implement the 'In' operator.
  collection.replaceFieldOperator(
    'lastMessageId',
    'In',
    async (lastMessageIds, context) => {
      const records = await context.dataSource
        .getCollection('messages')
        .list(
          { conditionTree: { field: 'id', operator: 'In', value: lastMessageIds } },
          ['customer_id'],
        );

      return { field: 'id', operator: 'In', value: records.map(r => r.customer_id) };
    },
  );

  // Create relationships using the foreign key we just added.
  collection.addManyToOneRelation('lastMessage', 'messages', {
    foreignKey: 'lastMessageId',
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\PaginatedFilter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\ConditionTreeFactory;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Projection\Projection;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        // Create foreign key
        $builder->addField(
            'lastMessageId',
            new ComputedDefinition(
                columnType: 'Number',
                dependencies: ['id'],
                values: function ($customers, $context) {
                	$customerIds = array_map(fn ($r) => $r['id'], $customers);

                    // We're using Forest Admin's query interface
                    $filter = new Filter(
                        conditionTree: ConditionTreeFactory::fromArray(
                            ['field' => 'customer_id', 'operator' => 'In', 'value' => $customerIds]
                        )
                    );
                    $aggregation = new Aggregation(operation: 'Max', field: 'id', groups: [[ 'field' =>'customer_id' ]]);
                    $rows = $context->getDatasource()->getCollection('Message')->aggregate($filter, $aggregation);

                    return array_map(
                        function ($customer) use ($rows) {
                            foreach ($rows as $row) {
                                if ($row['group']['customer_id'] === $customer['id']) {
                                    return $row['value'] ?? null;
                                }
                            }

                            return 0;
                        },
                        $customers
                    );
                }
            )
        )->replaceFieldOperator(
            'lastMessageId',
            'In',
            function ($lastMessageIds, $context) {
                $filter = new PaginatedFilter(
                        conditionTree: ConditionTreeFactory::fromArray(
                            ['field' => 'id', 'operator' => 'In', 'value' => $lastMessageIds]
                        )
                    );
                $records = $context->getDatasource()
                    ->getCollection('Message')
                    ->list($filter, new Projection(['customer_id']));

                return ['field' => 'id', 'operator' => 'In', 'value' => array_map(fn ($r) => $r['customer_id'], $records)];
            }
        )->addManyToOneRelation('lastMessage', 'Message', 'lastMessageId');
    }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed
include ForestAdminDatasourceToolkit::Components::Query
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('customer') do |collection|
  # Create foreign key
  collection.add_field(
    'lastMessageId',
    ComputedDefinition.new(
      column_type: 'Number',
      dependencies: ['id'],
      values: proc { |customers, context|
        customer_ids = customers.map { |r| r['id'] }

        # We're using Forest Admin's Query Interface
        filter = Filter.new(
          condition_tree: Nodes::ConditionTreeLeaf.new('customer_id', Operators::IN, customer_ids)
        )
        aggregation = Aggregation.new(operation: 'Max', field: 'id', groups: [{ field: 'customer_id' }])
        rows = context.datasource.get_collection('message').aggregate(filter, aggregation)

        customers.map do |customer|
          row = rows.find { |r| r['group']['customer_id'] == customer['id'] }
          row ? row['value'] : nil
        end
      }
    )
  )
  .replace_field_operator('lastMessageId', Operators::IN) do |last_message_ids, context|
    filter = Filter.new(
      condition_tree: Nodes::ConditionTreeLeaf.new('id', Operators::IN, last_message_ids)
    )
    records = context.datasource.get_collection('message').list(filter, Projection.new(['customer_id']))

    { field: 'id', operator: 'In', value: records.map { |r| r['customer_id'] } }
  end
  .add_many_to_one_relation('lastMessage', 'message', { foreign_key: 'lastMessageId' })
end
```

```python
from forestadmin.datasource_toolkit.context.collection_context import (
    CollectionCustomizationContext,
)
from forestadmin.datasource_toolkit.interfaces.fields import Operator
from forestadmin.datasource_toolkit.interfaces.query.aggregation import (
    Aggregation,
    Aggregator,
)
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.base import (
    ConditionTree,
)
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import (
    ConditionTreeLeaf,
)
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias


async def get_last_message_id(
    customers: RecordsDataAlias, context: CollectionCustomizationContext
):
    # We're using Forest Admin's Query Interface (you can use an ORM or plain SQL)
    messages = context.datasource.get_collection("messages")
    condition_tree = ConditionTreeLeaf(
        "customer_id", Operator.IN, [c["id"] for c in customers]
    )

    rows = await messages.aggregate(
        context.caller,
        Filter({"condition_tree": condition_tree}),
        Aggregation(
            {
                "field": "id",
                "operation": "Max",
                "groups": [{"field": "customer_id"}],
            }
        ),
    )
    ret = []
    for customer in customers:
        row = filter(lambda r: r["group"]["customer_id"] == customer["id"], rows)
        row = row[0] if len(row) == 1 else {}
        ret.append(row.get("value"))
    return ret


async def last_message_in_operator(
    last_message_ids: List[Any], context: CollectionCustomizationContext
) -> ConditionTree:
    records = await context.datasource.get_collection("messages").list(
        context.caller,
        Filter(
            {
                "condition_tree": ConditionTreeLeaf(
                    "id", Operator.IN, last_message_ids
                )
            }
        ),
        ["customer_id"],
    )
    return ConditionTreeLeaf("id", Operator.IN, [r["customer_id"] for r in records])


agent.customize_collection("Customers").add_field(
    "lastMessageId",
    {
        # Create foreign key
        "column_type": "Number",
        "dependencies": ["id"],
        "get_values": get_last_message_id,
    },
).replace_field_operator(
    # Implement the 'In' operator.
    "lastMessageId",
    Operator.IN,
    last_message_in_operator,
).add_many_to_one_relation(
    # Create relationships using the foreign key we just added.
    name="lastMessage",
    foreign_collection="messages",
    foreign_key="lastMessageId",
)
```

## Connecting collections without having a shared identifier

You have 2 Collections and both contain users: one comes from your database, and the other one is connected to the CRM that your company uses.

There is no common id between them that can be used to tell Forest Admin how to link them together, however, both Collections have `firstName`, `lastName`, and `birthDate` fields, which taken together, are unique enough.

```javascript
agent
  .customizeCollection('databaseUsers', createFilterableIdentityField)
  .customizeCollection('crmUsers', createFilterableIdentityField)
  .customizeCollection('databaseUsers', createRelationship)
  .customizeCollection('crmUsers', createInverseRelationship);

/**
 * Concatenate firstname, lastname and birthData to make a unique identifier
 * and ensure that the new field is filterable
 */
function createFilterableIdentityField(collection) {
  // Create foreign key on the collection from the database
  collection.addField('userIdentifier', {
    columnType: 'String',
    dependencies: ['firstName', 'lastName', 'birthDate'],
    getValues: user => user.map(u => `${u.firstName}/${u.lastName}/${u.birthDate}`),
  });

  // Implement 'In' filtering operator (required)
  collection.replaceFieldOperator('userIdentifier', 'In', values => ({
    aggregator: 'Or',
    conditions: values.map(value => ({
      aggregator: 'And',
      conditions: [
        { field: 'firstName', operator: 'Equal', value: value.split('/')[0] },
        { field: 'lastName', operator: 'Equal', value: value.split('/')[1] },
        { field: 'birthDate', operator: 'Equal', value: value.split('/')[2] },
      ],
    })),
  }));
}

/** Create relationship between databaseUsers and crmUsers */
function createRelationship(databaseUsers) {
  databaseUsers.addOneToOneRelation('userFromCrm', 'crmUsers', {
    originKey: 'userIdentifier',
    originKeyTarget: 'userIdentifier',
  });
}

/** Create relationship between crmUsers and databaseUsers */
function createInverseRelationship(crmUsers) {
  crmUsers.addManyToOneRelation('userFromDatabase', 'databaseUsers', {
    foreignKey: 'userIdentifier',
    foreignKeyTarget: 'userIdentifier',
  });
}
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\PaginatedFilter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\ConditionTreeFactory;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Projection\Projection;

/**
 * Concatenate firstname, lastname and birthData to make a unique identifier
 * and ensure that the new field is filterable
 */
$createFilterableIdentityField = function (CollectionCustomizer $collection) {
    // Create foreign key on the collection from the database
    $collection->addField(
        'userIdentifier',
        new ComputedDefinition(
            columnType: 'String',
            dependencies: ['firstName', 'lastName', 'birthDate'],
            values: fn($users) => array_map(fn($u) => $u['firstName'] .'/'. $u['lastName'] .'/'. $u['birthDate'] , $users),
        )
    );

  // Implement 'In' filtering operator (required)
  $collection->replaceFieldOperator(
      'userIdentifier',
      Operators::IN,
      fn ($values) => [
          'aggregator' => 'Or',
          'conditions' => array_map(
              fn ($value) => [
                  'aggregator' => 'And',
                  'conditions' => [
                      ['field' => 'firstName', 'operator' => Operators::EQUAL, explode('/', $value)[0]],
                      ['field' => 'lastName', 'operator' => Operators::EQUAL, explode('/', $value)[1]],
                      ['field' => 'birthDate', 'operator' => Operators::EQUAL, explode('/', $value)[2]],
                  ]
              ]
          ),
      ]
  );
};

/** Create relationship between databaseUsers and crmUsers */
$createRelationship = function (CollectionCustomizer $collection) {
    $collection->addOneToOneRelation(
        name: 'userFromCrm',
        foreignCollection: 'crmUsers',
        originKey: 'userIdentifier',
        originKeyTarget: 'userIdentifier',
    );
};

/** Create relationship between crmUsers and databaseUsers */
$createInverseRelationship = function (CollectionCustomizer $collection) {
    $collection->addManyToOneRelation(
        name: 'userFromDatabase',
        foreignCollection: 'databaseUsers',
        foreignKey: 'userIdentifier',
        foreignKeyTarget: 'userIdentifier',
    );
};

$forestAgent->customizeCollection('databaseUsers', $createFilterableIdentityField)
    ->customizeCollection('crmUsers', $createFilterableIdentityField)
    ->customizeCollection('databaseUsers', $createRelationship)
    ->customizeCollection('crmUsers', $createInverseRelationship);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

# Concatenate firstname, lastname and birthData to make a unique identifier
# and ensure that the new field is filterable
def create_filterable_identity_field(collection)
  # Create foreign key on the collection from the database
  collection.add_field(
    'userIdentifier',
    ComputedDefinition.new(
      column_type: 'String',
      dependencies: %w[firstName lastName birthDate],
      values: proc { |users| users.map { |u| "#{u['firstName']}/#{u['lastName']}/#{u['birthDate']}" } }
    )
  )

  # Implement 'In' filtering operator (required)
  collection.replace_field_operator('userIdentifier', Operators::IN) do |values, _context|
    {
      aggregator: 'Or',
      conditions: values.map do |value|
        {
          aggregator: 'And',
          conditions: [
            { field: 'firstName', operator: Operators::EQUAL, value: value.split('/')[0] },
            { field: 'lastName', operator: Operators::EQUAL, value: value.split('/')[1] },
            { field: 'birthDate', operator: Operators::EQUAL, value: value.split('/')[2] }
          ]
        }
      end
    }
  end
end

# Create relationship between databaseUsers and crmUsers
def create_relationship(collection)
  collection.add_one_to_one_relation(
    'userFromCrm',
    'crm_users',
    {
      origin_key: 'userIdentifier',
      origin_key_target: 'userIdentifier'
    }
  )
end

# Create relationship between crmUsers and databaseUsers
def create_inverse_relationship(collection)
  collection.add_many_to_one_relation(
    'userFromDatabase',
    'database_users',
    {
      foreign_key: 'userIdentifier',
      foreign_key_target: 'userIdentifier'
    }
  )
end

@create_agent.customize_collection('database_users', &method(:create_filterable_identity_field))
@create_agent.customize_collection('crm_users', &method(:create_filterable_identity_field))
@create_agent.customize_collection('database_users', &method(:create_relationship))
@create_agent.customize_collection('crm_users', &method(:create_inverse_relationship))
```

```python
from forestadmin.datasource_toolkit.datasource_customizer.collection_customizer import (
    CollectionCustomizer,
)
from forestadmin.datasource_toolkit.interfaces.fields import Operator
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.branch import (
    Aggregator,
)
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.branch import (
    ConditionTreeBranch,
)
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import (
    ConditionTreeLeaf,
)


# Concatenate firstname, lastname and birthData to make a unique identifier
# and ensure that the new field is filterable
def create_filterable_identity_field(collection: CollectionCustomizer):
    # Create foreign key on the collection from the database
    collection.add_field(
        "userIdentifier",
        {
            "column_type": "String",
            "dependencies": ["firstName", "lastName", "birthDate"],
            "get_values": lambda users, context: [
                f"{u['firstName']}/{u['lastName']}/{u['birthDate']}" for u in users
            ],
        },
    )

    # Implement 'In' filtering operator (required)
    collection.replace_field_operator(
        "userIdentifier",
        Operator.IN,
        lambda values, ctx: ConditionTreeBranch(
            Aggregator.OR,
            [
                ConditionTreeBranch(
                    Aggregator.AND,
                    [
                        ConditionTreeLeaf(
                            "firstName", Operator.EQUAL, value.split("/")[0]
                        ),
                        ConditionTreeLeaf(
                            "lastName", Operator.EQUAL, value.split("/")[1]
                        ),
                        ConditionTreeLeaf(
                            "birthDate", Operator.EQUAL, value.split("/")[2]
                        ),
                    ],
                )
                for value in values
            ],
        ),
    )


create_filterable_identity_field(agent.customize_collection("databaseUsers"))
create_filterable_identity_field(agent.customize_collection("crmUsers"))

# Create relationship between databaseUsers and crmUsers
agent.customize_collection("databaseUsers").add_one_to_one_relation(
    name="userFromCrm",
    foreign_collection="crmUsers",
    origin_key="userIdentifier",
    origin_key_target="userIdentifier",
)
# Create relationship between crmUsers and databaseUsers
agent.customize_collection("databaseUsers").add_many_to_one_relation(
    name="userFromCrm",
    foreign_collection="crmUsers",
    foreign_key="userIdentifier",
    foreign_key_target="userIdentifier",
)
```
