Smart Segments should be quick to migrate, as the syntax is very similar to the legacy agent.

{% hint style="info" %}
You can find the full documentation of segment customization [here](../../../../agent-customization/segments.md).
{% endhint %}

{{#nodejs}}

# Code cheatsheet

| Legacy agent    | New agent                                                                                    |
| --------------- | -------------------------------------------------------------------------------------------- |
| where:          | handler body                                                                                 |
| sequelize.where | [condition tree](../../../../datasources/getting-started/queries/filters.md#condition-trees) |

{{/nodejs}}

# How to migrate

## Structure

Because the new Forest Admin agent is designed to work with multiple databases, the return value of the filter function is not a Sequelize or Mongoose condition anymore.

Instead, you'll be building a [condition tree](../../../../datasources/getting-started/queries/filters.md#condition-trees) that will be translated to the appropriate database syntax by the agent.

## Performance

All queries cannot be expressed in the Forest Admin query interface, but many can.

You can have great performance improvements by using the Forest Admin query interface to build your conditions, instead of performing the query yourself, and then building a naive condition tree, which filters by primary key like in the example we're providing.

## Example

In this example, we migrate a segment that returns the 5 bestsellers of a product collection.

{% tabs %} {% tab title="Before" %}

<details>
<summary><strong>collection('products', {</strong></summary>

```javascript
  segments: [
    {
      name: 'Bestsellers',
      where: async product => {
        const query = `
        SELECT products.id, COUNT(orders.*)
        FROM products
        JOIN orders ON orders.product_id = products.id
        GROUP BY products.id
        ORDER BY count DESC
        LIMIT 5;
      `;

        const products = await models.connections.default.query(query, {
          type: QueryTypes.SELECT,
        });

        return { id: { [Op.in]: products.map(product => product.id) } };
      },
    },
  ],
});
```

</details>

<details>
<summary><strong>class Forest::Product</strong></summary>

```ruby
  include ForestLiana::Collection

  collection :Product

    segment 'Bestsellers' do
      query = <<~SQL
        SELECT products.id, COUNT(orders.*)
        FROM products
        JOIN orders ON orders.product_id = products.id
        GROUP BY products.id
        ORDER BY count DESC
        LIMIT 5;
      SQL

      products = ActiveRecord::Base.connection.execute(query)
      { id: products.map { |product| product['id'] } }
    end
end
```

</details>

<details>
<summary><strong>agent.customizeCollection('products', products => {</strong></summary>

```javascript
  products.addSegment('Bestsellers', async () => {
    const query = `
      SELECT products.id, COUNT(orders.*)
      FROM products
      JOIN orders ON orders.product_id = products.id
      GROUP BY products.id
      ORDER BY count DESC
      LIMIT 5;
    `;

    const products = await models.connections.default.query(query, {
      type: QueryTypes.SELECT,
    });

    return { field: 'id', operator: 'In', value: products.map(p => p.id) };
  });
});
```

</details>

<details>
<summary><strong>module ForestAdminRails</strong></summary>

```ruby
  class CreateAgent
    def self.setup!
      @create_agent.customize_collection('product') do |collection|
        collection.add_segment('best_sellers') do |context|
          rows = ActiveRecord::Base.connection.execute(
            'SELECT products.id as product_id, COUNT(orders.id) as nb
             FROM products
             INNER JOIN orders ON orders.product_id = products.id
             GROUP BY products.id
             ORDER BY nb DESC
             LIMIT 5;'
          )

          {
            field: 'id',
            operator: 'In',
            value: rows.map { |r| r['product_id'] }
          }
        end
      end
    end
  end
end
```

</details>

{% endtab %} {% endtabs %}
