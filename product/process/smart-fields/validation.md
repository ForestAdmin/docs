![A field failing validation](../../assets/field-validation-error.png)
{{#nodejs}}

Most data sources can import validation rules from their target.
For instance, if you are using the [SQL data source](../../datasources/provided/sql.md).

{{/nodejs}}

- Columns of type `VARCHAR(15)` will automatically carry a `less than 15 chars` validator.
- Non-nullable columns will automatically carry a `Present` validator.

However, you may want to enforce stricter restrictions than the ones which are implemented in your data source.

## Adding validation rules

The list of operators (`Present`, `LongerThan`, ...) which can be used when adding validators is the same as the [filter operators](../../datasources/getting-started/queries/filters.md#operators).

```javascript
collection
  .addFieldValidation('firstName', 'Present')
  .addFieldValidation('firstName', 'LongerThan', 2)
  .addFieldValidation('firstName', 'ShorterThan', 13)
  .addFieldValidation('firstName', 'Match', /^[a-z]+$/i);
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->addFieldValidation('firstName', Operators::PRESENT])
        	->addFieldValidation('firstName', Operators::LONGER_THAN, 2)
            ->addFieldValidation('firstName', Operators::SHORTER_THAN, 13);
    }
);
```

```ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('customer') do |collection|
  collection.add_field_validation('firstName', Operators::PRESENT)
    .add_field_validation('firstName', Operators::LONGER_THAN, 2)
    .add_field_validation('firstName', Operators::SHORTER_THAN, 13)
    .add_field_validation('firstName', Operators::MATCH, /^[a-z]+$/)
end
```

```python
collection.add_field_validation(
    "firstName", "present"
).add_field_validation(
    "firstName", "longer_than", 2
).add_field_validation(
    "firstName", "shorter_than", 30
).add_field_validation(
    "firstName", "match", r"^[a-z]+$"
)
```

{{#nodejs}}

## Custom validators

If you need to implement custom validators or validation over multiple fields you may use [change hooks](../hooks/README.md).

{{/nodejs}}

{{#nodejs}}

## Make Field Optional

If the introspection marks a field as required, and you would like to make it optional, you can use the `setFieldNullable` function on your collection.

{% hint style="danger" %}
Be wary that if your database system does not allow empty value on the specified field, updating that field on records with an empty value will result in an error.
{% endhint %}

```javascript
collection.setFieldNullable('firstName');
```

{{/nodejs}}
