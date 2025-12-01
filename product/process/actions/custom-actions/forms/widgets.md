Field Widgets empower your [Actions Forms](./forms-static.md) providing various options to display inputs and ease operators in their daily operations.

# Widgets

{{#nodejs}}

| Widget                                                | Supported types                                      | Minimal version             | Description                                                       |
| ----------------------------------------------------- | ---------------------------------------------------- | --------------------------- | ----------------------------------------------------------------- |
| `null`                                                | All types                                            | `@forestadmin/agent@*`      | Use the default widget                                            |
| [`AddressAutocomplete`](#address-autocomplete-widget) | `String`                                             | `@forestadmin/agent@1.34.0` | Display a text input with address autocomplete.                   |
| [`Checkbox`](#checkbox-widget)                        | `Boolean`                                            | `@forestadmin/agent@1.18.0` | Display a checkbox with true/false and possibly null values.      |
| [`CheckboxGroup`](#checkbox-group-widget)             | `StringList`, `NumberList`                           | `@forestadmin/agent@1.24.0` | Display a group of checkboxes to select multiple values.          |
| [`ColorPicker`](#color-picker-widget)                 | `String`                                             | `@forestadmin/agent@1.27.0` | Display a color picker to select a color.                         |
| [`CurrencyInput`](#currency-input-widget)             | `Number`                                             | `@forestadmin/agent@1.28.0` | Display a currency input.                                         |
| [`DatePicker`](#datepicker-widget)                    | `Date`, `Dateonly`, `String`                         | `@forestadmin/agent@1.29.0` | Display a calendar date picker                                    |
| [`Dropdown`](#dropdown-widget)                        | `Date`, `Dateonly`, `Number`, `String`, `StringList` | `@forestadmin/agent@1.17.0` | Users can choose between a limited set of values.                 |
| [`FilePicker`](#file-picker-widget)                   | `File`, `FileList`                                   | `@forestadmin/agent@1.35.0` | Users can upload files.                                           |
| [`JsonEditor`](#json-editor-widget)                   | `Json`                                               | `@forestadmin/agent@1.31.0` | Display a Json editor with syntax highlighting.                   |
| [`NumberInput`](#number-input-widget)                 | `Number`                                             | `@forestadmin/agent@1.25.0` | Display a standard number input.                                  |
| [`NumberInputList`](#number-input-list-widget)        | `NumberList`                                         | `@forestadmin/agent@1.26.0` | Display a standard number input to enter a list of number values. |
| [`RadioGroup`](#radio-group-widget)                   | `Date`, `Dateonly`, `Number`, `String`               | `@forestadmin/agent@1.23.0` | Group of radio buttons to choose a value from.                    |
| [`RichText`](#rich-text-widget)                       | `String`                                             | `@forestadmin/agent@1.22.0` | Rich text area allowing to input formatted text.                  |
| [`TextArea`](#text-area-widget)                       | `String`                                             | `@forestadmin/agent@1.21.0` | Multi-line text area.                                             |
| [`TextInput`](#text-input-widget)                     | `String`                                             | `@forestadmin/agent@1.19.0` | One-line text input.                                              |
| [`TextInputList`](#text-input-list-widget)            | `StringList`                                         | `@forestadmin/agent@1.20.0` | One-line text input to enter a list of string values              |
| [`TimePicker`](#time-picker-widget)                   | `Time`                                               | `@forestadmin/agent@1.32.0` | Input for entering a time                                         |
| [`UserDropdown`](#user-dropdown-widget)               | `String`, `StringList`                               | `@forestadmin/agent@1.33.0` | Dropdown containing the users available in the current project    |

{{/nodejs}}


{{#ruby}}

| Widget                                                | Supported types                                      | Minimal version | Description                                                       |
| ----------------------------------------------------- | ---------------------------------------------------- | --------------- | ----------------------------------------------------------------- |
| `null`                                                | All types                                            | `1.0.0`         | Use the default widget                                            |
| [`AddressAutocomplete`](#address-autocomplete-widget) | `String`                                             | `1.0.0`         | Display a text input with address autocomplete.                   |
| [`Checkbox`](#checkbox-widget)                        | `Boolean`                                            | `1.0.0`         | Display a checkbox with true/false and possibly null values.      |
| [`CheckboxGroup`](#checkbox-group-widget)             | `StringList`, `NumberList`                           | `1.0.0`         | Display a group of checkboxes to select multiple values.          |
| [`ColorPicker`](#color-picker-widget)                 | `String`                                             | `1.0.0`         | Display a color picker to select a color.                         |
| [`CurrencyInput`](#currency-input-widget)             | `Number`                                             | `1.0.0`         | Display a currency input.                                         |
| [`DatePicker`](#datepicker-widget)                    | `Date`, `Dateonly`, `String`                         | `1.0.0`         | Display a calendar date picker                                    |
| [`Dropdown`](#dropdown-widget)                        | `Date`, `Dateonly`, `Number`, `String`, `StringList` | `1.0.0`         | Users can choose between a limited set of values.                 |
| [`FilePicker`](#file-picker-widget)                   | `File`, `FileList`                                   | `1.0.0`         | Users can upload files.                                           |
| [`JsonEditor`](#json-editor-widget)                   | `Json`                                               | `1.0.0`         | Display a Json editor with syntax highlighting.                   |
| [`NumberInput`](#number-input-widget)                 | `Number`                                             | `1.0.0`         | Display a standard number input.                                  |
| [`NumberInputList`](#number-input-list-widget)        | `NumberList`                                         | `1.0.0`         | Display a standard number input to enter a list of number values. |
| [`RadioGroup`](#radio-group-widget)                   | `Date`, `Dateonly`, `Number`, `String`               | `1.0.0`         | Group of radio buttons to choose a value from.                    |
| [`RichText`](#rich-text-widget)                       | `String`                                             | `1.0.0`         | Rich text area allowing to input formatted text.                  |
| [`TextArea`](#text-area-widget)                       | `String`                                             | `1.0.0`         | Multi-line text area.                                             |
| [`TextInput`](#text-input-widget)                     | `String`                                             | `1.0.0`         | One-line text input.                                              |
| [`TextInputList`](#text-input-list-widget)            | `StringList`                                         | `1.0.0`         | One-line text input to enter a list of string values              |
| [`TimePicker`](#time-picker-widget)                   | `Time`                                               | `1.0.0`         | Input for entering a time                                         |
| [`UserDropdown`](#user-dropdown-widget)               | `String`, `StringList`                               | `1.0.0`         | Dropdown containing the users available in the current project    |

{{/ruby}}

## Address autocomplete widget

The address autocomplete widget allows to input an address as a text value, autocompleted by the Google Maps API.

{% tabs %}
{% tab title="agent.customizeCollection('customer', collection => {" %}
```typescript
collection.addAction('Update address', {
    scope: 'Single',
    form: [
      {
        label: 'Address',
        type: 'String',
        widget: 'AddressAutocomplete',
        placeholder: 'Type the address here',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Update address',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Address',
          type: FieldType::STRING,
          widget: 'AddressAutocomplete',
          placeholder: 'Type the address here',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form (when empty):

![Address autocomplete widget on an action form (empty)](../../assets/action-forms-widget-address-autocomplete-empty.png)

And once the user starts typing:

![Address autocomplete widget on an action form (with suggestions)](../../assets/action-forms-widget-address-autocomplete-open.png)

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `defaultValue` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `placeholder`  | string  | static value only                       | _optional_   | `@forestadmin/agent@1.34.0` | Placeholder shown in the component.                                |

\*
{{/nodejs}}


{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `default_value` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `placeholder`   | string  | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |

{{/ruby}}

## Checkbox widget

The checkbox widget allows to activate or deactivate a boolean value.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Refresh price', {
    scope: 'Single',
    form: [
      {
        label: 'Send a notification',
        type: 'Boolean',
        widget: 'Checkbox',
        isRequired: true,
        defaultValue: false,
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Refresh price',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Send a notification',
          type: FieldType::STRING,
          widget: 'Checkbox',
          is_required: true,
          default_value: false,
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Checkbox widget on an action form](../../assets/action-forms-widget-checkbox.png)

### Options

The `Checkbox` widget has no specific options, but its behavior can be customized using the following already existing [options on fields](./forms-static.md):
{{#nodejs}}

| Option         | Type              | Dynamic support                         | Usage        | Minimal version        | Description                                                                                                                                                                                                                                                                       |
| -------------- | ----------------- | --------------------------------------- | ------------ | ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultValue` | boolean or `null` | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*` | Default value of the field. Define a value different from `null` in combination to `isRequired` to receive a not-null value even if the user did not click the widget.                                                                                                            |
| `description`  | string            | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*` | Description shown above the field, with additional help for users.                                                                                                                                                                                                                |
| `isReadOnly`   | boolean           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*` | Displays a read only field.                                                                                                                                                                                                                                                       |
| `isRequired`   | boolean           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*` | If `isRequired` is set to false (default value), the checkbox widget allows users to choose between 3 states: `true`, `false` and `null`. Setting `isRequired` to `true` only allows 2 states: `true` and `false`. In both cases the default value will be `null` if not defined. |
| `label`        | string            | static value only                       | **required** | `@forestadmin/agent@*` | Field's label.                                                                                                                                                                                                                                                                    |

{{/nodejs}}


{{#ruby}}

| Option          | Type             | Dynamic support                         | Usage        | Minimal version | Description                                                                                                                                                                                                                                                                       |
| --------------- | ---------------- | --------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default_value` | boolean or `nil` | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value of the field. Define a value different from `nil` in combination to `is_required` to receive a not-null value even if the user did not click the widget.                                                                                                            |
| `description`   | string           | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                                                                                                                                                                                                |
| `is_read_only`  | boolean          | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                                                                                                                                                                                                       |
| `is_required`   | boolean          | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | If `is_required` is set to false (default value), the checkbox widget allows users to choose between 3 states: `true`, `false` and `nil`. Setting `is_required` to `true` only allows 2 states: `true` and `false`. In both cases the default value will be `nil` if not defined. |
| `label`         | string           | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                                                                                                                                                                                                    |

{{/ruby}}

{% hint style='info' %}

By default, the `Checkbox` widget allows 3 states:

- {{#nodejs}}`null`{{/nodejs}}{{#ruby}}`nil`{{/ruby}}
- {{#nodejs,ruby}}`true`{{/nodejs,ruby}}
- {{#nodejs,ruby}}`false`{{/nodejs,ruby}}

To ensure having only 2 states, you need to:

1. {{#nodejs}}set the `isRequired` option to `true`,{{/nodejs}}{{#ruby}}set the `is_required` option to `true`,{{/ruby}}
2. define a value for the {{#nodejs}}`defaultValue`{{/nodejs}}{{#ruby}}`default_value`{{/ruby}} option.

{% endhint %}

## Checkbox group widget

The checkbox group widget allows to select multiple values from a list of options.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Leave a review', {
    scope: 'Single',
    form: [
      {
        label: 'Why do you like this product?',
        type: 'StringList',
        widget: 'CheckboxGroup',
        options: [
          { value: 'price', label: 'Good price' },
          { value: 'quality', label: 'Build quality' },
          { value: 'look', label: 'It looks good' },
        ],
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Leave a review',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Why do you like this product?',
          type: FieldType::STRING_LIST,
          widget: 'CheckboxGroup',
          options: [
            { value: 'price', label: 'Good price' },
            { value: 'quality', label: 'Build quality' },
            { value: 'look', label: 'It looks good' },
          ],
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Checkbox group widget on an action form](../../assets/action-forms-widgets-checkbox-group.png)

### Options

{{#nodejs}}

| Option         | Type                                               | Dynamic support                         | Usage        | Minimal version             | Description                                                                                                                                                                                                                                                                                                    |
| -------------- | -------------------------------------------------- | --------------------------------------- | ------------ | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultValue` | string[] or number[]                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.                                                                                                                                                                                                                                                    |
| `description`  | string                                             | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users.                                                                                                                                                                                                                                             |
| `isReadOnly`   | boolean                                            | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                                                                                                                                                                                                                                                                    |
| `isRequired`   | boolean                                            | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.                                                                                                                                                                                                                                                          |
| `label`        | string                                             | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                                                                                                                                                                                                                                                                 |
| `options`      | An array of values, or array of value/label pairs. | [dynamic](./forms-dynamic.md) or static | **required** | `@forestadmin/agent@1.24.0` | List of available options in the component. Supported formats: <ul><li>`['Good', 'Very good']`</li><li>`[{value: 2, label: 'Good'}, {value: 3, label: 'Very good'}]`</li><li>`(context) => ['Good', 'Very good']`</li><li>`(context) => [{value: 2, label: 'Good'}, {value: 3, label: 'Very good'}]`</li></ul> |

{{/nodejs}}

{{#ruby}}

| Option          | Type                                                  | Dynamic support                         | Usage        | Minimal version | Description                                                                                                                                                                                                                                                                                                                                                    |
| --------------- | ----------------------------------------------------- | --------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default_value` | String_List or Number_List                            | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.                                                                                                                                                                                                                                                                                                    |
| `description`   | string                                                | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                                                                                                                                                                                                                                                                             |
| `is_read_only`  | boolean                                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                                                                                                                                                                                                                                                                                    |
| `is_required`   | boolean                                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.                                                                                                                                                                                                                                                                                                          |
| `label`         | string                                                | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                                                                                                                                                                                                                                                                                 |
| `options`       | An array of values, or an array of value/label pairs. | [dynamic](./forms-dynamic.md) or static | **required** | `1.0.0`         | List of available options in the component. Supported formats: <ul><li>`["Good", "Very good"]`</li><li>`[{"value": 2, "label": "Good"}, {"value": 3, "label": "Very good"}]`</li><li>`proc { &#124;context&#124; ["Good", "Very good"] }`</li><li>`proc { &#124;context&#124; [{"value": 2, "label": "Good"}, {"value": 3, "label": "Very good"}] }`</li></ul> |

{{/ruby}}

## Color picker widget

The color picker widget allows to select a color.

{% tabs %}
{% tab title="organization.addAction('Customize UI', {" %}
```typescript
scope: 'Single',
  form: [
    {
      label: 'Background color',
      type: 'String',
      widget: 'ColorPicker',
      placeholder: 'Select the color',
      isRequired: true,
      enableOpacity: false,
      quickPalette: [
        '#6002ee',
        '#90ee02',
        '#021aee',
        '#d602ee',
        '#ee0290',
        '#ee6002',
      ],
    },
  ],
  execute: () => {},
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('organization') do |collection|
  collection.add_action(
    'Customize UI',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Background color',
          type: FieldType::STRING,
          widget: 'ColorPicker',
          placeholder: 'Select the color',
          is_required: true,
          enable_opacity: false,
          quick_palette: [
            '#6002ee',
            '#90ee02',
            '#021aee',
            '#d602ee',
            '#ee0290',
            '#ee6002',
          ],
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Color picker widget on an action form](../../assets/action-forms-widgets-color-picker.png)

### Options

{{#nodejs}}

| Option          | Type     | Dynamic support                         | Usage        | Minimal version             | Description                                                                                            |
| --------------- | -------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------------------------------------------ |
| `defaultValue`  | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.                                            |
| `description`   | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users.                                     |
| `enableOpacity` | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.27.0` | Allow users to select a color with an opacity.                                                         |
| `isReadOnly`    | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                                                            |
| `isRequired`    | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.                                                  |
| `label`         | string   | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                                                         |
| `placeholder`   | string   | static value only                       | _optional_   | `@forestadmin/agent@1.27.0` | Placeholder shown in the component.                                                                    |
| `quickPalette`  | string[] | static value only                       | _optional_   | `@forestadmin/agent@1.27.0` | List of colors to display in the quick palette. The list can be an array of hex colors or rgba colors. |

{{/nodejs}}


{{#ruby}}

| Option           | Type     | Dynamic support                         | Usage        | Minimal version | Description                                                                                            |
| ---------------- | -------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------ |
| `default_value`  | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.                                            |
| `description`    | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                     |
| `enable_opacity` | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to select a color with an opacity.                                                         |
| `is_read_only`   | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                            |
| `is_required`    | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.                                                  |
| `label`          | string   | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                         |
| `placeholder`    | string   | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                                                    |
| `quick_palette`  | string[] | static value only                       | _optional_   | `1.0.0`         | List of colors to display in the quick palette. The list can be an array of hex colors or rgba colors. |

{{/ruby}}

## Currency input widget

The currency input widget allows to input a currency value.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Change price', {
    scope: 'Single',
    form: [
      {
        label: 'Price',
        type: 'Number',
        widget: 'CurrencyInput',
        placeholder: 'Enter the new price',
        isRequired: true,
        min: 0,
        max: 1000,
        step: 1,
        currency: 'USD',
        base: 'Unit',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Change price',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Price',
          type: FieldType::NUMBER,
          widget: 'CurrencyInput',
          placeholder: 'Enter the new price',
          is_required: true,
          min: 0,
          max: 1000,
          step: 1,
          currency: 'USD',
          base: 'Unit',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Currency input widget on an action form](../../assets/action-forms-widget-currency-input.png)

### Options

{{#nodejs}}

| Option         | Type                             | Dynamic support                         | Usage        | Minimal version             | Description                                                                                                                                                                                                  |
| -------------- | -------------------------------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `currency`     | string                           | [dynamic](./forms-dynamic.md) or static | **required** | `@forestadmin/agent@1.28.0` | Currency code to display in the component. Valid currency [ISO codes](https://en.wikipedia.org/wiki/ISO_4217) is required.                                                                                   |
| `defaultValue` | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.                                                                                                                                                  |
| `description`  | string                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users.                                                                                                                                           |
| `base`         | `Unit` (default value) or `Cent` | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.28.0` | Base unit to use for the value. <dl><dt>unit</dt><dd>Value is expressed in the currency's base unit.</dd><dt>cent</dt><dd>Value is expressed as a hundredth of the base unit (typically in cents).</dd></dl> |
| `isReadOnly`   | boolean                          | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                                                                                                                                                                  |
| `isRequired`   | boolean                          | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.                                                                                                                                                        |
| `label`        | string                           | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                                                                                                                                                               |
| `min`          | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.28.0` | Minimum value allowed.                                                                                                                                                                                       |
| `max`          | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.28.0` | Maximum value allowed.                                                                                                                                                                                       |
| `placeholder`  | string                           | static value only                       | _optional_   | `@forestadmin/agent@1.28.0` | Placeholder shown in the component.                                                                                                                                                                          |
| `step`         | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.28.0` | Step between two values.                                                                                                                                                                                     |

{{/nodejs}}

{{#ruby}}

| Option          | Type                             | Dynamic support                         | Usage        | Minimal version | Description                                                                                                                                                                                                  |
| --------------- | -------------------------------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `currency`      | string                           | [dynamic](./forms-dynamic.md) or static | **required** | `1.0.0`         | Currency code to display in the component. Valid currency [ISO codes](https://en.wikipedia.org/wiki/ISO_4217) is required.                                                                                   |
| `default_value` | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.                                                                                                                                                  |
| `description`   | string                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                                                                                                                           |
| `base`          | `Unit` (default value) or `Cent` | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Base unit to use for the value. <dl><dt>unit</dt><dd>Value is expressed in the currency's base unit.</dd><dt>cent</dt><dd>Value is expressed as a hundredth of the base unit (typically in cents).</dd></dl> |
| `is_read_only`  | boolean                          | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                                                                                                                                  |
| `is_required`   | boolean                          | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.                                                                                                                                                        |
| `label`         | string                           | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                                                                                                                               |
| `min`           | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Minimum value allowed.                                                                                                                                                                                       |
| `max`           | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Maximum value allowed.                                                                                                                                                                                       |
| `placeholder`   | string                           | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                                                                                                                                                          |
| `step`          | number                           | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Step between two values.                                                                                                                                                                                     |

{{/ruby}}

## DatePicker widget

The date picker widget allows to input a date in a form

{% tabs %}
{% tab title="agent.customizeCollection('order', collection => {" %}
```typescript
collection.addAction('Set shipping date', {
    scope: 'Single',
    form: [
      {
        label: 'Shipping date',
        type: 'Date',
        widget: 'DatePicker',
        format: 'DD-MM-YYYY',
        min: new Date(Date.now() - 10 * 24 * 60 * 60 * 1000),
        max: new Date(),
        placeholder: 'please indicate when you shipped the item',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('order') do |collection|
  collection.add_action(
    'Set shipping date',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Shipping date',
          type: FieldType::DATE,
          widget: 'DatePicker',
          format: 'DD-MM-YYYY',
          min: DateTime.now - 10,
          max: DateTime.now,
          placeholder: 'please indicate when you shipped the item',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form :

![Date input on an action form](../../assets/action-form-widgets-date-picker.png)

Note: using a `Dateonly` field type will hide the time section from the date picker

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                                                                                                                                           |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultValue` | Date    | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.                                                                                                                           |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users.                                                                                                                    |
| `format`       | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.29.0` | The date picker uses `moment.js` library under the hood. You can refer to [this documentation ](https://momentjs.com/docs/#/displaying/format/) to specify your date display `format` |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                                                                                                                                           |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.                                                                                                                                 |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                                                                                                                                        |
| `min`          | Date    | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.29.0` | The minimum date allowed to be set in the field                                                                                                                                       |
| `max`          | Date    | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.29.0` | The maximum date allowed to be set in the field                                                                                                                                       |
| `placeholder`  | string  | static value only                       | _optional_   | `@forestadmin/agent@1.29.0` | Text will be shown as placeholder if the field is empty                                                                                                                               |

{{/nodejs}}


{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                                                                                                                                          |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `default_value` | Date    | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.                                                                                                                          |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                                                                                                   |
| `format`        | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | The date picker uses `moment.js` library under the hood. You can refer to [this documentation](https://momentjs.com/docs/#/displaying/format/) to specify your date display `format` |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                                                                                                          |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.                                                                                                                                |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                                                                                                       |
| `min`           | Date    | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | The minimum date allowed to be set in the field                                                                                                                                      |
| `max`           | Date    | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | The maximum date allowed to be set in the field                                                                                                                                      |
| `placeholder`   | string  | static value only                       | _optional_   | `1.0.0`         | Text will be shown as placeholder if the field is empty                                                                                                                              |

{{/ruby}}

{% hint style="info" %}
The dates are sent as ISO 8601 formatted strings over the network.
In order to use them as `Date` object in your hooks, you can parse them directly like so

```javascript
const shippingDate = new Date(context.formValues['Shipping date']))
```

{% endhint %}

## Dropdown widget

The dropdown widget allows to select a value from a list of options.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Leave a review', {
    scope: 'Single',
    form: [
      {
        label: 'Rating',
        type: 'Number',
        widget: 'Dropdown',
        options: [
          { value: 0, label: 'Poor' },
          { value: 1, label: 'Fair' },
          { value: 2, label: 'Good' },
          { value: 3, label: 'Very good' },
          { value: 4, label: 'Excellent' },
        ],
        search: 'disabled',
        placeholder: 'Select a rating',
        isRequired: true,
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Leave a review',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Rating',
          type: FieldType::NUMBER,
          widget: 'Dropdown',
          options: [
            { value: 0, label: 'Poor' },
            { value: 1, label: 'Fair' },
            { value: 2, label: 'Good' },
            { value: 3, label: 'Very good' },
            { value: 4, label: 'Excellent' },
          ],
          search: 'disabled',
          placeholder: 'Select a rating',
          is_required: true,
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Dropdown widget on an action form](../../assets/actions-forms-widget-dropdown.png)

In this next example, we call a remote api to get a list of products and then perform a custom filter based on given fields of the json response, and return the first 25.
This example uses {{#nodejs}}`search: 'dynamic'`{{/nodejs}} dropdown widget, which provides you with the {{#nodejs}}`searchValue`{{/nodejs}} in the callback context.

{% hint style='info' %}
Note: when a search is performed, only the field on which it is performed will have its options recomputed.
{% endhint %}

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Add more products', {
    execute: async (context, resultBuilder) => {
      // ...
    },
    scope: 'Bulk',
    form: [
      {
        label: 'Select some products',
        type: 'StringList',
        widget: 'Dropdown',
        search: 'dynamic',
        options: async (context, searchValue) => {
          const products = await (
            await fetch('https://api.fake-eshop.com/v2/products')
          ).json();
          return products
            .filter(product => {
              return (
                !searchValue ||
                ['description', 'name', 'features', 'keyword', 'category'].some(
                  key =>
                    product[key]?.toLowerCase().includes(searchValue.toLowerCase()),
                )
              );
            })
            .slice(0, 25)
            .sort((a, b) => a.name.localeCompare(b.name))
            .map(product => ({ label: product.name, value: product.id }));
        },
      },
    ],
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Add more products',
    BaseAction.new(
      scope: ActionScope::BULK,
      form: [
        {
          label: 'Select some products',
          type: FieldType::STRING_LIST,
          widget: 'Dropdown',
          search: 'dynamic',
          options: ->(context, search_value) {
            products = JSON.parse(
              Net::HTTP.get(URI('https://api.fake-eshop.com/v2/products'))
            )
            products
              .filter do |product|
              !search_value ||
                %w[description name features keyword category].any? do |key|
                  product[key]&.downcase&.include?(search_value.downcase)
                end
            end
              .sort_by { |product| product['name'] }
              .map { |product| { label: product['name'], value: product['id'] } }
              .first(25)
          },
        },
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

### Options

{{#nodejs}}

| Option         | Type                                                 | Dynamic support                         | Usage        | Minimal version                                                                   | Description                                                                                                                                                                                                                                                                                                                                                                                                                 |
| -------------- | ---------------------------------------------------- | --------------------------------------- | ------------ | --------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultValue` | Coherent with `Type`                                 | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                            | Default value when displaying the field for the first time.                                                                                                                                                                                                                                                                                                                                                                 |
| `description`  | string                                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                            | Description shown above the field, with additional help for users.                                                                                                                                                                                                                                                                                                                                                          |
| `isReadOnly`   | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                            | Displays a read only field.                                                                                                                                                                                                                                                                                                                                                                                                 |
| `isRequired`   | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                            | Requires the user to input a value before validating.                                                                                                                                                                                                                                                                                                                                                                       |
| `label`        | string                                               | static value only                       | **required** | `@forestadmin/agent@*`                                                            | Field's label.                                                                                                                                                                                                                                                                                                                                                                                                              |
| `options`      | An array of values, or an array of value/label pairs | [dynamic](./forms-dynamic.md) or static | **required** | `@forestadmin/agent@1.17.0` (`@forestadmin/agent@1.20.1` for dynamic options)     | List of available options in the component. Supported formats: <ul><li>`['Good', 'Very good']`</li><li>`[{value: 2, label: 'Good'}, {value: 3, label: 'Very good'}]`</li></ul>                                                                                                                                                                                                                                              |
| `placeholder`  | string                                               | static value only                       | _optional_   | `@forestadmin/agent@1.17.0`                                                       | Placeholder shown in the component.                                                                                                                                                                                                                                                                                                                                                                                         |
| `search`       | `static`, `dynamic` or `disabled`(default)           | static value only                       | _optional_   | `@forestadmin/agent@1.17.0`, `dynamic` available from `@forestadmin/agent@1.30.0` | Allow users to input text to filter values displayed in the dropdown. <br/> - `disabled`: Users need to scroll to find the appropriate option. <br/> - `static`: Users can search a value by typing terms in an input. The search will be done in <strong>the browser</strong>, based on the label.<br/> - `dynamic` The search is performed on the agent side, using any custom logic needed to fetch the required options |

{{/nodejs}}

{{#ruby}}

| Option          | Type                                                 | Dynamic support                         | Usage        | Minimal version | Description                                                                                                                                                                                                                                                                                                                                                                                                                 |
| --------------- | ---------------------------------------------------- | --------------------------------------- | ------------ | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default_value` | Coherent with `Type`                                 | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.                                                                                                                                                                                                                                                                                                                                                                 |
| `description`   | string                                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                                                                                                                                                                                                                                                                                                                                          |
| `is_read_only`  | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                                                                                                                                                                                                                                                                                                                                                 |
| `is_required`   | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.                                                                                                                                                                                                                                                                                                                                                                       |
| `label`         | string                                               | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                                                                                                                                                                                                                                                                                                                                              |
| `options`       | An array of values, or an array of value/label pairs | [dynamic](./forms-dynamic.md) or static | **required** | `1.0.0`         | List of available options in the component. Supported formats: <ul><li>`["Good", "Very good"]`</li><li>`[{"value": 2, "label": "Good"}, {"value": 3, "label": "Very good"}]`</li></ul>                                                                                                                                                                                                                                      |
| `placeholder`   | string                                               | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                                                                                                                                                                                                                                                                                                                                                                         |
| `search`        | `static`, `dynamic` or `disabled`(default)           | static value only                       | _optional_   | `1.0.0`         | Allow users to input text to filter values displayed in the dropdown. <br/> - `disabled`: Users need to scroll to find the appropriate option. <br/> - `static`: Users can search a value by typing terms in an input. The search will be done in <strong>the browser</strong>, based on the label.<br/> - `dynamic` The search is performed on the agent side, using any custom logic needed to fetch the required options |

{{/ruby}}

## File picker widget

The file picker allows to upload files

{% tabs %}
{% tab title="import { File } from '@forestadmin/datasource-toolkit';" %}
```typescript
import fs from 'fs/promises';
import path from 'path';

import { UserCustomizer } from '../typings';

async function readFile(filePath: string): Promise<File> {
  const data = await fs.readFile(filePath);

  return {
    mimeType: 'image/png',
    buffer: Buffer.from(data),
    name: path.basename(filePath),
  };
}

async function writeFile(file: File, directoryPath: string): Promise<void> {
  await fs.mkdir(directoryPath, { recursive: true });
  await fs.writeFile(
    path.join(directoryPath, path.basename(file.name)),
    file.buffer,
  );
}

export default async (collection: UserCustomizer) => {
  collection.addAction('Update user identification details', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      try {
        const username = await context.getRecord(['username']).username;
        const userDirectory = path.join(
          __dirname,
          'data/documents',
          sanitize(username),
        );
        await Promise.all([
          await writeFile(context.formValues['Avatar picture'], userDirectory),
          ...context.formValues.Identification.map(async (file: File) => {
            await writeFile(file, path.join(userDirectory, 'identification'));
          }),
        ]);
        return resultBuilder.success(`Profile updated`);
      } catch (e) {
        return resultBuilder.error(
          `Upload failed with error: ${(e as Error).message}`,
        );
      }
    },
    form: [
      {
        label: 'Avatar picture',
        type: 'File',
        widget: 'FilePicker',
        description: 'Upload a profile picture or leave it to use the default one',
        extensions: ['png', 'jpg'],
        maxSizeMb: 20,
        defaultValue: readFile(
          path.join(__dirname, './data/avatars/default-avatar.png'),
        ),
      },
      {
        label: 'Identification',
        type: 'FileList',
        widget: 'FilePicker',
        description: 'Upload up to 4 documents to identify the user',
        extensions: ['png', 'jpg', 'bmp', 'pdf', 'gif'],
        maxSizeMb: 2,
        maxCount: 4,
        isRequired: true,
      },
    ],
  });
};
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

def read_file(file_path)
  File.read(file_path)
end

def write_file(directory_path, file)
  File.write(directory_path, file)
end

@create_agent.customize_collection('user') do |collection|
  collection.add_action(
    'Update user identification details',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Avatar picture',
          type: FieldType::FILE,
          widget: 'FilePicker',
          description: 'Upload a profile picture or leave it to use the default one',
          extensions: ['png', 'jpg'],
          max_size_mb: 20,
          default_value: read_file(
            File.dirname(__FILE__) + '/data/avatars/default-avatar.png'
          ),
        },
        {
          label: 'Identification',
          type: FieldType::FILE_LIST,
          widget: 'FilePicker',
          description: 'Upload up to 4 documents to identify the user',
          extensions: ['png', 'jpg', 'bmp', 'pdf', 'gif'],
          max_size_mb: 2,
          max_count: 4,
          is_required: true,
        },
      ]
    ) do |context, result_builder|
      begin
        username = context.get_record(['username'])['username']
        user_directory = File.join(
          __FILE__,
          'data',
          'documents',
          sanitize(username),
        )
        write_file(user_directory, context.form_values['Avatar picture'])
        context.form_values['Identification'].each do |f|
          write_file(user_directory, f)
        end
        result_builder.success('Profile updated')
      rescue StandardError => e
        result_builder.error("Upload failed with error: #{e.message}")
      end
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![File picker widget on an action form](../../assets/action-forms-widget-file-picker.png)

### Options

{{#nodejs}}

| Option         | Type                      | Dynamic support                         | Usage        | Minimal version                                                              | Description                                                                                                                   |
| -------------- | ------------------------- | --------------------------------------- | ------------ | ---------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `defaultValue` | File or File[]            | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                       | Default value when displaying the field for the first time.                                                                   |
| `description`  | string                    | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                       | Description shown above the field, with additional help for users.                                                            |
| `extensions`   | string[]                  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.35.0`                                                  | The whitelist of file extensions allowed. If not specified, any file extension will be accepted                               |
| `isReadOnly`   | boolean                   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                       | Displays a read only field.                                                                                                   |
| `isRequired`   | boolean                   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`                                                       | Requires the user to input a value before validating.                                                                         |
| `label`        | string                    | static value only                       | **required** | `@forestadmin/agent@*`                                                       | Field's label.                                                                                                                |
| `maxSizeMb`    | number                    | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.35.0`                                                  | The max file size allowed to upload. Any file size is allowed if left empty. Make sure your server will be able to handle it. |
| `maxCount`     | number (positive integer) | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.35.0` (only available if the field type is `FileList`) | The max number of files allowed to be uploaded. If not specified, any number of files is allowed                              |

{{/nodejs}}

{{#ruby}}

| Option          | Type                      | Dynamic support                         | Usage        | Minimal version                                          | Description                                                                                                                   |
| --------------- | ------------------------- | --------------------------------------- | ------------ | -------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------- |
| `default_value` | File or File[]            | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`                                                  | Default value when displaying the field for the first time.                                                                   |
| `description`   | string                    | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`                                                  | Description shown above the field, with additional help for users.                                                            |
| `is_read_only`  | boolean                   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`                                                  | Displays a read only field.                                                                                                   |
| `is_required`   | boolean                   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`                                                  | Requires the user to input a value before validating.                                                                         |
| `label`         | string                    | static value only                       | **required** | `1.0.0`                                                  | Field's label.                                                                                                                |
| `extensions`    | string[]                  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`                                                  | The whitelist of file extensions allowed. If not specified, any file extension will be accepted                               |
| `max_size_mb`   | number                    | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`                                                  | The max file size allowed to upload. Any file size is allowed if left empty. Make sure your server will be able to handle it. |
| `max_count`     | number (positive integer) | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0` (only available if the field type is `FileList`) | The max number of files allowed to be uploaded. If not specified, any number of files is allowed                              |

{{/ruby}}

## JSON editor widget

The JSON editor widget display a rich editor with syntax highlighting for JSON.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Set properties', {
    scope: 'Single',
    form: [
      {
        label: 'Properties',
        type: 'Json',
        widget: 'JsonEditor',
        defaultValue: { color: 'red' },
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Set properties',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Properties',
          type: FieldType::JSON,
          widget: 'JsonEditor',
          default_value: { color: 'red' },
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![JSON editor widget on an action form](../../assets/action-forms-widget-json-editor.png)

### Options

This widget does not have any options.

## Number input widget

The number input widget allows to input a number value.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Change price', {
    scope: 'Single',
    form: [
      {
        label: 'Price',
        type: 'Number',
        widget: 'NumberInput',
        placeholder: 'Enter the new price',
        isRequired: true,
        min: 0,
        max: 1000,
        step: 1,
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Change price',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Price',
          type: FieldType::NUMBER,
          widget: 'NumberInput',
          placeholder: 'Enter the new price',
          is_required: true,
          min: 0,
          max: 1000,
          step: 1,
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Number input widget on an action form](../../assets/action-forms-widgets-number-input.png)

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `defaultValue` | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `min`          | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.25.0` | Minimum value allowed.                                             |
| `max`          | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.25.0` | Maximum value allowed.                                             |
| `placeholder`  | string  | static value only                       | _optional_   | `@forestadmin/agent@1.25.0` | Placeholder shown in the component.                                |
| `step`         | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.25.0` | Step between two values.                                           |

{{/nodejs}}

{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `default_value` | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `min`           | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Minimum value allowed.                                             |
| `max`           | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Maximum value allowed.                                             |
| `placeholder`   | string  | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |
| `step`          | number  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Step between two values.                                           |

{{/ruby}}

## Number input list widget

The number input list widget allows to input a list of number values.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Change step values', {
    scope: 'Single',
    form: [
      {
        label: 'Step values',
        type: 'NumberList',
        widget: 'NumberInputList',
        placeholder: 'Enter the new step value',
        isRequired: true,
        min: 0,
        max: 1000,
        step: 1,
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Change step values',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Step values',
          type: FieldType::NUMBER_LIST,
          widget: 'NumberInputList',
          placeholder: 'Enter the new step value',
          is_required: true,
          min: 0,
          max: 1000,
          step: 1,
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form (once the user entered two step values):

![Number input list widget on an action form](../../assets/action-forms-widgets-number-input-list.png)

### Options

{{#nodejs}}

| Option             | Type     | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| ------------------ | -------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `allowDuplicates`  | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.26.0` | Allow users to enter duplicate values.                             |
| `allowEmptyValues` | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.26.0` | Allow users to enter empty values.                                 |
| `defaultValue`     | number[] | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`      | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `enableReorder`    | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.26.0` | Allow users to reorder values.                                     |
| `isReadOnly`       | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`       | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`            | string   | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `min`              | number   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.26.0` | Minimum value allowed.                                             |
| `max`              | number   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.26.0` | Maximum value allowed.                                             |
| `placeholder`      | string   | static value only                       | _optional_   | `@forestadmin/agent@1.26.0` | Placeholder shown in the component.                                |
| `step`             | number   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@1.26.0` | Step between two values.                                           |

{{/nodejs}}

{{#ruby}}

| Option               | Type     | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| -------------------- | -------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `allow_duplicates`   | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to enter duplicate values.                             |
| `allow_empty_values` | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to enter empty values.                                 |
| `default_value`      | number[] | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`        | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `enable_reorder`     | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to reorder values.                                     |
| `is_read_only`       | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`        | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`              | string   | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `min`                | number   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Minimum value allowed.                                             |
| `max`                | number   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Maximum value allowed.                                             |
| `placeholder`        | string   | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |
| `step`               | number   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Step between two values.                                           |

{{/ruby}}

## Radio group widget

The radio group widget allows to select a value from a list of options.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Leave a review', {
    scope: 'Single',
    form: [
      {
        label: 'Appreciation',
        type: 'Number',
        widget: 'RadioGroup',
        options: [
          { value: 1, label: 'Good' },
          { value: 0, label: 'Average' },
          { value: -1, label: 'Bad' },
        ],
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Leave a review',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Appreciation',
          type: FieldType::NUMBER,
          widget: 'RadioGroup',
          options: [
            { value: 1, label: 'Good' },
            { value: 0, label: 'Average' },
            { value: -1, label: 'Bad' },
          ],
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Radio group widget on an action form](../../assets/action-forms-widget-radio-group.png)

### Options

{{#nodejs}}

| Option         | Type                                                 | Dynamic support                         | Usage        | Minimal version             | Description                                                                                                                                                                                                                                                                                                    |
| -------------- | ---------------------------------------------------- | --------------------------------------- | ------------ | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `defaultValue` | Coherent with `Type`                                 | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.                                                                                                                                                                                                                                                    |
| `description`  | string                                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users.                                                                                                                                                                                                                                             |
| `isReadOnly`   | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                                                                                                                                                                                                                                                                    |
| `isRequired`   | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.                                                                                                                                                                                                                                                          |
| `label`        | string                                               | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                                                                                                                                                                                                                                                                 |
| `options`      | An array of values, or an array of value/label pairs | [dynamic](./forms-dynamic.md) or static | **required** | `@forestadmin/agent@1.23.0` | List of available options in the component. Supported formats: <ul><li>`['Good', 'Very good']`</li><li>`[{value: 2, label: 'Good'}, {value: 3, label: 'Very good'}]`</li><li>`(context) => ['Good', 'Very good']`</li><li>`(context) => [{value: 2, label: 'Good'}, {value: 3, label: 'Very good'}]`</li></ul> |

{{/nodejs}}

{{#ruby}}

| Option          | Type                                                 | Dynamic support                         | Usage        | Minimal version | Description                                                                                                                                                                                                                                                                                                                                                    |
| --------------- | ---------------------------------------------------- | --------------------------------------- | ------------ | --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `default_value` | Coherent with `Type`                                 | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.                                                                                                                                                                                                                                                                                                    |
| `description`   | string                                               | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users.                                                                                                                                                                                                                                                                                             |
| `is_read_only`  | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                                                                                                                                                                                                                                                                                                                    |
| `is_required`   | boolean                                              | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.                                                                                                                                                                                                                                                                                                          |
| `label`         | string                                               | static value only                       | **required** | `1.0.0`         | Field's label.                                                                                                                                                                                                                                                                                                                                                 |
| `options`       | An array of values, or an array of value/label pairs | [dynamic](./forms-dynamic.md) or static | **required** | `1.0.0`         | List of available options in the component. Supported formats: <ul><li>`["Good", "Very good"]`</li><li>`[{"value": 2, "label": "Good"}, {"value": 3, "label": "Very good"}]`</li><li>`proc { &#124;context&#124; ["Good", "Very good"] }`</li><li>`proc { &#124;context&#124; [{"value": 2, "label": "Good"}, {"value": 3, "label": "Very good"}] }`</li></ul> |

{{/ruby}}

{% hint style="info" %}
Using the `options` function, you can dynamically change the list of options based on the context, which contains information about the selected records and other values in the form. [More info about dynamic configuration](./forms-dynamic.md).
{% endhint %}

## Rich text widget

The rich text widget allows to input a formatted text value.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Leave a review', {
    scope: 'Single',
    form: [
      {
        label: 'Comment',
        type: 'String',
        widget: 'RichText',
        isRequired: true,
        placeholder: 'Type your comment here',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Leave a review',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Comment',
          type: FieldType::STRING,
          widget: 'RichText',
          is_required: true,
          placeholder: 'Type your comment here',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Rich text widget on an action form](../../assets/action-forms-widget-rich-text.png)

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `defaultValue` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `placeholder`  | string  | static value only                       | _optional_   | `@forestadmin/agent@1.22.0` | Placeholder shown in the component.                                |

{{/nodejs}}

{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `default_value` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `placeholder`   | string  | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |

{{/ruby}}

## Text area widget

The text area widget allows to input a multiline string value.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Leave a review', {
    scope: 'Single',
    form: [
      {
        label: 'Comment',
        type: 'String',
        widget: 'TextArea',
        isRequired: true,
        placeholder: 'Type your comment here...',
        rows: 10,
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Leave a review',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Comment',
          type: FieldType::STRING,
          widget: 'TextArea',
          is_required: true,
          placeholder: 'Type your comment here...',
          rows: 10,
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Text area widget on an action form](../../assets/action-forms-widget-text-area.png)

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `defaultValue` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `placeholder`  | string  |                                         | _optional_   | `@forestadmin/agent@1.21.0` | Placeholder shown in the component.                                |
| `rows`         | number  |                                         | _optional_   | `@forestadmin/agent@1.21.0` | Widget's height expressed as a number of rows.                     |

{{/nodejs}}

{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `default_value` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `placeholder`   | string  |                                         | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |
| `rows`          | number  |                                         | _optional_   | `1.0.0`         | Widget's height expressed as a number of rows.                     |

{{/ruby}}

## Text input widget

The text input widget allows to input a string value.

{% tabs %}
{% tab title="agent.customizeCollection('customer', collection => {" %}
```typescript
collection.addAction('Send notification', {
    scope: 'Single',
    form: [
      {
        label: 'Message',
        type: 'String',
        widget: 'TextInput',
        placeholder: 'Enter your message here',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Send notification',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Message',
          type: FieldType::STRING,
          widget: 'TextInput',
          placeholder: 'Enter your message here',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form:

![Text input widget on an action form](../../assets/action-forms-widget-text-input.png)

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `defaultValue` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `placeholder`  | string  | static value only                       | _optional_   | `@forestadmin/agent@1.19.0` | Placeholder shown in the component.                                |

{{/nodejs}}

{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `default_value` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `placeholder`   | string  | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |

{{/ruby}}

## Text input list widget

The text input list widget allows to input a list of string values.

{% tabs %}
{% tab title="agent.customizeCollection('product', collection => {" %}
```typescript
collection.addAction('Add tags', {
    scope: 'Single',
    form: [
      {
        label: 'Tag',
        type: 'StringList',
        widget: 'TextInputList',
        isRequired: true,
        placeholder: 'Enter a tag',
        allowDuplicates: false,
        allowEmptyValues: false,
        enableReorder: false,
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('product') do |collection|
  collection.add_action(
    'Add tags',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Tag',
          type: FieldType::STRING_LIST,
          widget: 'TextInputList',
          is_required: true,
          placeholder: 'Enter a tag',
          allow_duplicates: false,
          allow_empty_values: false,
          enable_reorder: false,
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form (once the user entered two tags):

![Text input list widget on an action form](../../assets/action-forms-widget-text-input-list.png)

### Options

{{#nodejs}}

| Option             | Type     | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| ------------------ | -------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `allowDuplicates`  | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.20.0` | Allow users to enter duplicate values.                             |
| `allowEmptyValues` | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.20.0` | Allow users to enter empty values.                                 |
| `enableReorder`    | boolean  | static value only                       | _optional_   | `@forestadmin/agent@1.20.0` | Allow users to reorder values.                                     |
| `defaultValue`     | string[] | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`      | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`       | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`       | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`            | string   | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `placeholder`      | string   | static value only                       | _optional_   | `@forestadmin/agent@1.20.0` | Placeholder shown in the component.                                |

{{/nodejs}}

{{#ruby}}

| Option               | Type     | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| -------------------- | -------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `allow_duplicates`   | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to enter duplicate values.                             |
| `allow_empty_values` | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to enter empty values.                                 |
| `enable_reorder`     | boolean  | static value only                       | _optional_   | `1.0.0`         | Allow users to reorder values.                                     |
| `default_value`      | string[] | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`        | string   | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`       | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`        | boolean  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`              | string   | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `placeholder`        | string   | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |

{{/ruby}}

## Time picker widget

The time picker widget allows to select a time

{% tabs %}
{% tab title="agent.customizeCollection('store', collection => {" %}
```typescript
collection.addAction('set opening and closing time', {
    scope: 'Single',
    form: [
      {
        label: 'Opening time',
        type: 'Time',
        widget: 'TimePicker',
      },
      {
        label: 'Closing time',
        type: 'Time',
        widget: 'TimePicker',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('store') do |collection|
  collection.add_action(
    'set opening and closing time',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Opening time',
          type: FieldType::TIME,
          widget: 'TimePicker',
        },
        {
          label: 'Closing time',
          type: FieldType::TIME,
          widget: 'TimePicker',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form :

![Time widget on an action form](../../assets/action-forms-widget-time-picker.png)

{% hint style="info" %}
time picker does not support additional options
{% endhint %}

## User dropdown widget

The user dropdown widget allows to input a user or list of users from the project

{% tabs %}
{% tab title="agent.customizeCollection('Ticket', collection => {" %}
```typescript
collection.addAction('Assign to the record', {
    scope: 'Single',
    form: [
      {
        type: 'String',
        label: 'Manager',
        widget: 'UserDropdown',
        placeholder: 'Select the manager in charge',
      },
      {
        type: 'StringList',
        label: 'Operators',
        widget: 'UserDropdown',
        placeholder: 'Select operators for this record',
      },
    ],
    execute: async context => {
      /* ... perform work here ... */
    },
  });
});
```
{% endtab %}

{% tab title="include ForestAdminDatasourceCustomizer::Decorators::Action" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('Ticket') do |collection|
  collection.add_action(
    'Assign to the record',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          label: 'Manager',
          type: FieldType::STRING,
          widget: 'UserDropdown',
          placeholder: 'Select the manager in charge',
        },
        {
          label: 'Operators',
          type: FieldType::STRING_LIST,
          widget: 'UserDropdown',
          placeholder: 'Select operators for this record',
        }
      ]
    ) do |context, result_builder|
      # perform work here
    end
  )
end
```
{% endtab %}
{% endtabs %}

The above code will produce the following form (once the user has selected some users)

![Dropdown widget on an action form](../../assets/action-forms-widget-user-dropdown.png)

### Options

{{#nodejs}}

| Option         | Type    | Dynamic support                         | Usage        | Minimal version             | Description                                                        |
| -------------- | ------- | --------------------------------------- | ------------ | --------------------------- | ------------------------------------------------------------------ |
| `defaultValue` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Default value when displaying the field for the first time.        |
| `description`  | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Description shown above the field, with additional help for users. |
| `isReadOnly`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Displays a read only field.                                        |
| `isRequired`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `@forestadmin/agent@*`      | Requires the user to input a value before validating.              |
| `label`        | string  | static value only                       | **required** | `@forestadmin/agent@*`      | Field's label.                                                     |
| `placeholder`  | string  | static value only                       | _optional_   | `@forestadmin/agent@1.33.0` | Placeholder shown in the component.                                |

{{/nodejs}}

{{#ruby}}

| Option          | Type    | Dynamic support                         | Usage        | Minimal version | Description                                                        |
| --------------- | ------- | --------------------------------------- | ------------ | --------------- | ------------------------------------------------------------------ |
| `default_value` | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Default value when displaying the field for the first time.        |
| `description`   | string  | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Description shown above the field, with additional help for users. |
| `is_read_only`  | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Displays a read only field.                                        |
| `is_required`   | boolean | [dynamic](./forms-dynamic.md) or static | _optional_   | `1.0.0`         | Requires the user to input a value before validating.              |
| `label`         | string  | static value only                       | **required** | `1.0.0`         | Field's label.                                                     |
| `placeholder`   | string  | static value only                       | _optional_   | `1.0.0`         | Placeholder shown in the component.                                |

{{/ruby}}
