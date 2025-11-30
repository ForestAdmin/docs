---
title: Validation
---

![A field failing validation](/assets/field-validation-error.png)

Most data sources can import validation rules from their target. For instance, when using SQL data sources:

- Columns of type `VARCHAR(15)` will automatically carry a `less than 15 chars` validator.
- Non-nullable columns will automatically carry a `Present` validator.

However, you may want to enforce stricter restrictions than the ones which are implemented in your data source.

Forest Admin allows adding validation rules to native and custom fields in your admin panel. Those rules will make errors appear in the UI, therefore preventing end-users from making invalid changes.

## Examples

<details>
<summary><strong>Node.js</strong></summary>

```javascript
collection.addFieldValidation('firstName', 'LongerThan', 2);
```

</details>

<details>
<summary><strong>Ruby</strong></summary>

```ruby
@create_agent.customize_collection('user') do |collection|
  collection.add_field_validation('firstName', 'LongerThan', 2)
end
```

</details>

Validation is performed on a best-effort basis and should **not be relied on exclusively** to ensure data integrity.

## Available validators

Common validators include: `Present`, `LongerThan`, `ShorterThan`, `Match` (regex), `Contains`, and more. Refer to the Forest Admin documentation for a complete list of available validators.
