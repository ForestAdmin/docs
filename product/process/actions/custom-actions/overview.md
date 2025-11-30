Sooner or later, you will need to perform actions on your data that are specific to your business.

Moderating comments, generating invoices, logging into a customer’s account, or banning users are exactly the kind of important tasks to unlock to manage your day-to-day operations.

![Custom Action displayed in a Table View](../../assets/actions-dropdown.png)

## In your code

To create an Action, you will first need to declare it in your code for a specific collection. Here we declare a "Mark as Live" Action for the `companies` collection.

{{#nodejs,ruby,python}}The action behavior is implemented in the `execute` function.{{/nodejs,ruby,python}}


<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    description: 'Mark the company as live',
    submitButtonLabel: 'Turn on',
    execute: async context => {
      // Implement your controller here.
    },
    form: [
      {
        type: 'Layout',
        component: 'Page',
        nextButtonLabel: 'Go to address',
        elements: [
          { type: 'Date', label: 'Live date', id: 'LiveDate' },
          { type: 'DateOnly', label: 'Exist since', id: 'CreationDate' },
          { type: 'Layout', component: 'Separator' },
          {
            type: 'Layout',
            component: 'Row',
            fields: [
              { type: 'Boolean', label: 'Credit Card ?', id: 'WithCreditCard' },
              {
                type: 'String',
                if: ctx => ctx.formValues?.WithCreditCard == true,
                label: 'Number',
                id: 'CreditCardNumber',
              },
            ],
          },
        ],
      },
      {
        type: 'Layout',
        component: 'Page',
        previousButtonLabel: 'Go back to general information',
        elements: [
          {
            type: 'Layout',
            component: 'HtmlBlock',
            content:
              async context => `If the company headquarter didn't change continue.<br/>
                    Otherwise set the new address. <br/>
                    <strong>The current address of the company is ${await context.getRecord(
                      ['fullAddress'],
                    ).fullAddress}.
                    `,
          },
          {
            type: 'Layout',
            component: 'Row',
            fields: [
              { type: 'Number', label: 'StreetNumber' },
              { type: 'String', label: 'StreetName' },
            ],
          },
          { type: 'Layout', component: 'Separator' },
          { type: 'String', label: 'PostalCode' },
          { type: 'Number', label: 'City' },
          { type: 'Layout', component: 'Separator' },
          { type: 'String', label: 'Country' },
        ],
      },
    ],
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action

@create_agent.customize_collection('companies') do |collection|
  collection.add_action(
    'Mark as Live',
    BaseAction.new(
      scope: ActionScope::GLOBAL,
      description: "Mark the company as live",
      submit_button_label: "Validate 🫵",
      form: [
        {
            type: FieldType::LAYOUT,
            component: 'Page',
            next_button_label: "Go to address",
            elements:[
                {type: FieldType::DATE, label: "Live date",id: "LiveDate"},
                {type: FieldType::STRING, label: "Exist since", id: "CreationDate"},
                {type: FieldType::LAYOUT, component: "Separator"},
                {
                    type: FieldType::LAYOUT,
                    component: "Row",
                    fields: [
                        {type: FieldType::BOOLEAN, label: "CreditCard ?" id: "WithCreditCard"},
                        {
                            type: FieldType::STRING,
                            if_condition: ->(ctx) {ctx.form_values?.WithCreditCard == true},
                            label: "Number",
                            id: "CreditCardNumber",
                        },
                    ],
                },
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
                        {type: FieldType::NUMBER, label: "StreetName"},
                    ],
                },
                {type: FieldType::LAYOUT, component: "Separator"},
                {type: FieldType::NUMBER, label: "PostalCode"},
                {type: FieldType::NUMBER, label: "City"},
                {type: FieldType::LAYOUT, component: "Separator"},
                {type: FieldType::NUMBER, label: "Country"},
            ],
        },
      ]
    ) do |context|
      # Implement your controller here.
    end
  )
end
```

</details>


| Property                                                                                           | Usage       | Description                                                                                                                                                                                                                |
| -------------------------------------------------------------------------------------------------- | ----------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| scope                                                                                              | **require** | `Single`, `Global` or `Bulk`. See [here](./scope-context.md) for more detail                                                                                                                                               |
| execute                                                                                            | **require** | The callable called when the action is executed, with `context` and `result builder` as parameters. See [context](./scope-context.md#the-context-object) and [result builder](./result-builder.md) pages for more details. |
| form                                                                                               | _optional_  | A list of static fields to be input by the user or a function called with `context` as parameters which returns a list of fields. See [form](./forms-dynamic.md) page for more details.                                    |
| description                                                                                        | _optional_  | An optional description of the action. _Default: null_                                                                                                                                                                     |
| {{#python,ruby}}submit_button_label{{/python,ruby}}{{#nodejs,php}}submitButtonLabel{{/nodejs,php}} | _optional_  | A custom label for the submit button. _Default: the action name_                                                                                                                                                           |

## In the admin panel

After declaring it, the Action will appear in the Smart Actions tab within your Collection Settings.

{% hint style='info' %}

An Action is displayed in the UI only if:

- it is set as "visible" (see screenshot below)
  AND
- in non-development environments, the user's role must grant the "trigger" permission

{% endhint %}

You must make the Action visible there if you wish users to be able to see it in this Team.

![Making the Action visible](../../assets/actions-visibility.png)
