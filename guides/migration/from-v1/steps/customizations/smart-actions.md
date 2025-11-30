In legacy agents declaring a Smart Action was a two-step process:

{{#nodejs}}

- First, you had to declare by changing the parameters of the `collection` function in the appropriate `collections/*.js` file.
- Then, you had to implement the action by creating a route handler in the appropriate `routes/*.js` file.
  {{/nodejs}}
  {{#php}}
- First, you had to declare by changing the parameters of the `collection` function in the appropriate `models/*.php` file.
- Then, you had to implement the action by creating a route handler in the appropriate `routes/web.php` file.
  {{/php}}
  {{#python}}
- First, you had to declare by changing the parameters of the `collection` function in the appropriate `app/forest/*.python` file.
- Then, you had to implement the action by creating a route handler in the appropriate `app/urls.py`/`app/views.py` file.
  {{/python}}
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
{{#php}}
| Legacy agent | New agent |
| ------------------------------------------------ | --------------------------------------------------------------------------- |
| type: 'single'<br>type: 'bulk'<br>type: 'global' | scope: 'Single'<br>scope: 'Bulk'<br>scope: 'Global' |
| download: true | generateFile: true |
| reference: 'otherCollection.id' | { type: 'Collection', collectionName: 'otherCollection' } |
| enums: ['foo', 'bar'] | { type: 'Enum', enumValues: ['foo', 'bar'] } |
| Request object | context.getRecordIds() |
| Response object | return resultBuilder.success(...)<br>return resultBuilder.error(...)<br>... |
{{/php}}
{{#python}}
| Legacy agent | New agent |
| ------------------------------------------------ | --------------------------------------------------------------------------- |
| type: 'single'<br>type: 'bulk'<br>type: 'global' | scope: 'Single'<br>scope: 'Bulk'<br>scope: 'Global' |
| download: True | generate_file: True |
| reference: 'otherCollection.id' | { type: 'Collection', collection_name: 'otherCollection' } |
| enums: ['foo', 'bar'] | { type: 'Enum', enum_values: ['foo', 'bar'] } |
| Request object | context.get_record_ids() |
| Response object | return result_builder.success(...)<br>return result_builder.error(...)<br>... |
{{/python}}
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

## Step 1: Calling {{#nodejs,php}}`addAction`{{/nodejs,php}}{{#python,ruby}}`add_action`{{/python,ruby}} for the appropriate collection

Start by calling the {{#nodejs,php}}`addAction`{{/nodejs,php}}{{#python,ruby}}`add_action`{{/python,ruby}} function on the appropriate collection and passing the appropriate parameters.

Most notably, you will need to pass:

- `type` should become `scope`
  - Note that the values are now capitalized (e.g. `single` becomes `Single`)
  - Legacy agents defaulted to `'bulk'` if no type was specified. The new agent requires you to specify the scope.
- `download` should become {{#nodejs,php}}`generateFile`{{/nodejs,php}}{{#python,ruby}}`generate_file`{{/python,ruby}}. This is still a boolean and the same value can be passed.
- `endpoint` and `httpMethod` should be removed. The agent will now automatically handle the routing.

{% tabs %} {% tab title="Before" %}

```javascript
collection('companies', {
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

```php
public function markAsLive(): SmartAction
{
    return $this->smartAction('single', 'Mark as Live');
}
```

```python
from django_forest.utils.collection import Collection

class CompanyForest(Collection):
    def load(self):
        self.actions = [
            {"type": "single", "name": "Mark as Live"},
        ]
```

```ruby
class Forest::Company
  include ForestLiana::Collection

  collection :Company

  action 'Mark as Live', type: 'bulk', download: false, endpoint: '/forest/actions/mark-as-live'
end
```

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('companies', companies => {
  companies.addAction('Mark as Live', {
    scope: 'Bulk',
    execute: async (context, resultBuilder) => {},
  });
});
```

```php
$forestAgent->customizeCollection(
    'Company',
    function (CollectionCustomizer $builder) {
        $builder->addAction(
            'Mark as live',
            new BaseAction(
                scope: ActionScope::SINGLE,
                execute: function(ActionContextSingle $context) {}
            )
        );
   }
);
```

```python
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.action.result_builder import ResultBuilder

def mark_as_live(context: ActionContextSingle, result_builder: ResultBuilder):
    pass

agent.customize_collection("Company").add_action("Mark as live", {
        "scope": "Single",
        "execute": mark_as_live,
    }
)
```

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types
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

{% endtab %} {% endtabs %}

## Step 2: Porting the form definition

Forms are now defined in the `form` property of the action.

You can simply copy the field's definition from the legacy agent to the new agent with the following differences:

- `fields` should become `form`.
- `widget` choice is no longer supported. A default widget will be used depending on the field type.
- `hook` can be removed, those will be handled by the new agent automatically.
- `reference` no longer exists. Use {{#nodejs,php}}`{ type: 'Collection', collectionName: '...' }`{{/nodejs,php}}{{#python,ruby}}`{ type: "Collection", collection_name: '... }`{{/python,ruby}} instead.
- `enums` no longer exist. Use {{#nodejs,php}}`{ type: 'Enum', enumValues: ['...'] }`{{/nodejs,php}}{{#python,ruby}}`{ type: "Enum", enum_values: ['...'] }`{{/python,ruby}} instead.

{% tabs %} {% tab title="Before" %}

```javascript
collection('customers', {
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

```php
public function chargeCreditCard(): SmartAction
{
    return $this->smartAction('single', 'Charge credit card')
        ->addField(
            [
                'field' => 'amount',
                'type' => 'Number',
                'is_required' => true,
                'description' => 'The amount (USD) to charge the credit card. Example: 42.50'
            ]
        )
        ...
}
```

```python
from django_forest.utils.collection import Collection

class CompanyForest(Collection):
    def load(self):
        self.actions = [
            {
                "type": "single",
                "name": "Charge credit card",
                "fields": [
                    {"field": "amount", "type": "number", "isRequired": True},
                ]
            },
        ]
```

```ruby
class Forest::Company
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

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('customers', companies => {
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

```php
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
                ...
            ],
            execute: fn ($context) => ...,
        ));
  }
);
```

```python
agent.customize_collection("Customer").add_action("Charge credit card",{
    "scope": "Single",
    "form": [
        {
            "label": "amount",
            "type": "Number",
            "description": "The amount (USD) to charge the credit card. Example: 42.50",
            "is_required": True
        }
    ],
    "execute": lambda ctx, result_builder: pass
})
```

```ruby
module ForestAdminRails
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

{% endtab %} {% endtabs %}

## Step 3: Porting the route to the new agent `execute` function

{{#nodejs}}
In the legacy agent, users had to implement the action by creating a route handler in the appropriate `routes/*.js` file.
{{/nodejs}}
{{#php}}
In the legacy agent, users had to implement the action by creating a route handler in the appropriate `routes/web.php` file.
{{/php}}
{{#python}}
In the legacy agent, users had to implement the action by creating a route handler in the appropriate `app/urls.py` file.
{{/python}}
{{#ruby}}
In the legacy agent, users had to implement the action by creating a route handler in the appropriate `config/routes.rb` file.
{{/ruby}}

This is no longer needed as the new agent provides a `context` object that contains all the information that is needed to implement the action.

When porting the route handler to the new agent, you will need to:

- Move the body of the route handler to the `execute` function of the action.
- Replace {{#nodejs,php}}`RecordsGetter.getIdsFromRequest()`{{/nodejs,php}}{{#python}}`self.get_ids_from_request()`{{/python}} call with {{#nodejs,php}}`context.getRecordIds()`{{/nodejs,php}}{{#python,ruby}}`context.get_record_ids()`{{/python,ruby}}.
  {{#nodejs}}
- Replace `res.send(...)` calls with `return resultBuilder.success()` or `return resultBuilder.error()`, or the [appropriate `resultBuilder` method](../../../../agent-customization/actions/result-builder.md).
  {{/nodejs}}
  {{#php}}
- Replace `return response();` calls with `return resultBuilder->success()` or `return resultBuilder->error()`, or the [appropriate `resultBuilder` method](../../../../agent-customization/actions/result-builder.md).
  {{/php}}{{#python}}
- Replace `return JsonResponse();` calls with `return result_builder.success()` or `return result_builder.error()`, or the [appropriate `result_builder` method](../../../../agent-customization/actions/result-builder.md).
  {{/python}}{{#ruby}}
- Replace `render json` calls with `return result_builder.success()` or `return result_builder.error()`, or the [appropriate `result_builder` method](../../../../agent-customization/actions/result-builder.md).
  {{/ruby}}
  {% tabs %} {% tab title="Before" %}

```javascript
router.post(
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

```php
public function markAsLive(): JsonResponse
{
    $id = request()->input('data.attributes.ids')[0];
    $company = Company::findOrFail($id);
    $company->status = 'live';
    $company->save();

    return response()->noContent();
}
```

```python
from django.http import HttpResponse
from app.model import Company

class MarksLiveAction(ActionView):
    def post(self, request, *args, **kwargs):
        ids = self.get_ids_from_request(request, self.Model)
        company = Company.objects.filter(id__in=ids)[0]
        company.status = "live"
        company.save()

    return HttpResponse(status=204)
```

```ruby
class Forest::CompaniesController < ForestLiana::SmartActionsController
  def mark_as_live
    company_id = ForestLiana::ResourcesGetter.get_ids_from_request(params, forest_user).first
    Company.update(company_id, status: 'live')

    head :no_content
  end
end
```

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('companies', companies => {
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

```php
$forestAgent->customizeCollection(
    'Company',
    function (CollectionCustomizer $builder) {
        $builder->addAction(
            'Mark as live',
            new BaseAction(
                scope: ActionScope::SINGLE,
                execute: function(ActionContextSingle $context, $resultBuilder) {
                    $companyId = $context->getRecordId();
                    $context->getCollection()->update(['status' => 'live'], new Filter('id', Operator::EQUAL, $companyId));

                    return $resultBuilder->success('Company is now live!');
                }
            )
        );
   }
);
```

```python
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle
from forestadmin.datasource_toolkit.decorators.chart.result_builder import ResultBuilder

async def mark_as_live(context: ActionContextSingle, result_builder: ResultBuilder):
      company_id = await context.get_record_id()
      await context.collection.update(
          { "status": 'live' },
          Filter(
              {"condition_tree": ConditionTreeLeaf("id", "equal", company_id)}
          )
      )

      return result_builder.success('Company is now live!')


agent.customize_collection('companies').addAction('Mark as Live', {
        "scope": "Single",
        "execute": mark_as_live
    }
)
```

```ruby
module ForestAdminRails
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

{% endtab %} {% endtabs %}

## Step 4: Porting Smart Action hooks

Load hooks and change hooks have been replaced on the new agent by the possibility to use callbacks in the form definition.

Here is an example of a load hook where the default value of a field is set to 50 euros converted into dollars:

{% tabs %} {% tab title="Before" %}

```javascript
collection('customers', {
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

```php
public function sendInvoice(): SmartAction
{
    return $this->smartAction('single', 'Charge credit card')
        ->addField(
            [
                'field' => 'country',
                'type' => 'Enum',
                'enums' => [],
            ]
        )
        ->load(
            function () {
                $fields = $this->getFields();
                $fields['country']['enums'] = Company::getEnumsFromDatabaseForThisRecord();

                return $fields;
            }
        )
    ...
```

```python
from django_forest.utils.collection import Collection

class CustomersForest(Collection):
    def load(self):
        self.actions = [
            {
                "type": "single",
                "name": "Charge credit card",
                "fields": [
                    {
                        "field": "amount",
                        "type": "number",
                    },
                ],
                "hooks": {
                    "load": self.send_invoice_load_hook,
                },
            },
        ]

    def send_invoice_load_hook(self, fields, request, *args, **kwargs):
        amount_field = next((x for x in fields if x["field"] == "amount"), None)
        amount_field["value"] = convertEurosIntoDollars(50)
        return fields
```

```ruby
class Forest::Customer
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

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('customers', companies => {
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

```php
$forestAgent->customizeCollection(
    'Company',
    function (CollectionCustomizer $builder) {
        $builder->addAction(
            'Charge credit card',
            new BaseAction(
                scope: ActionScope::SINGLE,
                form: [
                    new DynamicField(
                        label: 'Country',
                        type: FieldType::ENUM,
                        enumValues: fn ($context) => Company::getEnumsFromDatabaseForThisRecord(),
                    ),
                ],
            )
        );
    }
);
```

```python
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle

def amount_default_value(context: ActionContextSingle):
    return convertEurosIntoDollars(50)

agent.customize_collection('customers').add_action('Charge credit card', {
    "scope": "Single",
    "form": [
        {
            "field": 'amount',
            "type": "Number",

            # the function given can also be an async function,
            # it will be automatically awaited
            "default_value": amount_default_value,
            # or a lambda
            # "default_value": lambda context: convertEurosIntoDollars(50),
        },
    ]}
)
```

```ruby
module ForestAdminRails
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

{% endtab %} {% endtabs %}

And another for a change hook which makes a field required if the value of another field is greater than 100:

{% tabs %} {% tab title="Before" %}

```javascript
collection('customers', {
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

```php
return $this->smartAction('single', 'Charge credit card')
    ->addField(
        [
            'field' => 'amount',
            'type' => 'Number',
            'hook' => 'onAmountChange',
        ]
    )
    ->addField(
        [
            'field' => 'motivation',
            'type' => 'String',
            'isRequired' => false,
        ]
    )
    ->change(
        [
            'onAmountChange' => function () {
                $fields = $this->getFields();
                $fields['motivation']['isRequired'] = $fields['amount']['value'] > 100;

                return $fields;
            },
        ]
    );
```

```python
from django_forest.utils.collection import Collection

class CustomersForest(Collection):
    def load(self):
        self.actions = [
            {
                "type": "single",
                "name": "Charge credit card",
                "fields": [
                    {
                        "field": "amount",
                        "type": "number",
                        "hook": "on_amount_change",
                    },
                    {
                        "field": "motivation",
                        "type": "string",
                        "is_required": False,
                    },
                ],
                "hooks": {
                    "change": {
                        "on_amount_change": on_amount_change,
                    },
                },
            },
        ]

    def on_amount_change(self, fields, request, changed_field, *args, **kwargs):
        amount_field = next((x for x in fields if x["field"] == "amount"), None)
        motivation_field = next(
            (x for x in fields if x["field"] == "motivation"), None
        )
        motivationField["is_required"] = amount_field["value"] > 100
        return fields
```

```ruby
class Forest::Customer
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

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('customers', companies => {
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

```php
$forestAgent->customizeCollection(
    'Company',
    function (CollectionCustomizer $builder) {
        $builder->addAction(
            'Send invoice',
            new BaseAction(
                scope: ActionScope::SINGLE,
                form: [
                    new DynamicField(
                        label: 'city',
                        type: FieldType::STRING,
                    ),
                    new DynamicField(
                        label: 'zipCode',
                        type: FieldType::STRING,
                        value: function ($context) {
                          if ($context->getFormValues()['city']) {
                            return Company::getZipCodeFromCity(context->getFormValues()['city']['value']);
                          }
                        }
                    ),
                ],
            )
        );
    }
);

```

```python
from forestadmin.datasource_toolkit.decorators.action.context.single import ActionContextSingle

async def amount_is_required(context: ActionContextSingle):
    return context.form_values.get("amount", 0) > 100

agent.customize_collection('customers').add_action('Charge credit card', {
    "scope": "Single",
    "form": [
        {
            "field": 'amount',
            "type": "Number",
        },
        {
            "field": 'motivation',
            "type": "String",

            # the function given can also be an async function,
            # it will be automatically awaited
            "is_required": amount_is_required,
            # or a lambda
            # "is_required": lambda context:context.form_values.get("amount", 0)>100,
        },
    ]}
)
```

```ruby
module ForestAdminRails
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

{% endtab %} {% endtabs %}
