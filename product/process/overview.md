Customization refers to a series of actions that enable you to personalize your agent, data source, or collection.

An agent is an HTTP server that serves the Forest Admin front-end application.
A data source is a database or an API that you want to connect to Forest Admin.
A collection is a set of data that you want to manage in Forest Admin.

Among other things, you can modify your agent by incorporating data sources, using plugins, customizing collections, and adding charts.
Additionally, you can personalize your data sources by offering choices that cater to all data source types.
Lastly, you can adapt your collections by including actions, fields, relations, segments between the data sources and other functionalities.

# Customizing collections

The available customizations are listed in the sections below{{#php,nodejs,ruby}}: [actions](./actions/README.md), [charts](./charts/README.md), [fields](./fields/README.md), [hooks](./hooks/README.md), [pagination](./pagination.md), [plugins](./plugins/README.md), [relationships](./relationships/README.md), [search](./search.md) and [segments](./segments.md){{/php,nodejs,ruby}}.

Using them always starts with the same step: use the {{#nodejs,php}}`customizeCollection`{{/nodejs,php}}{{#python,ruby}}`customize_collection`{{/python,ruby}} method on the collection you want to customize.

{{#nodejs,php,ruby}}It takes two arguments: the collection name and a callback function.{{/nodejs,php,ruby}}


<details>
<summary><strong>const { createAgent } = require('@forestadmin/agent');</strong></summary>

```javascript
createAgent()
  // Add your data source.
  .addDataSource(createSqlDataSource('mariadb://localhost:3808/example'))

  // Customize the 'task' collection from the added data source.
  .customizeCollection('task', taskCollection => {
    // Add a Smart Action
    taskCollection.addAction('send-email', { ... });

    // Add a computed field
    taskCollection.addField('title', { ... });

    // ...
  });
```

</details>

<details>
<summary><strong>module ForestAdminRails</strong></summary>

```ruby
  class CreateAgent
    def self.setup!
      database_configuration = Rails.configuration.database_configuration
      datasource = ForestAdminDatasourceActiveRecord::Datasource.new(database_configuration[Rails.env])

      @create_agent = ForestAdminAgent::Builder::AgentFactory.instance.add_datasource(datasource)
      customize
      @create_agent.build
    end

    def self.customize
      @create_agent.customize_collection('task') do |collection|
        collection.add_field('title', ForestAdminDatasourceCustomizer::Decorators::Computed::ComputedDefinition.new(
            # field definition
        ))
      end
    end
  end
end
```

</details>


{{#nodejs,python}}

# Removing collections

You may want collections to be imported into your Forest Admin agent, but not exposed to the end-users.
To do so, you can use the {{#nodejs}}`removeCollection`{{/nodejs}} method.

All relations to this collection will be removed as well, but the collection will still be available in the Forest Admin agent and can be used in your code

{% hint style="info" %}
Using the [include/exclude options](../datasources/getting-started/partial-imports.md) of {{#nodejs}}`addDataSource`{{/nodejs}} ensures that a collection is not imported in the first place. In that case, it won't be available in your code.
{% endhint %}

```javascript
const { createAgent } = require('@forestadmin/agent');

createAgent()
  // Add your data source.
  .addDataSource(createSqlDataSource('mariadb://localhost:3808/example'))

  // Remove the 'task' collection from your user's admin-panel.
  .removeCollection('task')

  // You can still use the collection in your code
  .customizeCollection('task', taskCollection => {
    // ...
  });
```


{{/nodejs,python}}


{{#nodejs}}

# Auto-completion & Typings & Best practices

You may refer to [this section](../getting-started/install/autocompletion-and-typings.md) to activate auto-completion and typings.

{{/nodejs}}
