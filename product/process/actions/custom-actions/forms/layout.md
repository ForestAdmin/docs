# Form layout

The form layout feature lets you organize your fields in pages or rows, and add separators or html blocks. This is especially useful if you have many fields to display, and you want to break down your action form into more manageable chunks !


{{#ruby}}Theses form elements are available since the version `1.0.0` of the agent.{{/ruby}}
{{#nodejs}}Theses form elements are available since the version `1.49.0` of the agent.{{/nodejs}}

# Layout items

| Name                              | Nested elements                          | Description                                                                                                                         |
| --------------------------------- | ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------- |
| [Separator](#separator-element)   | None                                     | Allow to add a horizontal separator between two form elements                                                                       |
| [HTML block](#html-block-element) | None                                     | Allow to show HTML content                                                                                                          |
| [Row](#row-element)               | Fields                                   | Allow to put two (and only two) fields (and not layout elements) in a row                                                           |
| [Page](#multi-pages-form)         | Fields & layouts elements (but no pages) | Allow to use multi pages forms. <br/> :warning: When using multi pages form, you **must have only** pages at the root of your form. |

## General properties

Some layouts items will have options, but here are the common properties to all the layout elements

| Name                                                    | Usage        | Expected value                                        | Description                                                                                                                                                   |
| ------------------------------------------------------- | ------------ | ----------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|  type                                                   | **required** |  `Layout`                                             | It differentiate a `field` from `layout` elements. See over values in [static form documentation](forms-static.md#properties)                                 |
| component                                               | **required** | `Separator`, `Row`, `HtmlBlock`, or `Page`            | The layout component                                                                                                                                          |
| if{{#ruby}}\_condition{{/ruby}} | optional     | callable with [context](./scope-context.md) parameter | Only display the field if the function returns true. This one is the exact same one as presented in [dynamic forms](./forms-dynamic.md#form-field-properties) |

# Separator element

| Name      | Usage        | Expected value | Description              |
| --------- | ------------ | -------------- | ------------------------ |
| component | **required** | "Separator"    | To enable this component |

This item doesn't have specific options.

Example:

<details>
<summary><strong>agent.customizeCollection('customer', collection => {</strong></summary>

```javascript
  collection.addAction("What's your name", {
    scope: 'Single',
    form: [
      { type: 'String', label: 'firstName' },
      { type: 'Layout', component: 'Separator' },
      { type: 'String', label: 'lastName' },
    ],
    execute: async (context, resultBuilder) => {
      resultBuilder.success();
    },
  });
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'What\'s your name',
    BaseAction.new(
      scope: ActionScope::SINGLE,
      form: [
        {
          type: FieldType::STRING,
          label: 'firstName',
        }
        {
          type: FieldType::LAYOUT,
          component: 'Separator',
        }
        {
          type: FieldType::STRING,
          label: 'lastName',
        }
      ]
    ) do |context, result_builder|
      result_builder.success()
    end
  )
end

```

</details>


![Example](../../assets/action-form-layout-separator.png)

# HTML Block element

| Name      | Usage        | Expected value                                                                                        | Description                       |
| --------- | ------------ | ----------------------------------------------------------------------------------------------------- | --------------------------------- |
| component | **required** | "HtmlBlock"                                                                                           | To enable this component          |
| content   | **required** | String or callable with [context](./scope-context.md) parameter, returning a string formatted as html |  This is the HTML content to show |

Example:


<details>
<summary><strong>agent.customizeCollection('customer', collection => {</strong></summary>

```javascript
  collection.addAction('Boring form', {
    scope: 'Global',
    form: [
      {
        type: 'String',
        label: 'firstName',
        defaultValue: ctx => ctx.caller.firstName,
      },
      {
        type: 'String',
        label: 'lastName',
        defaultValue: ctx => ctx.caller.lastName,
      },
      {
        type: 'Layout',
        component: 'HtmlBlock',
        content: ctx => `
<div style="text-align:center;">
    <p>
        <strong>Hi ${ctx.formValues.firstName} ${ctx.formValues.lastName}</strong>,
        <br/>here you can put
        <strong style="color: red;">all the html</strong> you want.
    </p>
</div>
<div style="display: flex; flex-flow: row wrap; justify-content: space-around;">
    <a href="https://www.w3schools.com" target="_blank">
        <img src="https://www.w3schools.com/html/w3schools.jpg">
    </a>
    <iframe src="https://www.youtube.com/embed/xHPKuu9-yyw?autoplay=1&mute=1"></iframe>
</div>`,
      },
    ],
    execute: async (context, resultBuilder) => {
      resultBuilder.success();
    },
  });
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Boring form',
    BaseAction.new(
      scope: ActionScope::GLOBAL,
      form: [
        {
            type: FieldType::STRING,
            label: "firstName",
            default_value: ->(context) { context.caller.first_name },
        },
        {
            type: FieldType::STRING,
            label: "lastName",
            default_value: ->(context) { context.caller.last_name },
        },
        {
            type: FieldType::LAYOUT,
            component: 'HtmlBlock',
            content: ->(context) {
'<div style="text-align:center;">
    <p>
        <strong>Hi #{ctx.form_values["firstName"]} #{ctx.form_values["lastName"]}</strong>,
        <br/>here you can put
        <strong style="color: red;">all the html</strong> you want.
    </p>
</div>
<div style="display: flex; flex-flow: row wrap; justify-content: space-around;">
    <a href="https://www.w3schools.com" target="_blank">
        <img src="https://www.w3schools.com/html/w3schools.jpg">
    </a>
    <iframe src="https://www.youtube.com/embed/xHPKuu9-yyw?autoplay=1&mute=1"></iframe>
</div>,
'
            },
        }
        # ...
      ]
    ) do |context, result_builder|
      result_builder.success()
    end
  )
end

```

</details>

![Example](../../assets/action-form-layout-htmlblock.png)

# Row element

:warning: **This element is designed to work with only two inner fields.** You can control the display of each field using the `if{{#ruby}}_condition{{/ruby}}` property. If only one field is displayed, it will occupy the entire line. However, if the conditions defined by the `if{{#ruby}}_condition{{/ruby}}` properties result in more than two fields being displayed, only the first two will be shown. If there is nothing inside, it will be removed.

| Name      | Usage        | Expected value                                               | Description                                                                                                                                 |
| --------- | ------------ | ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------- |
| component | **required** | "Row"                                                        | To enable this component                                                                                                                    |
| fields    | **required** | Array of two [fields](./forms-static.md#field-configuration) | These two fields are the ones you want to display in a line<br/> :warning: No layout elements are allowed in a `Row` component; only fields |

Example:


<details>
<summary><strong>agent.customizeCollection('customer', collection => {</strong></summary>

```javascript
  collection.addAction('Personal form', {
    scope: 'Global',
    form: [
      {
        type: 'Layout',
        component: 'Row',
        fields: [
          {
            label: 'gender',
            type: 'Enum',
            enumValues: ['M', 'F', 'other'],
          },
          {
            label: 'specify',
            description: 'you may specify here'
            type: 'String',
            if: async ctx => ctx.formValues?.gender === 'other',
          },
        ],
      },
    ],
    execute: async (context, resultBuilder) => {
      resultBuilder.success();
    },
  });
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Personal form',
    BaseAction.new(
      scope: ActionScope::GLOBAL,
      form: [
        # ...
        {
            type: FieldType::LAYOUT,
            component: 'Row',
            fields: [
                {
                    label: 'gender',
                    type: FieldType::ENUM,
                    enum_values: ['M', 'F', 'other'],
                },
                {
                    label: 'specify',
                    description: 'you may specify here'
                    type: FieldType::STRING,
                    if_condition: ->(context) { context.form_values['gender'] == 'other' },
                },
            ]
        },
        # ...
      ]
    ) do |context, result_builder|
      result_builder.success()
    end
  )
end

```

</details>

![Example](../../assets/action-form-layout-row.png)

# Multi pages form

## Description

The pages feature is a way to break up your action form into more manageable chunks, by showing only a subset of fields at the same time, and letting the user navigate between the pages.

## Limitations

Please note this list of limitations:

- You cannot mix fields and pages at the root of your form, or put nest a page in a page
- The next (or previous) button is not clickable on the last (or first) page
- If you're using if conditions in a page; keep in mind that **if** all the elements in your page are hidden, the page will automatically be removed. You can avoid this by behavior by adding an unconditional [htmlBlock](#html-block-element) inside your page explaining why is your page empty.

| Name                                                                                           | Usage         | Expected value                      | Description                                                                                                      |
| ---------------------------------------------------------------------------------------------- | ------------- | ----------------------------------- | ---------------------------------------------------------------------------------------------------------------- |
| component                                                                                      | **required**  | "Page"                              | To enable this component                                                                                         |
| elements                                                                                       |  **required** | Array of fields and layout elements | The layout elements defined in this page and the fields defined [in the others documentation](./forms-static.md) |
| {{#nodejs}}nextButtonLabel{{/nodejs}}{{#ruby}}next_button_label{{/ruby}}         |  optional     | String                              | The label on the `next` button                                                                                   |
| {{#nodejs}}previousButtonLabel{{/nodejs}}{{#ruby}}previous_button_label{{/ruby}} |  optional     | String                              | The label on the `previous` button                                                                               |

Example:


<details>
<summary><strong>agent.customizeCollection('customer', collection => {</strong></summary>

```javascript
  collection.addAction('Create user with address', {
    scope: 'Global',
    form: [
      {
        type: 'Layout',
        component: 'Page',
        nextButtonLabel: 'Go to address',
        elements: [
          { type: 'String', id: 'Firstname', label: 'First name' },
          { type: 'String', id: 'Lastname', label: 'Last name' },
          { type: 'Layout', component: 'Separator' },
          { type: 'Date', id: 'Birthdate', label: 'Birth date' },
        ],
      },
      {
        type: 'Layout',
        component: 'Page',
        previousButtonLabel: 'Go back to identity',
        elements: [
          {
            type: 'Layout',
            component: 'Row',
            fields: [
              { type: 'Number', id: 'StreetNumber', label: 'Street number' },
              { type: 'String', id: 'StreetName', label: 'Street name' },
            ],
          },
          { type: 'Layout', component: 'Separator' },
          { type: 'String', id: 'PostalCode', label: 'Postal code' },
          { type: 'String', label: 'City' },
          { type: 'Layout', component: 'Separator' },
          { type: 'String', label: 'Country' },
        ],
      },
    ],
    execute: async (context, resultBuilder) => {
      // your business logic goes here
      return resultBuilder.success();
    },
  });
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('customer') do |collection|
  collection.add_action(
    'Create user with address',
    BaseAction.new(
      scope: ActionScope::GLOBAL,
      form: [
        {
            type: FieldType::LAYOUT,
            component: "Page",
            next_button_label: 'Go to address',
            elements: [
                {type: FieldType::STRING, id: "Firstname", label: "First name"},
                {type: FieldType::STRING, id: "Lastname", label: "Last name"},
                {type: FieldType::LAYOUT, component: "Separator"},
                {type: FieldType::DATE, id: "Birthdate", label: "Birth date"},
            ],
        },
        {
            type: FieldType::LAYOUT,
            component: "Page",
            previous_button_label: "Go back to identity",
            elements: [
                {
                    type: FieldType::LAYOUT,
                    component: "Row",
                    fields: [
                        {type: FieldType::NUMBER, label: "StreetNumber"},
                        {type: FieldType::STRING, label: "StreetName"},
                    ],
                },
                {type: FieldType::LAYOUT, component: "Separator"},
                {type: FieldType::STRING, id: "PostalCode", label: "Postal code"},
                {type: FieldType::STRING, label: "City"},
                {type: FieldType::LAYOUT, component: "Separator"},
                {type: FieldType::STRING, label: "Country"},
            ],
        },
      ]
    ) do |context, result_builder|
      result_builder.success()
    end
  )
end

```

</details>

![page 1](../../assets/action-form-layout-pages-1.png) ![page 2](../../assets/action-form-layout-pages-2.png)
