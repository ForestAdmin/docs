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

```javascript
// Many to one relationships
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

```php
// Model
class Product extends Model
{
    public function buyers(): SmartRelationship
    {
        return $this->smartRelationship(
            [
                'type' => ['String'],
                'reference' => 'customer.id'
            ]
        );
    }
    ...

// routes/web.php
Route::get('forest/product/{id}/relationships/buyers', [ProductsController::class, 'buyers']);

// Controller
class ProductsController extends ForestController
{
    public function buyers(int $id): JsonResponse
    {
        $query = Customer::whereHas('orders.products', fn ($query) => $query->where('products.id', $id))
            ->paginate($pageParams['size'] ?? null, '*', 'page', $pageParams['number'] ?? null);

        return response()->json(
            JsonApi::render($query, 'customers', ['count' => $query->total()])
        );
    }
}

```

```python
from app.models import Address
from django_forest.utils.collection import Collection

# Many to one relationships
class OrderForest(Collection):
    def load(self):
        self.fields = [
            {
                "field": "delivery_address",
                "type": "String",
                "reference": "Address.id",
                "get": self.get_delivery_address,
            }
        ]

    def get_delivery_address(self, obj):
        return obj.delivery_address

# Reverse relationship
class AddressForest(Collection):
    def load(self):
        self.fields = [
            {
                "field": "orders",
                "type": ["String"],
                "reference": "Order.id",
            }
        ]

# urls.py
from app.views import AddressOrders

urlpatterns = [
    path(
        '/forest/address/<pk>/relationships/orders',
        AddressOrdersView.as_view(),
        name='address_orders'
    ),
    # ...
]

# views.py
from django.http import JsonResponse
from django.views import generic
from django_forest.resources.utils.queryset import PaginationMixin
from django_forest.utils.schema.json_api_schema import JsonApiSchema

from app.models import Order


class AddressOrdersView(PaginationMixin, generic.ListView):
    def get(self, request, pk, *args, **kwargs):
        params = request.GET.dict()
        queryset = Orders.objects.filter(delivery_address_id=pk)

        # pagination
        queryset = self.get_pagination(params, queryset)

        # json api serializer
        Schema = JsonApiSchema.get('orders')
        data = Schema().dump(queryset, many=True)

        return JsonResponse(data, safe=False)
```

```ruby
# Model
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

{% endtab %} {% tab title="After" %}

```javascript
// Create the relationship
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

```php
// Create the relationship
$forestAgent->customizeCollection(
    'Product',
    function (CollectionCustomizer $builder) {
        $builder->addManyToOneRelation(name: 'buyers', foreignCollection: 'Customer', foreignKey: 'customerId');
    }
);

// Create the reverse relationship
$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->addOneToManyRelation(name: 'products', foreignCollection: 'Product', originKey: 'customerId');
    }
);
```

```python
# Create the relationship
agent.customize_collection("order").add_many_to_one_relation(
    "delivery_address", "Address", "delivery_address_id"
)

# Create the reverse relationship
agent.customize_collection("Address").add_one_to_many_relation(
    "orders", "Order", "delivery_address_id"
)
```

```ruby
module ForestAdminRails
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

```javascript
collection('order', {
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

```php
class ProductsController extends ForestController
{
    public function buyers(int $id): JsonResponse
    {
        $query = // complex query

        return response()->json(
            JsonApi::render($query, 'customers', ['count' => $query->total()])
        );
    }
}
```

```python
from app.models import Address
from django_forest.utils.collection import Collection

# Many to one relationships
class OrderForest(Collection):
    def load(self):
        self.fields = [
            {
                "field": "delivery_address",
                "type": "String",
                "reference": "Address.id",
                "get": self.get_delivery_address,
            }
        ]

    def get_delivery_address(self, obj):
        return Address.objects.filter( """complex_query""" )
```

```ruby
class Forest::ProductsController < ForestLiana::ApplicationController
  def buyers
    query = # complex query

    render json: serialize_models(query, meta: {count: query.count})
  end
end
```

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('order', orders => {
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

```php
$forestAgent->customizeCollection(
    'Product',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'customerId',
            new ComputedDefinition(
                columnType: 'Number',
                dependencies: ['id'],
                values: function ($customers, $context) {
                    ...
                }
            )
        )
        ->replaceFieldOperator(
            'customerId',
            'In',
            function ($customerIds, $context) {
                ...
            }
        )
        ->addManyToOneRelation(name: 'buyers', foreignCollection: 'Customer', foreignKey: 'customerId');
    }
);
```

```python
from typing import List, Dict

from app.models import Address
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias


def get_delivery_address_id(
    records: List[RecordsDataAlias], context: CollectionCustomizationContext
):
    addresses_by_order_id = Address.objects.filter("""complex_query_here""")
    return [addresses_by_order_id[order["id"]]["id"] for order in records]


# Create a computed field that will contain the address ID (the foreign key)
agent.customize_collection("order").add_field("delivery_address_id", {
    "column_type": "Number",
    "dependencies": ["id"],
    "get_values": get_delivery_address_id
}).replace_field_operator(
    # Make the field filterable (this is required for the relationship to work)
    "delivery_address_id", "in",
    lambda value, context: pass  # implement the reverse-lookup logic here
).add_many_to_one_relation(
    # Create the relationship
    "delivery_address", "Address", "delivery_address_id"
)
```

```ruby
module ForestAdminRails
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

{% endtab %} {% endtabs %}
