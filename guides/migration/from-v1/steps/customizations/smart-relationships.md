Smart relationships are very different between the two versions of the Agent.

{% hint style="info" %}
You can find the full documentation of relationship customization [here](../../../../agent-customization/relationships/README.md).
{% endhint %}

# Structure

Smart relationships on legacy agents were declared creating a smart field with a `reference` property but differed in the way that:

- Relationships to a single record (many-to-one or one-to-one) worked using the `get` function which needed to return a single record.
- Relationships to a list of records (one-to-many or many-to-many) worked by implementing all the CRUD routes on a router file.

The new system is completely different: it is based on primary keys and foreign keys.

# Migrating

## Relationships when the foreign key is accessible

{% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="// Many to one relationships" %}
```javascript
collection('order', {
  fields: [
    {
      field: 'delivery_address',
      type: 'String',
      reference: 'Address._id',
      get: async order => {
        return models.addresses.find({ id: order.delivery_address_id });
      },
    },
  ],
});

// Reverse relationship
collection('address', {
  fields: [{ field: 'orders', type: ['String'], reference: 'Order.id' }],
});

router.get('/address/:id/relationships/orders', (req, res) => {
  // ... route handler
});

// ... other routes
```
{% endtab %}

{% tab title="# Model" %}
```ruby
class Forest::Product
  include ForestLiana::Collection

  collection :Product

  has_many :buyers, type: ['String'], reference: 'Customer.id'
end

# routes.rb
namespace :forest do
  get '/Product/:product_id/buyers' => 'orders#buyers'
end

mount ForestLiana::Engine => '/forest'

# Controller
class Forest::ProductsController < ForestLiana::ApplicationController
  def buyers
    limit = params['page']['size'].to_i
    offset = (params['page']['number'].to_i - 1) * limit

    product = Product.find(params['product_id'])
    customers = Customer.where(order_id: product.orders.ids)

    render json: serialize_models(customers.limit(limit).offset(offset), meta: {count: customers.count})
  end
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="// Create the relationship" %}
```javascript
agent.customizeCollection('order', orders => {
  orders.addManyToOneRelation('deliveryAddress', 'address', {
    foreignKey: 'deliveryAddressId',
  });
});

// Create the reverse relationship
agent.customizeCollection('address', addresses => {
  addresses.addOneToManyRelation('orders', 'order', {
    originKey: 'deliveryAddressId',
  });
});
```
{% endtab %}

{% tab title="module ForestAdminRails" %}
```ruby
class CreateAgent
    def self.setup!
      @create_agent.customize_collection('Product') do |collection|
        collection.add_many_to_one_relation('buyers', 'Customer', { foreign_key: 'country_id' })
      end

      @create_agent.customize_collection('Customer') do |collection|
        collection.add_one_to_many_relation('products', 'Product', { origin_key: 'country_id' })
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

{% endtab %} {% endtabs %}

## Relationships when you need complex logic to get a foreign key

In this example, we want to create a relationship between the `order` collection and the `address` collection
(assuming that it does not already exist in the database because depends on complex logic).

We can see that in the legacy agent, the `delivery_address` field was a smart field that returned the full address of the order, while in the new agent, we will create a computed field that will contain the address ID (the foreign key), and then create the relationship.

We won't be detailing the migration of a relation to a list of records here, but it is very similar to the one described below.

{% hint style="info" %}
If the foreign key was already present in the database in a related table, use the [import-rename-delete](../../../../agent-customization/fields/import-rename-remove.md) feature to move it to the correct collection instead of using a computed field.

This will be much faster and will not require `In` filter operators to be implemented (as unlike computed fields, imported fields are natively filterable and sortable).
{% endhint %}

{% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="collection('order', {" %}
```javascript
fields: [
    {
      field: 'delivery_address',
      type: 'String',
      reference: 'Address._id',
      get: async order => {
        return models.addresses.find(/* complex query */);
      },
    },
  ],
});
```

</details>

<details>
<summary><strong>class Forest::ProductsController < ForestLiana::ApplicationController</strong></summary>

```ruby
  def buyers
    query = # complex query

    render json: serialize_models(query, meta: {count: query.count})
  end
end
```
{% endtab %}

{% tab title="agent.customizeCollection('order', orders => {" %}
```javascript
// Create a computed field that will contain the address ID (the foreign key)
  orders.addField('deliveryAddressId', {
    columnType: 'Number',
    dependencies: ['id'],
    getValues: async orders => {
      const addressByOrderId = await models.addresses.find(/* complex query */);

      return orders.map(order => addressByOrderId[order.id].id);
    },
  });

  // Make the field filterable (this is required for the relationship to work)
  orders.replaceFieldOperator('deliveryAddressId', 'In', (value, context) => {
    // implement the reverse-lookup logic here
  });

  // Create the relationship
  orders.addManyToOneRelation('deliveryAddress', 'address', {
    foreignKey: 'deliveryAddressId',
  });
});
```
{% endtab %}
{% endtabs %}

<details>
<summary><strong>module ForestAdminRails</strong></summary>

```ruby
  class CreateAgent
    include ForestAdminDatasourceCustomizer::Decorators::Computed
    include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

    def self.setup!
      @create_agent.customize_collection('Product') do |collection|
        collection.add_field(
          'customerId',
          ComputedDefinition.new(
           column_type: 'Number',
            dependencies: ['id'],
            values: proc { |customers, context|
              ...
           }
          )
        )
        .replace_field_operator('customerId', Operators::IN) { |customer_ids, context|
          ...
        }
        .add_many_to_one_relation('buyers', 'Customer', { foreign_key: 'customerId' })
      end
    end
  end
end
```

</details>

{% endtab %} {% endtabs %}
