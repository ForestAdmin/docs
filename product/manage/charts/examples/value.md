![Value Chart example](../../assets/chart-value.png)

Value Charts display a single numerical value.

They can be added to a Dashboard using the `addChart` method on the `agent` object

```javascript
// Add a chart to the Dashboard page
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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\ConditionTreeFactory;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;

$forestAgent->addChart(
    'monthlyRecuringRevenue',
    function (AgentCustomizationContext $context, ResultBuilder $resultBuilder) {
        $aggregation = new Aggregation('Sum', 'amount');
        $filter = new Filter(
            ConditionTreeFactory::fromArray(['field' => 'status', 'operator' => Operators::EQUAL, 'value' => paid])
        );
        $value = $context->getDatasource()->getCollection('Payment')->aggregate($filter, $aggregation);

        return $resultBuilder->value($value)
    }
);

```

```ruby
include ForestAdminDatasourceToolkit::Components::Query
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.add_chart('monthlyRecuringRevenue') do |context, result_builder|
  aggregation = Aggregation.new(operation: 'Sum', field: 'amount')
  filter = Filter.new(condition_tree: Nodes::ConditionTreeLeaf.new('status', Operators::EQUAL, 'paid'))
  result = context.datasource.get_collection('payment').aggregate(filter, aggregation)

  result_builder.value(result[0]['value'])
end
```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart
from forestadmin.datasource_toolkit.interfaces.query.aggregation import Aggregation
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf

async def monthly_recuring_revenue_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    aggregation = Aggregation({"field": "amount", "operation": "Count"})
    filter_ = Filter(
        {"condition_tree": ConditionTreeLeaf("status", "equal", "paid")}
    )
    rows = await context.datasource.get_collection("Payment").aggregate(
        context.caller, filter_, aggregation
    )
    return result_builder.value(rows[0]["value"])

agent.add_chart("monthlyRecuringRevenue", monthly_recuring_revenue_chart_fn)
```

Or to the "Analytics" Tab of a Collection using the `addChart` method on the `collection` object

```javascript
// Add a chart to the Analytics page of the "customers" Collection
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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\CollectionChartContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\ConditionTreeFactory;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;

$forestAgent->customizeCollection('Customer', fn(CollectionCustomizer $builder) => $builder->addChart(
    'monthlyRecuringRevenue',
    function (CollectionChartContext $context, ResultBuilder $resultBuilder) {
        $aggregation = new Aggregation('Count', 'id');
        $filter = new Filter(
            ConditionTreeFactory::fromArray(
                [
                    'aggregator' => 'And',
                    'conditions' => [
                        ['field' => 'status', 'operator' => Operators::EQUAL, 'value' => 'paid']
                        ['field' => 'customer:id', 'operator' => Operators::EQUAL, 'value' => $context->getRecordId()]
                    ]
                ]
            )
        );
        $value = $context->getDatasource()->getCollection('Payment')->aggregate($filter, $aggregation);

        return $resultBuilder->value($value)
    }
);
```

```ruby
include ForestAdminDatasourceToolkit::Components::Query
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

```python
from forestadmin.datasource_toolkit.decorators.chart.collection_chart_context import CollectionChartContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart
from forestadmin.datasource_toolkit.interfaces.query.aggregation import Aggregation
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.branch import ConditionTreeBranch

async def monthly_recuring_revenue_chart_fn(
    context: CollectionChartContext, result_builder: ResultBuilder
) -> Chart:
    aggregation = Aggregation({"field": "amount", "operation": "Sum"})
    filter_ = Filter(
        {
            "condition_tree": ConditionTreeBranch(
                "and",[
                    ConditionTreeLeaf(
                        "customer:id", "equal", await context.get_record_id()
                    ),
                    ConditionTreeLeaf("status", "equal", "paid"),
                ]
            )
        }
    )
    rows = await context.datasource.get_collection("Payment").aggregate(
        context.caller, filter_, aggregation
    )
    return result_builder.value(rows[0]["value"])

agent.customize_collection("Customer").add_chart(
    "monthlyRecuringRevenue", monthly_recuring_revenue_chart_fn
)
```

Optionally, an older value can be provided to the `resultBuilder` to display a growth percentage on the top right of the widget as in the following Chart display:

![Value chart with percentage example](../../assets/chart-value-percentage.png)

```javascript
agent.addChart('appointments', async (context, resultBuilder) => {
  // [...]

  return resultBuilder.value(784, 760);
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\AgentCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Chart\ResultBuilder;

$forestAgent->addChart(
    'appointments',
    function (AgentCustomizationContext $context, ResultBuilder $resultBuilder) {
        return $resultBuilder->value(784, 760)
    }
);
```

```ruby
@create_agent.add_chart('appointments') do |_context, result_builder|
  result_builder.value(784, 760)
end
```

```python
from forestadmin.datasource_toolkit.context.agent_context import AgentCustomizationContext
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.chart import Chart

async def appointments_chart_fn(
    context: AgentCustomizationContext, result_builder: ResultBuilder
) -> Chart:
    # ...
    return result_builder.value(784, 760)

agent.add_chart("appointments", appointments_chart_fn)
```
