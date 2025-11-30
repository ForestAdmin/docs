![Value Chart example](../../assets/chart-value.png)

Value Charts display a single numerical value.

They can be added to a Dashboard using the `addChart` method on the `agent` object

<details>
<summary><strong>// Add a chart to the Dashboard page</strong></summary>

```javascript
agent.addChart('monthlyRecuringRevenue', async (context, resultBuilder) => {
  // Request the sum of the "amount" field of all the records in "payments"
  const aggregation = { operation: 'Sum', field: 'amount' };
  const filter = {
    conditionTree: { field: 'status', operator: 'equal', value: 'paid' },
  };

  const rows = await context.dataSource
    .getCollection('payments')
    .aggregate(filter, aggregation);

  // Return the result
  return resultBuilder.value(rows[0].value);
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceToolkit::Components::Query</strong></summary>

```ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.add_chart('monthlyRecuringRevenue') do |context, result_builder|
  aggregation = Aggregation.new(operation: 'Sum', field: 'amount')
  filter = Filter.new(condition_tree: Nodes::ConditionTreeLeaf.new('status', Operators::EQUAL, 'paid'))
  result = context.datasource.get_collection('payment').aggregate(filter, aggregation)

  result_builder.value(result[0]['value'])
end
```

</details>

<details>
<summary><strong>// Add a chart to the Analytics page of the "customers" Collection</strong></summary>

```javascript
agent.customizeCollection('customers', collection => {
  collection.addChart('monthlyRecuringRevenue', async (context, resultBuilder) => {
    // Request the sum of the "amount" field of records in "payments" matching
    // current customer
    const aggregation = { operation: 'Sum', field: 'amount' };
    const filter = {
      conditionTree: {
        aggregator: 'And',
        conditions: [
          { field: 'status', operator: 'equal', value: 'paid' },
          { field: 'customer:id', operator: 'equal', value: context.recordId },
        ],
      },
    };

    const rows = await context.dataSource
      .getCollection('payments')
      .aggregate(filter, aggregation);

    // Return the result
    return resultBuilder.value(rows[0].value);
  });
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceToolkit::Components::Query</strong></summary>

```ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('customer') do |collection|
  collection.add_chart('monthlyRecuringRevenue') do |context, result_builder|
    aggregation = Aggregation.new(operation: 'Count', field: 'id')
    filter = Filter.new(
      condition_tree: Nodes::ConditionTreeBranch.new(
        'And',
        [
          Nodes::ConditionTreeLeaf.new('status', Operators::EQUAL, 'paid'),
          Nodes::ConditionTreeLeaf.new('customer:id', Operators::EQUAL, context.get_record_id)
        ]
      )
    )
    result = context.datasource.get_collection('payment').aggregate(filter, aggregation)

    result_builder.value(result[0]['value'])
  end
end
```

</details>

<details>
<summary><strong>agent.addChart('appointments', async (context, resultBuilder) => {</strong></summary>

```javascript
  // [...]

  return resultBuilder.value(784, 760);
});
```

</details>

<details>
<summary><strong>@create_agent.add_chart('appointments') do |_context, result_builder|</strong></summary>

```ruby
  result_builder.value(784, 760)
end
```

</details>


