Business logic often requires your forms to adapt to their context. Forest Admin makes this possible through a powerful way to extend your form's logic.

To make an action form dynamic, simply use functions instead of a static value on the compatible properties.

{{#nodejs}}
But you can also define entirely your form to be dynamic.
{{/nodejs}}

Note that:

- Both synchronous and asynchronous functions are supported
- The functions take the [same context object](./scope-context.md) as the `execute` handler.
  - As such, they have access to the current values of the form.
  - And the records that the action will be applied to.

# Form field properties

`functions` can be used for the following properties which are also available as static values:

{{#nodejs}}
| Property | Description |
| ------------------------------------------------------------------------------- | -------------------------------------- |
| `isRequired` | Make the field required. |
| `isReadOnly` | Make the field read-only. |
| `defaultValue` | Set the default value of the field. |
| `description` | Set the text displayed below the label |
{{/nodejs}}
{{#python,php}}
| Property | Description |
| ------------------------------------------------------------------------------- | -------------------------------------- |
| `{{#python,ruby}}is_required{{/python,ruby}}{{#php}}isRequired{{/php}}` | Make the field required. |
| `{{#python,ruby}}is_read_only{{/python,ruby}}{{#php}}isReadOnly{{/php}}` | Make the field read-only. |
| `{{#python,ruby}}default_value{{/python,ruby}}{{#php}}defaultValue{{/php}}` | Set the default value of the field. |
{{/python,php}}

In addition, depending on the field type, you can also use functions for the following properties:

| Property                                                                                      | Description                                                            |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| `{{#python,ruby}}enum_values{{/python,ruby}}{{#php,nodejs}}enumValues{{/php,nodejs}}`         | Change the list of possible values of the field when `type == 'Enum'`. |
| `{{#python,ruby}}collection_name{{/python,ruby}}{{#php,nodejs}}collectionName{{/php,nodejs}}` | Change the target collection of the field when `type: 'Collection'`.   |

{{#nodejs}}
Some widgets also support dynamic configuration:

| Property  | Widget                                                                                                                                                                 | Description                                           |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------- |
| `options` | [`Dropdown`](./forms-widgets.md#dropdown-widget), [`CheckboxGroup`](./forms-widgets.md#checkbox-group-widget) or [`RadioGroup`](./forms-widgets.md#radio-group-widget) | Change the list of possible values of the field.      |
| `min`     | [`NumberInput`](./forms-widgets.md#number-input-widget) or [`NumberInputList`](./forms-widgets.md#number-input-list-widget)                                            | Define the min value that can be entered by the user. |
| `max`     | [`NumberInput`](./forms-widgets.md#number-input-widget) or [`NumberInputList`](./forms-widgets.md#number-input-list-widget)                                            | Define the max value that can be entered by the user. |
| `step`    | [`NumberInput`](./forms-widgets.md#number-input-widget) or [`NumberInputList`](./forms-widgets.md#number-input-list-widget)                                            | Define the step value of the input.                   |

{{/nodejs}}

And finally, those two extra properties are available and can only be used as functions:

| Property                                                | Description                                          |
| ------------------------------------------------------- | ---------------------------------------------------- |
| `if{{#python}}_{{/python}}{{#ruby}}_condition{{/ruby}}` | Only display the field if the function returns true. |
| `value`                                                 | Set the current value of the field.                  |

# Examples

## Example 1: Dynamic fields based on form values

In this example we make a field required only if the user enters a value greater than 1000 in another field.

```javascript
agent.customizeCollection('customer', collection => {
  collection.addAction('Charge credit card', {
    scope: 'Single',
    form: [
      {
        label: 'amount',
        type: 'Number',
        description: 'The amount (USD) to charge the credit card. Example: 42.50',
        isRequired: true,
      },
      {
        label: 'description',
        type: 'String',
        description: 'Explain why you want to charge the customer manually',

        /**
         * The field will only be required if the function returns true.
         */
        isRequired: context => context.formValues['amount'] > 1000,
      },
    ],
    execute: async (context, resultBuilder) => {
      // ...
    },
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\BaseAction;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Context\ActionContextSingle;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\DynamicField;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\ActionScope;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\FieldType;

$forestAgent->customizeCollection(
  'Customer',
  function (CollectionCustomizer $builder) {
    $builder->addAction('Charge credit card', new BaseAction(
      scope: ActionScope::SINGLE,
      form: [
        new DynamicField(
          label: 'amount',
          type: FieldType::NUMBER,
          description: 'The amount (USD) to charge the credit card. Example: 42.50',
          isRequired: true,
        ),
        new DynamicField(
          label: 'description',
          type: FieldType::STRING,

          /**
           * The field will only be required if the function returns true.
           */
          isRequired: fn ($context) => $context->getFormValues()['amount'] > 1000,
        ),
      ],
      execute: fn ($context) => ...,
    ));
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Charge credit card',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'amount',
          type: FieldType::NUMBER,
          description: 'The amount (USD) to charge the credit card. Example: 42.50',
          is_required: true,
        },
        {
          label: 'description',
          type: FieldType::STRING,
          description: 'Explain why you want to charge the customer manually',

          # The field will only be required if the function returns true.
          is_required: ->(context) { context.get_form_value['amount'].to_i > 1000 },
        },
      ]
    ) do |context|
      # ...
    end
  )
end
```

```python
from typing import Union
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult


async def execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> Union[None, ActionResult]:
    pass


agent.customize_collection("customer").add_action("Charge credit card", {
    "form": [
        {
          "label": 'amount',
          "type": "Number",
        },
        {
          "label": 'description',
          "type": "String",
          # The field will only be required if the function returns true.
          "is_required": lambda context: context.form_values.get('amount', 0) > 1000,

        },
    ],
    "scope": "Single",
    "execute": execute,
})

```

## Example 2: Conditional field display based on record data

Unlike the previous example, this one will only display the field if the record has a specific value.

It is still a dynamic field, but this time, the condition does not depend on the form values but on the record data.

```javascript
agent.customizeCollection('product', collection => {
  collection.addAction('Leave a review', {
    scope: 'Single',
    form: [
      { label: 'Rating', type: 'Enum', enumValues: ['1', '2', '3', '4', '5'] },

      // Only display this field the data does not have comment already
      {
        label: 'Put a comment',
        type: 'String',
        if: async context => {
          const existingComment = await context.getRecord(['comment']).comment;
          return !existingComment;
        },
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\BaseAction;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Context\ActionContextSingle;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\DynamicField;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\ActionScope;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\FieldType;

$forestAgent->customizeCollection(
  'Product',
  function (CollectionCustomizer $builder) {
    $builder->addAction('Leave a review', new BaseAction(
      scope: ActionScope::SINGLE,
      execute: fn ($context) => /* ... perform work here ... */,
      form: [
        new DynamicField(
          label: 'Rating',
          type: FieldType::ENUM,
          enumValues: ['1', '2', '3', '4', '5'],
        ),
        new DynamicField(
          label: 'Put a comment',
          type: FieldType::STRING,
          // Only display this field the data does not have comment already
          if: function ($context) {
            $existingComment = $context->getRecord(['comment']);
            return !isset($existingComment['comment']);
          };
        ),
      ]
    ));
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Leave a review',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Rating',
          type: FieldType::ENUM,
          enum_values: ['1', '2', '3', '4', '5'],
        },
        {
          label: 'Put a comment',
          type: FieldType::STRING,
          # Only display this field the data does not have comment already
          if_condition = proc do |context|
            record = context.get_record(['comment'])
            !record[:comment].nil?
          end,
        },
      ]
    ) do |context|
      # ... perform work here ...
    end
  )
end
```

```python
from typing import Union
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult

async def should_display_comment(context: ActionContextSingle):
    existing_comment = await context.get_record(['comment']).comment
    return existing_comment is None

async def execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> Union[None, ActionResult]:
    # perform work here

agent.customize_collection("Product").add_action("Leave a review", {
    "form": [
        {
            "label": "Rating",
            "type": "Enum",
            "enum_values": ["1", "2", "3", "4", "5"]
        },
        {
            "label": "Put a comment",
            "type": "String",
            # Only display this field the data a does not have comment already
            "if_": should_display_comment,
        },
    ],
})

```

## Example 3: Conditional enum values based on both record data and form values

You can mix and match the previous examples to create complex forms.

In this example, the form will display a different set of enum values depending on both the record data and the value of the form field.

The first field displays different denominations that can be used to address the customer, depending on the full name and gender of the customer.

The second field displays different levels of loudness depending on if the customer is Morgan Freeman, as to ensure that we never speak `Very Loudly` at him, for the sake of politeness.

```javascript
agent.customizeCollection('customer', collection => {
  collection.addAction('Tell me a greeting', {
    scope: 'Single',
    form: [
      {
        label: 'How should we refer to you?',
        type: 'Enum',
        enumValues: async context => {
          // Enum values are computed based on the record data
          // Use an async function to fetch the record data
          const user = await context.getRecord(['firstName', 'lastName', 'gender']);
          const base = [user.firstName, `${user.firstName} ${user.lastName}`];

          if (gender === 'Female') {
            return [...base, `Mrs. ${user.lastName}`, `Miss ${user.lastName}`];
          } else {
            return [...base, `Mr. ${user.lastName}`];
          }
        },
      },
      {
        label: 'How loud should we say it?',
        type: 'Enum',
        enumValues: context => {
          // Enum values are computed based on another form field value
          // (no need to use an async function, but doing so would not be a problem)
          const denomination = context.formValues['How should we refer to you?'];

          return denomination === 'Morgan Freeman'
            ? ['Whispering', 'Softly', 'Loudly']
            : ['Softly', 'Loudly', 'Very Loudly'];
        },
      },
    ],
    execute: async (context, resultBuilder) => {
      const denomination = context.formValues['How should we refer to you?'];
      const loudness = context.formValues['How loud should we say it?'];

      let text = `Hello ${denomination}`;
      if (loudness === 'Whispering') {
        text = text.toLowerCase();
      } else if (loudness === 'Loudly') {
        text = text.toUpperCase();
      } else if (loudness === 'Very Loudly') {
        text = text.toUpperCase() + '!!!';
      }

      return resultBuilder.success(text);
    },
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\BaseAction;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Context\ActionContextSingle;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\DynamicField;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\ActionScope;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\FieldType;

$forestAgent->customizeCollection(
  'Customer',
  function (CollectionCustomizer $builder) {
    $builder->addAction('Tell me a greeting', new BaseAction(
      scope: ActionScope::SINGLE,
      execute: function ($context, $resultBuilder) {
        $denomination = $context->getFormValue('How should we refer to you?');
        $loudness = $context.getFormValue('How loud should we say it?');

        $text = "Hello $denomination";
        if ($loudness === 'Whispering') {
          $text = strtolower($text);
        } elseif ($loudness === 'Loudly') {
          $text = strtoupper($text);
        } elseif ($loudness === 'Very Loudly') {
          $text = strtoupper($text) + '!!!';
        }

        return $resultBuilder->success($text);
      },
      form: [
        new DynamicField(
          label: 'How loud should we say it?',
          type: FieldType::ENUM,
          enumValues: function (ActionContextSingle $context) {
            // Enum values are computed based on another form field value
            // (no need to use an async function here, but doing so would not be a problem)
            $user = $context->getRecord(['firstName', 'lastName', 'gender']);
            $base = [
              $user['firstName'], $user['firstName'] . ' ' . $user['lastName']
            ];

            if ($gender === 'Female') {
              $base[] = 'Mrs. ' . $user['lastName'];
              $base[] = 'Miss. ' . $user['lastName'];
            } else {
              $base[] = 'Mr. ' . $user['lastName'];
            }

            return $base;
          }
        ),
        new DynamicField(
          label: 'How should we refer to you?',
          type: FieldType::ENUM,
          enumValues: function (ActionContextSingle $context) {
            // Enum values are computed based on the record data
            // Because we need to fetch the record data, we need to use an async function
            $denomination = $context->getFormValue('How should we refer to you?');

            return $denomination === 'Morgan Freeman'
              ? ['Whispering', 'Softly', 'Loudly']
              : ['Softly', 'Loudly', 'Very Loudly'];
          }
        ),
      ]
    ));
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Tell me a greeting',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'How should we refer to you?',
          type: FieldType::ENUM,
          enum_values: ->(context) {
            # Enum values are computed based on the record data
            # Because we need to fetch the record data, we need to use an async function
            user = context.get_record(%w[firstName lastName gender])
            base = [user['firstName'], "#{user['firstName']} #{user['lastName']}"]

            if gender == 'Female'
              base << "Mrs. #{user['lastName']}"
              base << "Miss. #{user['lastName']}"
            else
              base << "Mr. #{user['lastName']}"
            end

            base
          }
        },
        {
          label: 'How loud should we say it?',
          type: FieldType::ENUM,
          enum_values: ->(context) {
            # Enum values are computed based on another form field value
            # (no need to use an async function here, but doing so would not be a problem)
            denomination = context.form_values['How should we refer to you?']

            if denomination == 'Morgan Freeman'
              ['Whispering', 'Softly', 'Loudly']
            else
              ['Softly', 'Loudly', 'Very Loudly']
            end
          }
        }
      ]
    ) do |context, result_builder|
        denomination = context.form_values['How should we refer to you?']
        loudness = context.form_values['How loud should we say it?']

        text = "Hello #{denomination}"
        if loudness == 'Whispering'
            text = text.downcase
        elsif loudness == 'Loudly'
            text = text.upcase
        elsif loudness == 'Very Loudly'
            text = "#{text.upcase}!!!"
        end

        result_builder.success(text)
    end
  )
end
```

```python
from typing import Union
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult


def get_loudly_enum_values(context: ActionContextSingle):
    # Enum values are computed based on the record data
    # Because we need to fetch the record data, we need to use an async function
    denomination = context.form_values.get("How should we refer to you?")
    if denomination == "Morgan Freeman":
      return ['Whispering', 'Softly', 'Loudly']
    else:
      return ['Softly', 'Loudly', 'Very Loudly']

def get_denomination_enum_values(context: ActionContextSingle):
    # Enum values are computed based on another form field value
    # (no need to use an async function here, but doing so would not be a problem)
    person = context.get_record(['firstName', 'lastName', 'gender'])
    base = [person['firstName'], f"{person['firstName']} {person['lastName']}"]
    if (person['gender'] == 'Female'):
      base.append(f"Mrs.  {person['lastName']}")
      base.append(f"Miss.  {person['lastName']}")
    else:
      base.append(f"Mr.  {person['lastName']}")
    return base

async def execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> Union[None, ActionResult]:
    denomination = context.form_values["How should we refer to you?"]
    loudness = context.form_values["How loud should we say it?"]

    text = f"Hello {denomination}"
    if loudness == 'Whispering':
      text = text.lower()
    elif loudness == 'Loudly':
      text = text.upper()
    elif loudness == 'Very Loudly:
      text = f"{text.upper()}!!!"

    return result_builder.success(text)


agent.customize_collection("customer").add_action("Tell me a greeting", {
    "form": [
        {
            "label": "How should we refer to you?",
            "type": "Enum",
            "enum_values": get_denomination_enum_values
        }
        {
            "label": "How should we refer to you?",
            "type": "Enum",
            "enum_values": get_loudly_enum_values
        }
    ],
    "scope": "Single",
    "execute": execute,
})
```

## Example 4: Using {{#nodejs,php,ruby}}hasFieldChanged{{/nodejs,php,ruby}}{{#python}}has_field_changed{{/python}} to reset value

In this example we reset a field based on change on another one.

```javascript
agent.customizeCollection('customer', collection => {
  collection.addAction('Create banking identity', {
    scope: 'Single',
    form: [
      {
        label: 'Bank Name',
        type: 'Enum',
        enumValues: ['CE', 'BP'],
        isRequired: true,
      },
      {
        label: 'BIC',
        type: 'String',
        value: context => {
          if (context.hasFieldChanged('Bank Name')) {
            return context.formValues['Bank Name'] === 'CE'
              ? 'CEPAFRPPXXX'
              : 'CCBPFRPPXXX';
          }
        },
      },
    ],
    execute: async (context, resultBuilder) => {
      // ...
    },
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\BaseAction;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Context\ActionContextSingle;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\DynamicField;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\ActionScope;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\FieldType;

$forestAgent->customizeCollection(
  'Customer',
  function (CollectionCustomizer $builder) {
    $builder->addAction(
      'Create banking identity',
      new BaseAction(
        scope: ActionScope::SINGLE,
        execute: function (ActionContextSingle $context, ResultBuilder $resultBuilder) {
          //...
        },
        form: [
          new DynamicField(type: FieldType::ENUM, label: 'Bank Name', isRequired: true, enumValues: ['CE', 'BP']),
          new DynamicField(
            type: FieldType::STRING,
            label: 'BIC',
            value: function (ActionContextSingle $context) {
              if ($context->hasFieldChanged('Bank Name')) {
                return $context->getFormValue('Bank Name') === 'CE'
                  ? 'CEPAFRPPXXX'
                  : 'CCBPFRPPXXX';
              }
            }
          ),
        ],
      )
    );
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Create banking identity',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          type: FieldType::ENUM,
          label: 'Bank Name',
          is_required: true,
          enum_values: %w[CE BP]
        },
        {
          type: FieldType::STRING,
          label: 'BIC',
          if_condition: ->(context) { context.has_field_changed('Bank Name') },
          value: ->(context) { context.form_values['Bank Name'] == 'CE' ? 'CEPAFRPPXXX' : 'CCBPFRPPXXX' }
        }
      ]
    ) do |context, result_builder|
      #...
    end
  )
end
```

```python
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult

def bank_name_value(context: ActionContextSingle):
    if context.has_field_changed('Bank Name'):
        if context.form_value["Bank Name"] == "CE":
            return "CEPAFRPPXXX"
        else:
            return "CCBPFRPPXXX"



agent.customize_collection("customer").add_action("Create banking identity", {
    "form": [
        {
            "label": "Bank Name",
            "type": "Enum",
            "is_required": True,
            "enum_values":  ['CE', 'BP'],
        },
        {
            "label": "BIC",
            "type": "String",
            "value": bank_name_value,
        }
    ],
    "scope": "Single",
    "execute": lambda context, result_builder: pass,
})
```

## Example 5: Setting up default value based on the record

In this example we setup a the default value of a field based on the current record.

```javascript
agent.customizeCollection('order', collection => {
  collection.addAction('Change order price', {
    scope: 'Single',
    form: [
      {
        label: 'New Price',
        type: 'Number',
        defaultValue: async context => {
          return (await context.getRecord(['amount'])).amount;
        },
      },
    ],
    execute: async (context, resultBuilder) => {
      // ...
    },
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\BaseAction;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Context\ActionContextSingle;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\DynamicField;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\ActionScope;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\FieldType;

$forestAgent->customizeCollection(
  'Customer',
  function (CollectionCustomizer $builder) {
    $builder->addAction(
      'Change order price',
      new BaseAction(
        scope: ActionScope::SINGLE,
        execute: function (ActionContextSingle $context, ResultBuilder $resultBuilder) {
          //...
        },
        form: [
          new DynamicField(
            type: FieldType::NUMBER,
            label: 'New Price',
            defaultValue: function (ActionContextSingle $context) {
              return $context->getRecord(['amount'])['amount'];
            }
          ),
        ],
      )
    );
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Change order price',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          type: FieldType::NUMBER,
          label: 'New Price',
          default_value: ->(context) { context.get_record(['amount'])['amount'] }
        }
      ]
    ) do |context, result_builder|
      #...
    end
  )
end
```

```python
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult


async def new_price_default_value(context: ActionContextSingle):
    return await context.get_record(['amount'])['amount']

agent.customize_collection("customer").add_action("Change order price", {
    "form": [
        {
            "label": 'New Price',
            "type": "Number",
            "default_value": new_price_default_value,
        }
    ],
    "scope": "Single",
    "execute": lambda context, result_builder: #...
})

```

## Example 6: Block field edition

In this example we make a field read only based on a field from the current record.

```javascript
agent.customizeCollection('order', collection => {
  collection.addAction('Change order price', {
    scope: 'Single',
    form: [
      {
        label: 'New Price',
        type: 'Number',
        defaultValue: async context => {
          return (await context.getRecord(['amount'])).amount;
        },
        isReadOnly: async context => {
          return (await context.getRecord(['status'])).status === 'paid';
        },
      },
    ],
    execute: async (context, resultBuilder) => {
      // ...
    },
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\BaseAction;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Context\ActionContextSingle;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\DynamicField;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\ResultBuilder;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\ActionScope;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Actions\Types\FieldType;

$forestAgent->customizeCollection(
  'Customer',
  function (CollectionCustomizer $builder) {
    $builder->addAction(
      'Change order price',
      new BaseAction(
        scope: ActionScope::SINGLE,
        execute: function (ActionContextSingle $context, ResultBuilder $resultBuilder) {
          //...
        },
        form: [
          new DynamicField(
            type: FieldType::NUMBER,
            label: 'New Price',
            defaultValue: function (ActionContextSingle $context) {
              return $context->getRecord(['amount'])['amount'];
            },
            isReadOnly: function (ActionContextSingle $context) {
              return $context->getRecord(['status'])['status'] === 'paid';
            },
          ),
        ],
      )
    );
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Change order price',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          type: FieldType::NUMBER,
          label: 'New Price',
          default_value: ->(context) { context.get_record(['amount'])['amount'] },
          is_read_only: ->(context) { context.get_record(['status'])['status'] == 'paid' }
        }
      ]
    ) do |context, result_builder|
      #...
    end
  )
end
```

```python
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult


async def new_price_default_value(context: ActionContextSingle):
    return await context.get_record(['amount'])['amount']

async def new_price_is_read_only(context: ActionContextSingle):
    return await context.get_record(['status'])['status'] == 'paid'

agent.customize_collection("customer").add_action("Change order price", {
    "form":  [
        {
            "label": 'New Price',
            "type": "Number",
            "default_value": new_price_default_value,
            "is_read_only": new_price_is_read_only
        }
    ],
    "scope": "Single",
    "execute": lambda context, result_builder: #...
})
```

{{#nodejs}}

## Exemple 7: Define a totally dynamic form

In this example we define the fields to be all the fields of the current collection.
Doing that will allow the form to be always up to date and to be able to enforce the edition of a record through this action for example.

```javascript
agent.customizeCollection('order', collection => {
  collection.addAction('Update order', {
    scope: 'Single',
    form: async formContext => {
      return Object.entries(formContext.collection.schema.fields)
        .map(([fieldName, field]) =>
          field.type === 'Column'
            ? {
                label: fieldName,
                type: field.columnType,
              }
            : undefined,
        )
        .filter(field => field !== undefined);
    },
    execute: async (context, resultBuilder) => {
      // ...
    },
  });
});
```

{{/nodejs}}
