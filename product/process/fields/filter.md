---
title: Override filtering behavior
---

{% hint style="info" %}

This is an advanced feature that involves understanding condition trees, filtering operators, and operator equivalence. Use this when you need to customize how fields are filtered beyond the default behavior.

{% endhint %}

Forest Admin allows making any field filterable.

However, depending on the data source, not all fields may be filterable, or you may want to change how the native filtering works.

Both can be done by using the `replaceFieldOperator` (or `replace_field_operator` in Ruby) and `emulateFieldOperator` (or `emulate_field_operator` in Ruby) methods.

## Examples

### Substitution

You can provide a handler that converts a `ConditionTree` to one or more others `ConditionTree`.

This example tells Forest Admin "When a user filters by first name, I want to filter by last name instead".

<CodeGroup>

```javascript Node.js
collection.replaceFieldOperator('firstName', 'Contains', value => {
  // Return a condition tree that makes sense for your use-case.
  return {
    field: 'lastName',
    operator: 'Contains',
    value,
  };
});
```

```ruby Ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('user') do |collection|
  collection.replace_field_operator('firstName', Operators::CONTAINS) do |value|
    {
      field: 'lastName',
      operator: Operators::CONTAINS,
      value: value
    }
  end
end
```

</CodeGroup>

### Emulation

You can also use the `emulateFieldOperator``emulate_field_operator` method. This allows you to define filtering behavior for fields that are not filterable by default.

Under the hood, it will work by fetching all records from the datasource and filtering them in memory.

**You should think twice before using emulation**, as it will have terrible performance on large collections.

For more information about operator equivalence, refer to the Forest Admin documentation on filtering.

<CodeGroup>

```javascript Node.js
collection.emulateFieldOperator('aField', 'LongerThan', (recordFieldValue, value) => {
  // recordFieldValue is the value of the field on the record
  // value is the value of the filter (the one provided by the user)
  return typeof recordFieldValue === 'string' && recordFieldValue.length > value;
});
```

```ruby Ruby
@create_agent.customize_collection('user') do |collection|
  collection.emulate_field_operator('aField', 'LongerThan') do |record_field_value, value|
    record_field_value.is_a?(String) && record_field_value.length > value
  end
end
```

</CodeGroup>
