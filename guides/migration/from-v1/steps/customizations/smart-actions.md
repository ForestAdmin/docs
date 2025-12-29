In legacy agents declaring a Smart Action was a two-step process:

{{#nodejs}}

- First, you had to declare by changing the parameters of the `collection` function in the appropriate `collections/*.js` file.
- Then, you had to implement the action by creating a route handler in the appropriate `routes/*.js` file.
  {{/nodejs}}


  {{#ruby}}
- First, you had to declare by changing the parameters of the `collection` function in the appropriate `/lib/forest_liana/collections/*.rb` file.
- Then, you had to implement the action by creating a route handler in the appropriate `config/routes.rb` file.
  {{/ruby}}
  In the new agent, the process is simplified to a single step.

{% hint style="info" %}
You can find the full documentation of action customization [here](../../../../agent-customization/actions/README.md).
{% endhint %}

# Code cheatsheet

{{#nodejs}}
| Legacy agent | New agent |
| ------------------------------------------------ | --------------------------------------------------------------------------- |
| type: 'single'<br>type: 'bulk'<br>type: 'global' | scope: 'Single'<br>scope: 'Bulk'<br>scope: 'Global' |
| download: true | generateFile: true |
| reference: 'otherCollection.id' | { type: 'Collection', collectionName: 'otherCollection' } |
| enums: ['foo', 'bar'] | { type: 'Enum', enumValues: ['foo', 'bar'] } |
| Request object | context.getRecordIds() |
| res.send(...) | return resultBuilder.success(...)<br>return resultBuilder.error(...)<br>... |
{{/nodejs}}


{{#ruby}}
| Legacy agent | New agent |
| ------------------------------------------------ | --------------------------------------------------------------------------- |
| type: 'single'<br>type: 'bulk'<br>type: 'global' | scope: 'Single'<br>scope: 'Bulk'<br>scope: 'Global' |
| download: true | generate_file: true |
| reference: 'otherCollection.id' | { type: 'Collection', collection_name: 'otherCollection' } |
| enums: ['foo', 'bar'] | { type: 'Enum', enum_values: ['foo', 'bar'] } |
| Request object | context.get_record_ids() |
| Response object | return result_builder.success(...)<br>return result_builder.error(...)<br>... |
{{/ruby}}

# Steps

## Step 1: Calling {{#nodejs}}`addaction`{{/nodejs}}{{#ruby}}`add_action`{{/ruby}} for the appropriate collection

Start by calling the {{#nodejs}}`addAction`{{/nodejs}}{{#ruby}}`add_action`{{/ruby}} function on the appropriate collection and passing the appropriate parameters.

Most notably, you will need to pass:

- `type` should become `scope`
  - Note that the values are now capitalized (e.g. `single` becomes `Single`)
  - Legacy agents defaulted to `'bulk'` if no type was specified. The new agent requires you to specify the scope.
- `download` should become {{#nodejs}}`generateFile`{{/nodejs}}{{#ruby}}`generate_file`{{/ruby}}. This is still a boolean and the same value can be passed.
- `endpoint` and `httpMethod` should be removed. The agent will now automatically handle the routing.

{% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
actions: [
    {
      name: 'Mark as Live',
      type: 'bulk',
      download: false,
      endpoint: '/forest/actions/mark-as-live',
    },
  ],
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestLiana::Collection

  collection :Company

  action 'Mark as Live', type: 'bulk', download: false, endpoint: '/forest/actions/mark-as-live'
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
companies.addAction('Mark as Live', {
    scope: 'Bulk',
    execute: async (context, resultBuilder) => {},
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action

module ForestAdminRails
  class CreateAgent
    def self.customize
      @create_agent.customize_collection('Company') do |collection|
        collection.add_action(
          'Mark as live',
          BaseAction.new(scope: ActionScope::SINGLE) do |context|
            # Implement your controller here.
          end
        )
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

{% endtab %} {% endtabs %}

## Step 2: Porting the form definition

Forms are now defined in the `form` property of the action.

You can simply copy the field's definition from the legacy agent to the new agent with the following differences:

- `fields` should become `form`.
- `widget` choice is no longer supported. A default widget will be used depending on the field type.
- `hook` can be removed, those will be handled by the new agent automatically.
- `reference` no longer exists. Use {{#nodejs}}`{ type: 'Collection', collectionName: '...' }`{{/nodejs}}{{#ruby}}`{ type: "Collection", collection_name: '... }`{{/ruby}} instead.
- `enums` no longer exist. Use {{#nodejs}}`{ type: 'Enum', enumValues: ['...'] }`{{/nodejs}}{{#ruby}}`{ type: "Enum", enum_values: ['...'] }`{{/ruby}} instead.

{% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
actions: [
    {
      name: 'Charge credit card',
      type: 'single',
      fields: [
        {
          field: 'amount',
          isRequired: true,
          description: 'The amount (USD) to charge the credit card. Example: 42.50',
          type: 'Number',
        },
      ],
    },
  ],
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
include ForestLiana::Collection

  collection :Company

  action 'Charge credit card', type: 'single', fields: [
    {
      field: 'amount',
      is_required: true,
      description: 'The amount (USD) to charge the credit card. Example: 42.50',
      type: 'Number',
    }
  ]
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
companies.addAction('Charge credit card', {
    // [...]
    form: [
      {
        field: 'amount',
        isRequired: true,
        description: 'The amount (USD) to charge the credit card. Example: 42.50',
        type: 'Number',
      },
    ],
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class CreateAgent
    include ForestAdminDatasourceCustomizer::Decorators::Action::Types

    def self.customize
      @create_agent.customize_collection('Company') do |collection|
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
              }
            ]
          ) do |context, result_builder|
            #...
          end
        )
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

{% endtab %} {% endtabs %}

## Step 3: Porting the route to the new agent `execute` function

{{#nodejs}}
In the legacy agent, users had to implement the action by creating a route handler in the appropriate `routes/*.js` file.
{{/nodejs}}


{{#ruby}}
In the legacy agent, users had to implement the action by creating a route handler in the appropriate `config/routes.rb` file.
{{/ruby}}

This is no longer needed as the new agent provides a `context` object that contains all the information that is needed to implement the action.

When porting the route handler to the new agent, you will need to:

- Move the body of the route handler to the `execute` function of the action.
- Replace {{#nodejs}}`RecordsGetter.getIdsFromRequest()`{{/nodejs}} call with {{#nodejs}}`context.getRecordIds()`{{/nodejs}}{{#ruby}}`context.get_record_ids()`{{/ruby}}.
  {{#nodejs}}
- Replace `res.send(...)` calls with `return resultBuilder.success()` or `return resultBuilder.error()`, or the [appropriate `resultBuilder` method](../../../../agent-customization/actions/result-builder.md).
  {{/nodejs}}
  {{#ruby}}
- Replace `render json` calls with `return result_builder.success()` or `return result_builder.error()`, or the [appropriate `result_builder` method](../../../../agent-customization/actions/result-builder.md).
  {{/ruby}}
  {% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="router.post(" %}
```javascript
'/actions/mark-as-live',
  permissionMiddlewareCreator.smartAction(),
  (req, res) => {
    const recordsGetter = new RecordsGetter(companies, request.user, request.query);

    return recordsGetter
      .getIdsFromRequest(req)
      .then(companyIds =>
        companies.update({ status: 'live' }, { where: { id: companyIds } }),
      )
      .then(() => res.send({ success: 'Company is now live!' }));
  },
);
```

</details>

<details>
<summary><strong>class Forest::CompaniesController < ForestLiana::SmartActionsController</strong></summary>

```ruby
  def mark_as_live
    company_id = ForestLiana::ResourcesGetter.get_ids_from_request(params, forest_user).first
    Company.update(company_id, status: 'live')

    head :no_content
  end
end
```
{% endtab %}

{% tab title="JavaScript" %}
```javascript
companies.addAction('Mark as Live', {
    // ...
    execute: async (context, resultBuilder) => {
      const companyIds = await context.getRecordIds();
      await companies.update({ status: 'live' }, { where: { id: companyIds } });

      return resultBuilder.success('Company is now live!');
    },
  });
});
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Ruby" %}
```ruby
class CreateAgent
    def self.customize
      include ForestAdminDatasourceCustomizer::Decorators::Action::Types

      @create_agent.customize_collection('Company') do |collection|
        collection.add_action(
          'Mark as live',
          BaseAction.new(scope: ActionScope::SINGLE) do |context, result_builder|
            company_id = context.get_record_id
            Company.update(company_id, status: 'live')

            result_builder.success(message: 'Company is now live!')
          end
        )
      end
    end
  end
end
```

</details>

{% endtab %} {% endtabs %}

## Step 4: Porting smart action hooks

Load hooks and change hooks have been replaced on the new agent by the possibility to use callbacks in the form definition.

Here is an example of a load hook where the default value of a field is set to 50 euros converted into dollars:

{% tabs %} {% tab title="Before" %}

<details>
<summary><strong>collection('customers', {</strong></summary>

```javascript
  actions: [
    {
      name: 'Charge credit card',
      type: 'single',
      fields: [{ field: 'amount', type: 'Number' }],

      // Here is the load hook that sets the default value of the amount field to
      // 50 euros converted into dollars
      hooks: {
        load: async ({ fields, request }) => {
          const amountField = fields.find(field => field.field === 'amount');
          amountField.value = await convertEurosIntoDollars(50);
          return fields;
        },
      },
    },
  ],
});
```
{% endtab %}

{% tab title="class Forest::Customer" %}
```ruby
include ForestLiana::Collection

  collection :Customer

  action 'Charge credit card', type: 'single', fields: [
    {
      field: 'country',
      type: 'Enum',
      enums: [],
    }
  ],
  hooks: {
     :load => -> do
       fields = get_fields
       fields['country']['enums'] = Company.get_enums_from_database_for_this_record

       fields
     end
  }
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
companies.addAction('Charge credit card', {
    scope: 'Single',
    form: [
      {
        field: 'amount',
        type: 'Number',

        // Set the default value of the amount field to 50 euros in dollars.
        // `convertEurosIntoDollars` is a function that returns a promise, it will be
        // awaited automatically
        defaultValues: () => convertEurosIntoDollars(50),
      },
    ],
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class CreateAgent
    def self.customize
      include ForestAdminDatasourceCustomizer::Decorators::Action::Types

      @create_agent.customize_collection('Customer') do |collection|
        collection.add_action(
          'Charge credit card',
          BaseAction.new(
            scope: ActionScope::SINGLE,
            form: [
              {
                label: 'Country',
                type: FieldType::ENUM,
                enum_values: -> { Company.get_enums_from_database_for_this_record }
              }
            ]
          ) do |context, result_builder|
            #...
          end
        )
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

{% endtab %} {% endtabs %}

And another for a change hook which makes a field required if the value of another field is greater than 100:

{% tabs %} {% tab title="Before" %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
actions: [
    {
      name: 'Charge credit card',
      type: 'single',
      fields: [
        { field: 'amount', type: 'Number', hook: 'onAmountChange' },
        { field: 'motivation', type: 'String', isRequired: false },
      ],

      // Change hook requires the motivation field when amount is greater than 100
      hooks: {
        change: {
          onAmountChange: async ({ fields, request }) => {
            const amountField = fields.find(f => f.field === 'amount');
            const motivationField = fields.find(f => f.field === 'motivation');

            motivationField.isRequired = amountField.value > 100;

            return fields;
          },
        },
      },
    },
  ],
});
```
{% endtab %}

{% tab title="class Forest::Customer" %}
```ruby
include ForestLiana::Collection

  collection :Customer

  action 'Charge credit card', type: 'single', fields: [
    {
      field: 'amount',
      type: 'Number',
      hook: 'onAmountChange',
    },
    {
      field: 'motivation',
      type: 'String',
      is_required: false,
    }
  ],
  hooks: {
    :change => {
      'onAmountChanged' => -> (context) {
        amount_field = context[:fields].find{ |field| field[:field] == 'amount'}
        motivation_field = context[:fields].find{ |field| field[:field] == 'motivation'}

        motivation_field[:is_required] = amount_field[:value] > 100

        context[:fields]
      }
    }
  }
end
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="JavaScript" %}
```javascript
companies.addAction('Charge credit card', {
    scope: 'Single',
    form: [
      { field: 'amount', type: 'Number' },
      {
        field: 'motivation',
        type: 'String',
        isRequired: context => context.formValues.amount > 100,
      },
    ],
  });
});
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
class CreateAgent
    def self.customize
      include ForestAdminDatasourceCustomizer::Decorators::Action::Types
      include ForestAdminDatasourceCustomizer::Decorators::Action::Context

      @create_agent.customize_collection('Customer') do |collection|
        collection.add_action(
          'Charge credit card',
          BaseAction.new(
            scope: ActionScope::SINGLE,
            form: [
              {
                label: 'Amount',
                type: FieldType::NUMBER,
              },
              {
                label: 'Motivation',
                type: FieldType::STRING,
                is_required: -> (context) { context.form_values['amount'] > 100 }
              }
            ]
          ) do |context, result_builder|
            #...
          end
        )
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

{% endtab %} {% endtabs %}
