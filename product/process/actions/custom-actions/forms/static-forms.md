Very often, you will need to ask for user inputs before triggering the logic behind an action.

For example, you might want to specify a reason if you want to block a user account. Or set the amount to charge a user’s credit card.

![Action Form displayed on Forest Admin](../../assets/actions-forms-charge-cc.png)

# Field Configuration

{{#nodejs}}

{% hint style='info' %}
More information about the `ActionField` type can be found on our [API Reference](https://forestadmin.github.io/agent-nodejs/types/_forestadmin_datasource_toolkit.ActionField.html).
{% endhint %}

{{/nodejs}}

## Properties

Fields are configurable using the following properties:

{{#nodejs}}

| Property       | Usage                              | Expected value                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          | Description                                                                                                                              |
| -------------- | ---------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| `type`         | **required**                       | `Boolean`, `Date`, `Dateonly`, `Enum`, `Json`, `Number`, `NumberList`, `EnumList`, `String`, `StringList`, `File`, `FileList` and [`Collection`](#references-to-records)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Set the [type of the field](https://forestadmin.github.io/agent-nodejs/interfaces/_forestadmin_datasource_toolkit.ActionFieldBase.html). |
| `label`        | **required**                       | string                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Set the label of the field.                                                                                                              |
| `id`           | _optional_                         | string                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Set the id of the field. The is used internally and to access values from the context. If not set the label will be used.                |
| `description`  | _optional_                         | string                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | Set the description of the field.                                                                                                        |
| `isRequired`   | _optional_                         | boolean                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Make the field required (default false).                                                                                                 |
| `defaultValue` | _optional_                         | any                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     | Set the default value of the field.                                                                                                      |
| `isReadOnly`   | _optional_                         | boolean                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Make the field read-only (default false).                                                                                                |
| `enumValues`   | **required** when `type` is `Enum` | array of strings                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        | Change the list of possible values of the field when type is `Enum`.                                                                     |
| `widget`       | _optional_                         | `null`, [`AddressAutocomplete`](./forms-widgets.md#address-autocomplete-widget), [`Checkbox`](./forms-widgets.md#checkbox-widget), [`CheckboxGroup`](./forms-widgets.md#checkbox-group-widget) , [`ColorPicker`](./forms-widgets.md#color-picker-widget), [`CurrencyInput`](./forms-widgets.md#currency-input-widget),[`DatePicker`](./forms-widgets.md#datepicker-widget), [`Dropdown`](./forms-widgets.md#dropdown-widget), [`FilePicker`](./forms-widgets.md#file-picker-widget), [`JsonEditor`](./forms-widgets.md#json-editor-widget), [`NumberInput`](./forms-widgets.md#number-input-widget), [`NumberInputList`](./forms-widgets.md#number-input-list-widget), [`RadioGroup`](./forms-widgets.md#radio-group-widget), [`RichText`](./forms-widgets.md#rich-text-widget), [`TextArea`](./forms-widgets.md#text-area-widget), [`TextInput`](./forms-widgets.md#text-input-widget), [`TextInputList`](./forms-widgets.md#text-input-list-widget), [`TimePicker`](./forms-widgets.md#time-picker-widget), [`UserDropdown`](./forms-widgets.md#user-dropdown-widget) | Set the widget to use for the field. More info on the [dedicated page](./forms-widgets.md).                                              |

{{/nodejs}}

{{#php,ruby,python}}
| Property | Expected value | Description |
| ------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------- |
| `type` | {{#php,ruby}}`FieldType::{BOOLEAN`, `DATE`, `DATE_ONLY`, `ENUM`, `JSON`, `NUMBER`, `STRING`, `NUMBER_LIST`, `ENUM_LIST`, `STRING_LIST`, `LIST`, `FILE_LIST` and [`COLLECTION`](#references-to-records)`}`{{/php,ruby}}{{#python}}`Boolean`, `Date`, `Dateonly`, `Enum`, `Json`, `Number`, `String`, `NumberList`, `EnumList`, `StringList`, `FileList` and [`Collection`](#references-to-records)`}`{{/python}} | Set the type of the field. |
| `label` | string | Set the label of the field. |
| `id` | string | Set the id of the field. The is used internally and to access values from the context. If not set the label will be used. |
| `description` | string | Set the description of the field. |
| {{#php}}`isRequired`{{/php}}{{#python,ruby}}`is_required`{{/python,ruby}} | boolean | Make the field required (default false). |
| {{#php}}`isReadOnly`{{/php}}{{#python,ruby}}`is_read_only`{{/python,ruby}} | boolean | Make the field read-only (default false). |
| {{#php}}`defaultValue`{{/php}}{{#python,ruby}}`default_value`{{/python,ruby}} | any | Set the default value of the field. |
| {{#php}}`enumValues`{{/php}}{{#python,ruby}}`enum_values`{{/python,ruby}} | array of strings | Change the list of possible values of the field when type is {{#php}}`FieldType::ENUM`{{/php}}{{#python}}`Enum`{{/python}}. |
{{/php,ruby,python}}

```javascript
agent.customizeCollection('customer', collection => {
  collection.addAction('Charge credit card', {
    scope: 'Single',
    form: [
      {
        label: 'amount',
        description: 'The amount (USD) to charge the credit card. Example: 42.50',
        type: 'Number',
        isRequired: true,
      },
    ],
    execute: async (context, resultBuilder) => {
      // Retrieve values entered in the form and columns from the selected record.
      const { amount } = context.formValues;
      const { stripeId, address } = await context.getRecord([
        'stripeId',
        'address:country',
      ]);

      /* ... Charge the credit card here ... */
      return resultBuilder.success('Amount charged!');
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
          type: FieldType::NUMBER,
          label: 'amount',
          description: 'The amount (USD) to charge the credit card. Example: 42.50',
          isRequired: true,
        ),
      ]
      execute: function(ActionContextSingle $context, ResultBuilder $resultBuilder) {
        $record = $context->getRecord(['stripeId', 'address:country']);
        $formValues = $context->getFormValues();

        /* ... Charge the credit card here ... */

        return $resultBuilder->success('Amount charged!');
      }
    ));
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Charge credit card',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          type: FieldType::NUMBER,
          label: 'amount',
          description: 'The amount (USD) to charge the credit card. Example: 42.50',
          is_required: true
        }
      ]
    ) do |context, result_builder|
      record = context.get_record(['stripeId', 'address:country'])
      form_values = context.form_values

      # ... charge the credit card ...

      result_builder.success(message: 'Amount charged!')
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
    record = await context.get_record(['stripeId', 'address:country'])
    form_values = context.form_values

    # ... charge the credit card ...

    return result_builder.success("Amount charged")

agent.customize_collection("Company").add_action("Charge credit card", {
    "scope": "Single",
    "execute": execute,
    "form":[
      {
        "type": "Number",
        "label": "amount",
        "description": "The amount (USD) to charge the credit card. Example: 42.50",
        "is_required": True,
      }
    ],
})
```

## References to records

![Reference widget on an action form](../../assets/actions-form-collection.png)

When using the `Collection` type, you can create a reference to a record of any collection.

The value of the field will be the primary key of the selected record.

{% hint style="info" %}
Note that the value will be stored in an array as the target collection may be using a composite primary key.
When not using a composite primary key, the array can be assumed to contain a single value.
{% endhint %}

```javascript
agent.customizeCollection('ticket', collection => {
  collection.addAction('Assign ticket', {
    scope: 'Single',
    form: [
      {
        label: 'Assignee',
        description: 'The user to assign the ticket to',
        type: 'Collection',
        collectionName: 'user',
        isRequired: true,
      },
    ],
    execute: async (context, resultBuilder) => {
      // Retrieve user id from the form
      // (assuming the user collection has a single primary key)
      const [userId] = context.formValues.Assignee;
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
  'ticket',
  function (CollectionCustomizer $builder) {
    $builder->addAction('Assign ticket', new BaseAction(
      scope: ActionScope::SINGLE,
      form: [
        new DynamicField(
          label: 'Assignee',
          description: 'The user to assign the ticket to',
          type: FieldType::COLLECTION,
          collectionName: 'user',
          isRequired: true,
        ),
      ]
      execute: function(ActionContextSingle $context, ResultBuilder $resultBuilder) {
        // Retrieve user id from the form
        $formValues = $context->getFormValues();
      }
    ));
  }
);
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('ticket') do |collection|
  collection.add_action(
    'Assign ticket',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Assignee',
          description: 'The user to assign the ticket to',
          type: FieldType::COLLECTION,
          collection_name: 'user',
          is_required: true
        }
      ]
    ) do |context, result_builder|
      # Retrieve user id from the form
      user_id = context.form_values['Assignee']
    end
  )
end
```

```python
from typing import Union
from forestadmin.datasource_toolkit.decorators.action.types.actions import ActionSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder
from forestadmin.datasource_toolkit.interfaces.actions import ActionResult


async def execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> Union[None, ActionResult]:
    # Retrieve user id from the form
    user_id = context.form_values["Assigne"]


agent.customize_collection("Company").add_action("Charge credit card", {
    "form":[
        {
            "label": "Assigne",
            "type": "Collection",
            "description": "The user to assign the ticket to",
            "collection_name": "user",
            "is_required": True,
        }
    ],
    "scope": "Single",
    "execute": execute,
})
```
