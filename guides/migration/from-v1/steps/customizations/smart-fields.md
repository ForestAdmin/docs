In legacy agents declaring a smart field was done in one big step.

In the new agent, the process was split into multiple steps, depending on the capabilities of the field (writing, filtering, sorting, etc.).

This was done to reduce the complexity of the code and to make it easier to understand, but also because it allows customers to reuse the same API when customizing the behavior of normal fields, and thus reduce the API surface that you need to learn.

{% hint style="info" %}
You can find the full documentation of field customization [here](../../../../agent-customization/fields/README.md).
{% endhint %}

# Code cheatsheet

| Legacy agent                              | New agent                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| get: (record) => { ... }                  | {{#nodejs,php}}getValues: (records) => { ... }{{/nodejs,php}}{{#python}}"get_values": lambda records : ...{{/python}}{{#ruby}}"values": proc { \|records\| ... }{{/ruby}}                                                                                                                                                                            |
| set: (record, value) => { ... }           | .{{#nodejs,php}}replaceFieldWriting{{/nodejs,php}}{{#python,ruby}}replace_field_writing{{/python,ruby}}(...)                                                                                                                                                                                                                                         |
| filter: ({ condition, where }) => { ... } | .{{#nodejs,php}}replaceFieldOperator{{/nodejs,php}}{{#python,ruby}}replace_field_operator{{/python,ruby}}(...)<br>.{{#nodejs,php}}emulateFieldOperator{{/nodejs,php}}{{#python,ruby}}emulate_field_operator{{/python,ruby}}(...)<br>.{{#nodejs,php}}emulateFieldFiltering{{/nodejs,php}}{{#python,ruby}}emulate_field_filtering{{/python,ruby}}(...) |
| type: 'String'                            | columnType: 'String'                                                                                                                                                                                                                                                                                                                                 |
| enums: ['foo', 'bar']                     | {{#nodejs,php}}columnType: 'Enum', enumValues: ['foo', 'bar']{{/nodejs,php}}{{#python,ruby}}"column_type": "Enum", "enum_values": ['foo', 'bar']{{/python,ruby}}                                                                                                                                                                                     |
| reference: 'otherCollection.id'           | [Use a smart relationship](./smart-relationships.md)                                                                                                                                                                                                                                                                                                 |

# Do you still need a computed field?

Smart fields were a powerful tool, but they were also a performance bottleneck.

In the new agent, we have introduced two new concepts that can replace many of the use cases of smart fields:

If you were using a smart field to move a field from one collection to another or to create a link to another record in the UI, you can likely use one of these much simpler solutions.

- [Moving fields](../../../../agent-customization/fields/import-rename-remove.md#moving-fields)
- [Relationships](../../../../agent-customization/relationships/README.md)

# Steps

## Step 1: Implement a read-only version of the field

Computed fields in the new agent are declared by calling the {{#nodejs,php}}`addField`{{/nodejs,php}}{{#python,ruby}}`add_field`{{/python,ruby}} function on the appropriate collection.

Many changes have been made to the API.

### Dependencies are explicit

You will notice that a new `dependencies` property is required when declaring a computed field.

It is an array of field names that tells Forest Admin which fields the {{#nodejs,php}}`getValues()`{{/nodejs,php}}{{#python}}`get_values()`{{/python}}{{#ruby}}`values`{{/ruby}} function depends on. Unlike the legacy agent, the new agent will not automatically fetch the whole record.

You can [fetch data from relations](../../../../agent-customization/fields/computed.md#adding-a-field-that-depends-on-a-many-to-one-relationship) and [fetch data from other computed fields](../../../../agent-customization/fields/computed.md#adding-a-field-that-depends-on-another-computed-field).

### Fields now work in batches

Even if it adds some complexity, exposing a batch API to our customers is a much better solution for performance.

{{#nodejs,php,python}}The `get` function is now called {{#nodejs,php}}`getValues`{{/nodejs,php}}{{#python}}`get_values`{{/python}}: it no longer takes a single record as its first argument, but an array of records, and must return an array of values, one for each record, in the same order.{{/nodejs,php,python}}
{{#ruby}}The function now returns a value called `values` which is an array of values, one for each record, in the same order. It no longer takes a single record as its first argument, but an array of records.{{/ruby}}

### Other changes

There are other minor changes to the API:

- the `type` property was renamed to {{#nodejs,php}}`columnType`{{/nodejs,php}}{{#python,ruby}}`column_type`{{/python,ruby}}.
- The `field` property no longer exists. The field name is now the first argument of the {{#nodejs,php}}`addField`{{/nodejs,php}}{{#python,ruby}}`add_field`{{/python,ruby}} function.
- The `reference` property no longer exists: use the [smart relationships guide](./smart-relationships.md).
- The `enums` property was renamed to {{#nodejs,php}}`enumValues`{{/nodejs,php}}{{#python,ruby}}`enum_values`{{/python,ruby}}.

### Example

In the following example, we will port a field that fetches the full address of a user from a third-party service.

Note that when displaying a list of records, the new agent will only make one call to your handler, and then display the results for all records, instead of making one call per record.

{% tabs %} {% tab title="Before" %}

```javascript
collection('users', {
  fields: [
    {
      field: 'full_address',
      type: 'String',
      get: async user => {
        const addr = await geoWebService.getAddress(customer.address_id);

        return [addr.line_1, addr.line_2, addr.city, addr.country].join('\n');
      },
    },
  ],
});
```

```php
public function fullAddress(): SmartField
{
    return $this->smartField(['type' => 'String'])
        ->get(
            function () {
                $address = Address::firstWhere('customer_id', $this->id);

                return "$address->address_line1  $address->address_line2 $address->address_city  $address->country";
            }
        );
}
```

```python
from django_forest.utils.collection import Collection

class UsersForest(Collection):
    def load(self):
        self.fields = [
            {
                "field": "full_address",
                "type": "String",
                "get": self.get_full_address,
            }
        ]

    def get_full_address(self, obj):
        return "\n".join([
            obj.address_line_1,
            obj.address_line_2,
            obj.address_city,
            obj.address_country
        ])
```

```ruby
class Forest::User
  collection :User

  field :full_address, type: 'String' do
    addr = GeoWebService.get_address(user.address_id)
    [addr.line_1, addr.line_2, addr.city, addr.country].join("\n")
  end
end
```

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('users', users => {
  users.addField('full_address', {
    columnType: 'String',
    dependencies: ['address_id'],
    getValues: users =>
      users.map(async user => {
        const addr = await geoWebService.getAddress(customer.address_id);

        return [addr.line_1, addr.line_2, addr.city, addr.country].join('\n');
      }),
  });
});
```

```php
$forestAgent->customizeCollection(
    'Address',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'fullAddress',
            new ComputedDefinition(
                columnType: 'String',
                dependencies: ['line1', 'line2', 'city', 'country'],
                values: fn ($records) => collect($records)->map(fn ($record) => $record['line1'] . ' ' . $record['line2'] . ' ' . $record['city'] . ' ' . $record['country']),
            )
        );
    }
);
```

```python
from typing import List

from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias


def get_customer_full_address(
    records: List[RecordsDataAlias], context: CollectionCustomizationContext
):
    return [
        "\n".join(
            [
              rec["address_line_1"],
              rec["address_line_2"],
              rec["address_city"],
              rec["address_country"],
            ]
        ) for rec in records
    ]

agent.customize_collection("customer").add_field("full_address", {
    "column_type": "String",
    "dependencies": [
        "address_line_1", "address_line_2", "address_city", "address_country"
    ],
    "get_values": get_customer_full_address,
})
```

```ruby
module ForestAdminRails
  class CreateAgent
    include ForestAdminDatasourceCustomizer::Decorators::Computed

    def self.customize
      @create_agent.customize_collection('customer') do |collection|
        collection.add_field(
          'full_address',
          ComputedDefinition.new(
            column_type: 'String',
            dependencies: ['address_line_1', 'address_line_2', 'address_city', 'address_country'],
            values: proc { |records| records.map { |record| "#{record['address_line_1']} #{record['address_line_2']} #{record['address_city']} #{record['address_country']}" } }
          )
        )
      end
    end
  end
end
```

{% endtab %} {% endtabs %}

## Step 2: Implement write handler

If you want your computed field to be writable, you will need to call the {{#nodejs,php}}`.replaceFieldWriting()`{{/nodejs,php}}{{#python,ruby}}`.replace_field_writing()`{{/python,ruby}} function.

This part is very similar to the legacy agent. The API change is because this function can be used to make any field writable, not just computed fields, or to change the default writing behavior of a normal field.

{% tabs %} {% tab title="Before" %}

```javascript
collection('users', {
  fields: [
    {
      field: 'full_address',
      type: 'String',
      get: /* ... same as before ... */,
      set: async (user, value) => {
        const address = await geoWebService.getAddress(customer.address_id);
        address.line_1 = value.split('\n')[0];
        address.line_2 = value.split('\n')[1];
        address.city = value.split('\n')[2];
        address.country = value.split('\n')[3];

        await geoWebService.updateAddress(address);

        // You can optionally return a hash of attributes to update the record
        return {};
      },
    },
  ],
});
```

```php
public function fullAddress(): SmartField
{
    return $this->smartField(['type' => 'String'])
        ->get(...)
        ->set(
            function ($value) {
                [$line1, $line2, $city, $country] = explode(' ', $value);
                $this->line1 = $line1;
                $this->line2 = $line2;
                $this->city = $city;
                $this->country = $country;

                return $this;
            }
        );
}
```

```python
from django_forest.utils.collection import Collection

class UsersForest(Collection):
    def load(self):
        self.fields = [
            {
                "field": "full_address",
                "type": "String",
                "get": self.get_full_address,
                "set": self.set_full_address,
            }
        ]

    def set_full_name(self, obj, value):
        obj.address_line_1 = value.split('\n')[0]
        obj.address_line_2 = value.split('\n')[1]
        obj.address_city = value.split('\n')[2]
        obj.address_country = value.split('\n')[3]
        return obj

    def get_full_address(self, obj):
        # ...

```

```ruby
class Forest::User
  collection :User

  set_full_address = lambda do |address_params, full_address|
    full_address = full_address.split("\n")
    address_params[:line_1] = full_address[0]
    address_params[:line_2] = full_address[1]
    address_params[:city] = full_address[2]
    address_params[:country] = full_address[3]

    address_params
  end

  field :full_address, type: 'String', set: set_full_address do
    ...
  end
end
```

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('users', users => {
  users
    .addField('full_address', { /* ... same as before ... */ })
    .replaceFieldWriting('full_address', (value) => {
      const address = await geoWebService.getAddress(customer.address_id);
      address.line_1 = value.split('\n')[0];
      address.line_2 = value.split('\n')[1];
      address.city = value.split('\n')[2];
      address.country = value.split('\n')[3];

      await geoWebService.updateAddress(address);

      // You can optionally return a hash of attributes to update the record
      return {};
    });
});
```

```php
$forestAgent->customizeCollection(
    'Address',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldWriting(
        	'fullAddress',
        	function($value) {
        		[$line1, $line2, $city, $country] = explode(' ', $value);

        		return compact('line1', 'line2', 'city', 'country);
        	}
        );
    }
);
```

```python
from typing import Any, List

from forestadmin.datasource_toolkit.decorators.write.write_replace.write_customization_context import (
    WriteCustomizationContext,
)


def set_customer_full_address(value: Any, context: WriteCustomizationContext):
    return {
        "address_line_1": value.split('\n')[0],
        "address_line_2": value.split('\n')[1],
        "address_city": value.split('\n')[2],
        "address_country": value.split('\n')[3],
    }

agent.customize_collection("customer").add_field("full_address", {
    # ... same as before
}).replace_field_writing(
    "full_address", set_customer_full_address
)
```

```ruby
module ForestAdminRails
  class CreateAgent
    include ForestAdminDatasourceCustomizer::Decorators::Computed

    def self.customize
      @create_agent.customize_collection('customer') do |collection|
        collection.add_field(
          'full_address',
         #... same as before
        )
        .replace_field_writing('full_address') do |value, context|
          {
            address_line_1: value.split("\n")[0],
            address_line_2: value.split("\n")[1],
            address_city: value.split("\n")[2],
            address_country: value.split("\n")[3]
          }
        end
      end
    end
  end
end
```

{% endtab %} {% endtabs %}

## Step 3: Implement the filters you use

### Structure

Implementing filters in the new agent is done operator by operator, instead of using a single function.

This allows more fine-grained control over the behavior of each operator and makes it possible to implement only the operators you need.

Also note that unlike in the legacy agent, [automatic operator replacement](../../../../datasources/getting-started/queries/filters.md#operator-equivalence) will be performed by the agent, so the number of operators that you need to implement to unlock all filtering is much lower.

### Return value

Because the new Forest Admin agent is designed to work with multiple databases, the return value of the filter function is not a Sequelize or Mongoose condition anymore.

Instead, you'll be building a [condition tree](../../../../datasources/getting-started/queries/filters.md#condition-trees) that will be translated to the appropriate database syntax by the agent.

### Emulation

At the cost of performance, you can tell the agent to [emulate](../../../../agent-customization/fields/filter.md#emulation) the behavior of a given operator by calling the {{#nodejs,php}}`.emulateFieldOperator()`{{/nodejs,php}}{{#python}}`.emulate_field_operator()`{{/python}} function.

### Example

{% tabs %} {% tab title="Before" %}

```javascript
collection('users', {
  fields: [
    {
      field: 'full_address',
      type: 'String',
      get: /* ... same as before ... */,
      filter: ({ condition, where }) => {
        switch (condition.operator) {
          case 'equal':
            return { $and: [/* ... Sequelize or Mongoose conditions ... */] };
          case 'less_than':
            return { $and: [/* ... Sequelize or Mongoose conditions ... */] };

          // [...] all other operators
        }
      },
    },
  ],
});
```

```php
public function fullAddress(): SmartField
{
    return $this->smartField(['type' => 'String'])
        ->get(/* ... same as before ... */)
        ->filter(
            function (Builder $query, $value, string $operator, string $aggregator) {
                $data = explode(' ', $value);
                switch ($operator) {
                    case 'equal':
                        $query->where(...);
                        break;
                    case 'less_than':
                        $query->where(...);
                        break;
                   // [...] all other operators
                }

                return $query;
            }
        );
}
```

```python
from django.db.models import Q
from django_forest.utils.collection import Collection

class UsersForest(Collection):
    def load(self):
        self.fields = [
            {
                "field": "full_address",
                "type": "String",
                "get": self.get_full_address,
                "set": self.set_full_address,
                "filter": self.filter_full_name,
            }
        ]

    def filter_full_name(self, operator, value):
        if operator == "equal":
            return Q(...)
        elif operator == "less_than":
            return Q(...)
        # [...] all other operators


    def set_full_name(self, obj, value):
        # ...

    def get_full_address(self, obj):
        # ...

```

```ruby
class Forest::User
  collection :User

  def filter_fullname(operator, value)
    case operator
    when 'equal'
      # ...
    when 'less_than'
      # ...
    end
  end

  field :full_address, type: 'String', filter: filter_fullname do
    # same as before
  end
end
```

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('users', users => {
  users
    .addField('full_address', {
      /* ... same as before ... */
    })

    // Implement the operators you actually use for best performance.
    .replaceFieldOperator('full_address', 'Equal', (value, context) => ({
      aggregator: 'And',
      conditions: [],
    }))
    .replaceFieldOperator('full_address', 'LessThan', (value, context) => ({
      aggregator: 'And',
      conditions: [],
    }))

    // Emulate other operators.
    .emulateFieldOperator('full_address', 'GreaterThan')
    .emulateFieldOperator('full_address', 'NotEqual')

    // [Or] Emulate all operators which are not already defined in one call.
    .emulateFieldFiltering('full_address');
});
```

```php
$forestAgent->customizeCollection(
    'Address',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            /* ... same as before ... */
        )
        ->replaceFieldOperator('fullAddress', Operators::EQUAL, ...)
        ->replaceFieldOperator('fullAddress', Operators::LESS_THAN, ...)

        // Emulate other operators.
        ->emulateFieldOperator('fullName', Operators::GREATER_THAN)
        ->emulateFieldOperator('fullName', Operators::NOT_EQUAL)

        // [Or] Emulate all operators which are not already defined in one call.
        ->emulateFieldFiltering('fullName');
    }
);
```

```python
from typing import Any, List

from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext

def full_address_operator_equal(value, context: CollectionCustomizationContext):
    pass


agent.customize_collection("customer").add_field("full_address", {
    # ... same as before
}).replace_field_writing(
    # ... same as before
).replace_field_operator(
    "full_address", "equal", full_address_operator_equal
).replace_field_operator(
    "full_address", "less_than", lambda value, context: ...

# Emulate other operators.
).emulate_field_operator(
    "full_address", "greater_than"
).emulate_field_operator(
    "full_address", "not_equal"

# [Or] Emulate all operators which are not already defined in one call.
).emulate_field_filtering("full_address")
```

```ruby
module ForestAdminRails
  class CreateAgent
    include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

    def self.customize
      @create_agent.customize_collection('customer') do |collection|
        collection.add_field(
          'full_address',
          # ... same as before
        )
        .replace_field_operator('full_address', Operators::EQUAL) do |value, context|
          # ...
        end
        .replace_field_operator('full_address', Operators::LESS_THAN) do |value, context|
          # ...
        end

        # Emulate other operators.
        .emulate_field_operator('full_address', Operators::GREATER_THAN)
        .emulate_field_operator('full_address', Operators::NOT_EQUAL)

        # [Or] Emulate all operators which are not already defined in one call.
        .emulate_field_filtering('full_address')
      end
    end
  end
end
```

{% endtab %} {% endtabs %}
