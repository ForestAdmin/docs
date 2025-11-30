Forest Admin is a platform to administrate your business operations efficiently; it provides powerful features to ease data navigation and implement high level views.

When designing databases and APIs, the way to go is usually to push for normalization. This means ensuring there is no redundancy of data (all data is stored in only one place), and that data dependencies are logical.

On the other hand, graphical user interfaces usually need duplication and shortcuts to be user-friendly.

To bridge that gap, Forest Admin allows adding, moving, removing, and overriding behaviors from fields.

## Minimal example

<details>
<summary><strong>collection</strong></summary>

```javascript
  // Create a new field
  .addField('fullName', {
    columnType: 'String',
    dependencies: ['firstName', 'lastName'],
    getValues: (records, context) =>
      records.map(r => `${r.firstName} ${r.lastName}`),
  })

  // Make it writable
  .replaceFieldWriting('fullName', (value, context) => {
    const [firstName, lastName] = value.split(' ');

    return { firstName, lastName };
  })

  // Add validators
  .addFieldValidation('fullName', 'Present')
  .addFieldValidation('fullName', 'ShorterThan', 30)
  .addFieldValidation('fullName', 'LongerThan', 2)

  // Make it filterable and sortable
  .emulateFieldFiltering('fullName')
  .emulateFieldSorting('fullName')

  // Remove previous fields
  .removeField('firstName', 'lastName');
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Computed</strong></summary>

```ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('user') do |collection|
  collection.add_field(
    'fullName',
    ComputedDefinition.new(
      column_type: 'String',
      dependencies: ['firstName', 'lastName'],
      values: proc { |records| records.map { |record| "#{record['firstName']} #{record['lastName']}" } }
    )
  )
    # Make it writable
    .replace_field_writing('fullName') do |value, context|
      firstName, lastName = value.split(' ')

      { firstName: firstName, lastName: lastName }
    end
    # Add validators
    .add_field_validation('fullName', Operators::PRESENT)
    .add_field_validation('fullName', Operators::SHORTER_THAN, 30)
    .add_field_validation('fullName', Operators::LONGER_THAN, 2)
    # Make it filterable and sortable
    .emulate_field_filtering('fullName')
    .emulate_field_sorting('fullName')
    # Remove previous fields
    .remove_field('firstName', 'lastName')
end
```

</details>


