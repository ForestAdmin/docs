A Segment is a subset of a Collection: it's a saved filter of your Collection.

Segments are designed for those who want to _systematically_ visualize data according to specific sets of filters. It allows you to save filter configurations so user don’t have to do repetitive actions every day.

![](../assets/segment-example.png)

# From your admin panel

Segments can be configured from the interface, without the need to write any code.

This is documented in the [User Guide](https://docs.forestadmin.com/user-guide/collections/segments).

# From your Agent

Sometimes, Segment filters are complicated and closely tied to your business. Forest Admin allows you to code how the Segment is computed.

For instance, in our [Live Demo example](https://app.forestadmin.com/livedemo), we’ve implemented a Segment on the collection `products` to allow admin users to see the bestsellers at a glance.

## Example

{{#php,nodejs}}
{% hint style='info' %}
In the following example, we are making queries using the [Forest Admin Query Interface](../datasources/getting-started/README.md).

As Forest Admin does not impose any restriction on the handler, you are free to call external APIs or query your database directly instead.
{% endhint %}
{{/php,nodejs}}

The only requirement when implementing a Segment from your agent is to return a valid `ConditionTree`{{#php,nodejs}} (see [Understanding Filters](../datasources/getting-started/queries/filters.md)){{/php,nodejs}}.

```javascript
agent.customizeCollection('products', collection =>
  collection.addSegment('mostPurchased', async context => {
    // Query the ids of the 10 most ordered products.
    const rows = await context.dataSource
      .getCollection('orders')
      .aggregate({}, { operation: 'Count', groups: [{ field: 'product_id' }] }, 10);

    // Return a condition tree which matches those records
    return { field: 'id', operator: 'In', value: rows.map(r => r['product_id']) };
  });
);
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Context\CollectionCustomizationContext;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->addSegment(
            'mostPurchased',
            function (CollectionCustomizationContext $context) {
                $rows = $context->getDatasource()->getCollection('Order')->aggregate(
                    new Filter(),
                    new Aggregation(operation: 'Count', groups: [['field' => 'category_id']]),
                    10
                );

                return [
                    'field'    => 'id',
                    'operator' => Operators::IN,
                    'value'    => array_map(fn ($r) => $r['product_id'], $rows)
                ];
            }
        );
    }
);
```

```ruby
include ForestAdminDatasourceToolkit::Components::Query
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('customer') do |collection|
  collection.add_segment('mostPurchased') do |context|
    rows = context.datasource.get_collection('order').aggregate(
      Filter.new,
      Aggregation.new(operation: 'Count', groups: [{ field: 'product_id' }]),
      10
    )

    {
      field: 'id',
      operator: Operators::IN,
      value: rows.map { |r| r['product_id'] }
    }
  end
end
```

```python
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.query.aggregation import Aggregation
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf

async def most_purchased_segment(context: CollectionCustomizationContext):
    # Query the ids of the 10 most populate products
    # by looking at the `orders` collection.
    rows = await context.datasource.get_collection("order").aggregate(
        context.caller,
        Aggregation({"operation": "Count", "groups": [{"field": "product_id"}]}),
        10,
    )

    # Return a condition tree which matches those records
    return ConditionTreeLeaf("id", "in", [row["product_id"] for row in rows])


agent.customize_collection("products").add_segment(
    "mostPurchased", most_purchased_segment
)
```
