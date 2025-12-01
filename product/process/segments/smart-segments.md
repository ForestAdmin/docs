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

{{#nodejs}}
{% hint style='info' %}
In the following example, we are making queries using the [Forest Admin Query Interface](../datasources/getting-started/README.md).

As Forest Admin does not impose any restriction on the handler, you are free to call external APIs or query your database directly instead.
{% endhint %}
{{/nodejs}}

The only requirement when implementing a Segment from your agent is to return a valid `ConditionTree`{{#nodejs}} (see [Understanding Filters](../datasources/getting-started/queries/filters.md)){{/nodejs}}.

{% tabs %}
{% tab title="agent.customizeCollection('products', collection =>" %}
```javascript
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
{% endtab %}

{% tab title="include ForestAdminDatasourceToolkit::Components::Query" %}
```ruby
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
{% endtab %}
{% endtabs %}


