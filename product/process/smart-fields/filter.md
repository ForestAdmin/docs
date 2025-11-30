You may want to read about the following topics before using those features:
{{#nodejs}}

- [Unlocking filtering, scopes, and segments on the UI](../../datasources/custom/translation/capabilities.md#unlock-filtering-scopes-and-segments-on-ui)
- [Structure of a `ConditionTree`](../../datasources/getting-started/queries/filters.md#examples)
- [List of all filtering Operators](../../datasources/getting-started/queries/filters.md#operators)
- [Operator equivalence system](../../datasources/getting-started/queries/filters.md#operator-equivalence)

{{/nodejs}}

{{#php,python}}

- [Structure of a `ConditionTree`](../../datasources/getting-started/queries/filters.md#examples)
- [List of all filtering Operators](../../datasources/getting-started/queries/filters.md#operators)
- [Operator equivalence system](../../datasources/getting-started/queries/filters.md#operator-equivalence)

{{/php,python}}

## Disabling operators

{% hint style="info" %}
Disabling filtering can be made without any code [in the field settings](https://docs.forestadmin.com/user-guide/collections/customize-your-fields#basic-settings).
{% endhint %}

## Substitution

Operation substitution can be used for two motives:

- Performance: provide a more efficient way to perform a given filtering operation
- Capabilities: enable filtering on a computed field or other non-filterable fields

<details>
<summary><strong>collection.replaceFieldOperator('fullName', 'Equal', (value, context) => {</strong></summary>

```javascript
  const [firstName, ...lastNames] = value.split(' ');

  return {
    aggregator: 'And',
    conditions: [
      { field: 'firstName', operator: 'Equal', value: firstName },
      { field: 'lastName', operator: 'Equal', value: lastNames.join(' ') },
    ],
  };
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceToolkit::Components::Query::ConditionTree</strong></summary>

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_operator('fullName', Operators::EQUAL) do |value|
    first_name, *last_names = value.split(' ')

    Nodes::ConditionTreeBranch.new(
      'And',
      [
        Nodes::ConditionTreeLeaf.new('firstName', Operators::EQUAL, first_name),
        Nodes::ConditionTreeLeaf.new('lastName', Operators::EQUAL, last_names.join(' ')),
      ]
    )
  end
end
```

</details>


## Operators to support to enable the search

| Column Type | Operator to support            |
| ----------- | ------------------------------ |
| Number      | Equal                          |
| Enum        | Equal                          |
| String      | IContains OR Contains OR Equal |
| Uuid        | Equal                          |

You can use the [replaceFieldOperator](#substitution) method to unlock the operators.

## Emulation

Filtering emulation allows making fields filterable automatically.
It is a convenient way to get things working quickly for Collections that have a low number of records (in the thousands at most).

{% hint style="warning" %}
This emulation forces the Agent to retrieve all the Collection records and compute the field values for each one of them.
As a consequence, filtering emulation performance cost is **linear** with the number of records in the Collection, so **activate it sparingly and with great care**.
{% endhint %}

<details>
<summary><strong>// Add support for all operators</strong></summary>

```javascript
collection.emulateFieldFiltering('fullName');

// Add support for a single operator
collection.emulateFieldOperator('fullName', 'Equal');
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceToolkit::Components::Query::ConditionTree</strong></summary>

```ruby
@create_agent.customize_collection('customer') do |collection|
  # Add support for all operators
  collection.emulate_field_filtering('fullName')

  # Add support for a single operator
  collection.emulate_field_operator('fullName', Operators::EQUAL)
end
```

</details>


