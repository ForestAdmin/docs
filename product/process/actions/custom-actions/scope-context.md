Actions can have 3 different scopes: `Single`, `Bulk`, and `Global`.

The scope of an action defines how it can be triggered and which records it will target.

| -                                                          | Single                           | Bulk                                                   | Global                                                      |
| ---------------------------------------------------------- | -------------------------------- | ------------------------------------------------------ | ----------------------------------------------------------- |
| **Targeted records**                                       | One at a time                    | All selected and matching the current segment / search | Your choice among all matching the current segment / search |
| **Can be triggered from the List View**                    | When a single record is selected | When one or more records are selected                  | ✅                                                          |
| **Can be triggered from the Details View or Summary View** | ✅                               | ✅                                                     | 🚫                                                          |

# The `context` object

The `context` object is central to writing Actions controllers in Forest Admin.

{{#ruby}}There are two different object context. The first `ForestAdminDatasourceCustomizer::Decorators::Action::Context::ActionContext` is used for the actions of type Bulk or Global.
And the second `ForestAdminDatasourceCustomizer::Decorators::Action::Context::ActionContextSingle` is only used with the actions of type Single
{{/ruby}}

It is the bridge between all the data that your agent has access to and the action's execution. It is passed to the `execute` function as the first argument and provides access to the following properties:

- {{#nodejs,php}}`getRecord(fieldNames)`{{/nodejs,php}}{{#python,ruby}}`get_record(field_names)`{{/python,ruby}} (or {{#nodejs,php}}`getRecords(fieldNames)`{{/nodejs,php}}{{#python,ruby}}`get_records(field_names)`{{/python,ruby}} for `Bulk` and `Global` Actions)
- {{#nodejs,php}}`getRecordId()`{{/nodejs,php}}{{#python,ruby}}`get_record_id()`{{/python,ruby}} (or {{#nodejs,php}}`getRecordIds()`{{/nodejs,php}}{{#python,ruby}}`get_record_ids()`{{/python,ruby}} for `Bulk` and `Global` Actions)
- {{#nodejs,python,ruby}}`collection`{{/nodejs,python,ruby}} the collection on which the action is declared, which can be queried using the [Forest Admin Query Interface](../../datasources/getting-started/queries/README.md#collection-interface).
- {{#nodejs}}`dataSource`{{/nodejs}}{{#ruby,python}}`datasource`{{/ruby,python}} the composite data source who contains all your collections, which can be queried using the [Forest Admin Query Interface](../../datasources/getting-started/queries/README.md#data-source-interface)
- {{#nodejs,python,ruby}}`filter`{{/nodejs,python,ruby}} a filter that can be used to query the collection, and which is based on action scope and the list of selected records.
- {{#nodejs,python,ruby}}`caller`{{/nodejs,python,ruby}} an object containing information about the user who is performing the action (including email, username, timezone, team, role …)
- {{#nodejs,php}}`hasFieldChanged(fieldName)`{{/nodejs,php}}{{#ruby}}`field_changed?(field_name)`{{/ruby}} the name of the field who has changed in the UI. [See an example of usage](./forms-dynamic.md#example-4-using-hasfieldchanged-to-reset-value)

{{#nodejs}}
{% hint style="warning" %}
`changedField` was deprecated in favor of `hasFieldChanged(fieldName)` starting from `@forestadmin/agent@1.28.3` see more information in [this issue report](https://github.com/ForestAdmin/agent-nodejs/issues/815).
{% endhint %}
{{/nodejs}}


## Example 1: Getting data from the selected records

We can simply use the {{#nodejs,php}}`getRecord(fieldNames)`{{/nodejs,php}}{{#python,ruby}}`get_record(field_names)`{{/python,ruby}} method to get any column from the selected record or a relation.

<details>
<summary><strong>agent.customizeCollection('customers', collection =></strong></summary>

```javascript
  collection.addAction('Call me John in the server logs', {
    scope: 'Single',
    execute: async context => {
      // use getRecords() for Bulk and Global Actions
      const { firstName } = await context.getRecord(['firstName']);

      if (firstName === 'John') {
        console.log('Hi John!');
      } else {
        console.error('You are not John!');
      }
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Context</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('user') do |collection|
  collection.add_action(
    'Mark as live',
    BaseAction.new(scope: ActionScope::SINGLE) do |context|
      record = context.get_record(['name'])
        if record['name'] === 'John'
          # log with your favorite logger => 'hi John'
        else
          # log with your favorite logger => 'You are not John!'
        end
    end
  )
end
```

</details>


## Example 2: Updating a field of the selected record

For simple queries, use {{#nodejs,python,ruby}}`context.collection`{{/nodejs,python,ruby}} and {{#nodejs,python,ruby}}`context.filter`{{/nodejs,python,ruby}} to query the collection.

Those are instances of objects from the [Forest Admin Query Interface](../../datasources/getting-started/README.md).

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async context => {
      await context.collection.update(context.filter, { live: true });
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Context</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('user') do |collection|
  collection.add_action(
    'Mark as live',
    BaseAction.new(scope: ActionScope::SINGLE) do |context|
      context.collection.update(context.filter, { name: 'foo' })
    end
  )
end

```

</details>


## Example 3: Coding any business logic

Forest Admin does not impose any restriction on the handler: you are free to write the `execute` handler to fit your use case.

You are free to call external APIs, query your database, or perform any work in action handlers.

<details>
<summary><strong>import axios from 'axios';</strong></summary>

```javascript
agent.customizeCollection('companies', collection =>
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async context => {
      const url = 'http://my-api.com/mark-as-live';
      const params = { id: await context.getRecordId() };

      await axios.post(url, params);
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Context</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Types

@create_agent.customize_collection('user') do |collection|
  collection.add_action(
    'Mark as live',
    BaseAction.new(scope: ActionScope::SINGLE) do |_context|
      res = HTTParty.get('http://my-api.com/mark-as-live')
    end
  )
end
```

</details>


