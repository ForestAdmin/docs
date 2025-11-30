Forest Admin is a platform to administrate your business operations efficiently; it provides powerful features to ease data navigation and implement high level views.

When designing databases and APIs, the way to go is usually to push for normalization. This means ensuring there is no redundancy of data (all data is stored in only one place), and that data dependencies are logical.

On the other hand, graphical user interfaces usually need duplication and shortcuts to be user-friendly.

To bridge that gap, Forest Admin allows adding, moving, removing, and overriding behaviors from fields.

## Minimal example

```javascript
collection
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

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Computed\ComputedDefinition;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->addField(
            'fullName',
            new ComputedDefinition(
                columnType: 'String',
                dependencies: ['firstName', 'lastName'],
                values: fn ($records) => collect($records)->map(fn ($record) => $record['firstName'] . ' ' . $record['lastName']),
            )
        )
            // Make it writable
            ->replaceFieldWriting(
                'fullName',
                function($value) {
                    [$firstName, $lastName] = explode(' ', $value);

                    return compact('firstName', 'lastName');
                }
            )

            // Add validators
            ->addFieldValidation('fullName', Operators::PRESENT)
            ->addFieldValidation('fullName', Operators::SHORTER_THAN, 30)
            ->addFieldValidation('fullName', Operators::GREATER_THAN, 2)

           // Make it filterable and sortable
            ->emulateFieldFiltering('fullName')
            ->emulateFieldSorting('fullName')

            // Remove previous fields
            ->removeField('firstName', 'lastName');
        }
    );

```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Computed
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

```python
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext

agent.customize_collection("User").add_field(
    "fullName",
    {
        "column_type": "String",
        "dependencies": ["firstName", "lastName"],
        "get_values": "get_user_full_name",
    }
    # Make it writable
).replace_field_writing(
    "fullname",
    lambda value, context: {
        "firstName": value.split(" ")[0], "lastName": value.split(" ")[1]
    }
    # add validations
).add_field_validation(
    "fullName", "present"
).add_field_validation(
    "fullName", "shorter_than", 30
).add_field_validation(
    "fullName", "longer_than", 2
    # Make it filterable and sortable
).emulate_field_filtering(
    "fullName"
).emulate_field_sorting(
    "fullName"
    # remove  previous fields
).remove_field(
    "firstName", "lastName"
)

```
