{{#nodejs}}
Each Plugin is nothing more than an `async function` that can perform customizations at either Agent level, Collection level, or both.
{{/nodejs}}


{{#ruby}}
Each Plugin is nothing more than a `class` that implements our `Plugin abstract class` and can perform customizations at either Agent level, Collection level, or both.
{{/ruby}}


<details>
<summary><strong>export async function removeTimestamps(dataSource, collection, options) {</strong></summary>

```javascript
  // ... call customization methods here
}
```

</details>

<details>
<summary><strong>class RemoveTimestamps < ForestAdminDatasourceCustomizer::Plugins::Plugin</strong></summary>

```ruby
  def run(datasource, collection, options)
    # ... call customization methods here
  end
end
```

</details>


3 parameters are provided:

{{#nodejs}}
| Name | Description |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataSource<br>([API Reference](https://forestadmin.github.io/agent-nodejs/classes/_forestadmin_datasource_customizer.DataSourceCustomizer.html)) | An object that allows customizing the whole agent. It has the same interface than the `Agent` you manipulate outside of Plugins |
| collection<br>([API Reference](https://forestadmin.github.io/agent-nodejs/classes/_forestadmin_datasource_customizer.CollectionCustomizer.html)) | An object that allows customizing the collection that the Plugin was called from (`null` if the Plugin was called on the Agent). It is the same object than is passed when you call `customizeCollection` |
| options | Options that are provided to the Plugin. There is no set structure for this parameter, as each Plugin will provide specific mandatory or optional options. |
{{/nodejs}}

{{#ruby}}
| Name | Description |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataSource | An object that allows customizing the whole agent. It has the same interface than the `Agent` you manipulate outside of Plugins |
| collection | An object that allows customizing the collection that the Plugin was called from (`nil` if the Plugin was called on the Agent). It is the same object than is passed when you call `customize_collection` |
| options | An array of options that are provided to the Plugin. Each Plugin will provide specific mandatory or optional options.
{{/ruby}}


## Making your Plugin act differently depending on the Collection

When making a Plugin, you may want it to generalize to many different Collections.

This can be achieved by adopting different behavior depending on the `schema` of the Collection being targeted.
{{#nodejs}}
Relevant documentation:

- [DataSourceSchema](https://forestadmin.github.io/agent-nodejs/types/_forestadmin_datasource_toolkit.DataSourceSchema.html) (for Plugins working on Charts)
- [CollectionSchema](https://forestadmin.github.io/agent-nodejs/types/_forestadmin_datasource_toolkit.CollectionSchema.html) (for all other Plugins)

{{/nodejs}}

<details>
<summary><strong>export async function removeTimestamps(dataSource, collection, options) {</strong></summary>

```javascript
  for (const currentCollection of dataSource.collections) {
    if (currentCollection.schema.fields.createdAt) {
      currentCollection.removeField('createdAt');
    }

    if (currentCollection.schema.fields.updatedAt) {
      currentCollection.removeField('updatedAt');
    }
  }
}
```

</details>

<details>
<summary><strong>class RemoveTimestamps < ForestAdminDatasourceCustomizer::Plugins::Plugin</strong></summary>

```ruby
  def run(datasource, _collection, _options)
    datasource.collections.each do |current_collection|
      if current_collection.schema[:fields]['createdAt']
        current_collection.remove_field('createdAt')
      end

      if current_collection.schema[:fields]['updatedAt']
        current_collection.remove_field('updatedAt')
      end
    end
  end
end
```

</details>


