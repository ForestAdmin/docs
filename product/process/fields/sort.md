---
title: Override sorting behavior
---

Depending on the data source, not all fields may be sortable, or you may want to change how the native sorting works.

By using the `replaceFieldSorting` (or `replace_field_sorting` in Ruby) and `emulateFieldSorting` (or `emulate_field_sorting` in Ruby) methods, you can change a single column's sorting behavior.

## Substitution

You can also provide replacement sort clauses. In this example, we're telling Forest Admin "When a user sorts by full name, I want to sort by the last name, and then by the first name".

<details>
<summary><strong>Node.js</strong></summary>

```javascript
collection.replaceFieldSorting('fullName', [
  { field: 'lastName', ascending: true },
  { field: 'firstName', ascending: true },
]);
```

</details>

<details>
<summary><strong>Ruby</strong></summary>

```ruby
@create_agent.customize_collection('user') do |collection|
  collection.replace_field_sorting(
    'fullName',
    [
      { field: 'lastName', ascending: true },
      { field: 'firstName', ascending: true }
    ]
  )
end
```

</details>

## Emulation

The `emulateFieldSorting``emulate_field_sorting` method works by fetching all records from the data source and sorting them in memory.

**You should think twice before using emulation**, as it will have terrible performance on large collections.

<details>
<summary><strong>Node.js</strong></summary>

```javascript
collection.emulateFieldSorting('aField');
```

</details>

<details>
<summary><strong>Ruby</strong></summary>

```ruby
@create_agent.customize_collection('user') do |collection|
  collection.emulate_field_sorting('aField')
end
```

</details>
