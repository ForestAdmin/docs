{{#nodejs}}
Each Plugin is nothing more than an `async function` that can perform customizations at either Agent level, Collection level, or both.
{{/nodejs}}

{{#php}}
Each Plugin is nothing more than a `class` that implements our `Plugin interface` and can perform customizations at either Agent level, Collection level, or both.
{{/php}}

{{#ruby}}
Each Plugin is nothing more than a `class` that implements our `Plugin abstract class` and can perform customizations at either Agent level, Collection level, or both.
{{/ruby}}

{{#python}}
Each Plugin is nothing more than a `class` that implements our `Plugin interface` with an `async method` and can perform customizations at either Agent level, Collection level, or both.
{{/python}}

```javascript
export async function removeTimestamps(dataSource, collection, options) {
  // ... call customization methods here
}
```

```php
<?php

namespace ...;

use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\DatasourceCustomizer;

class RemoveTimestamps implements Plugin
{
    public function run(DatasourceCustomizer $datasourceCustomizer, ?CollectionCustomizer $collectionCustomizer = null, $options = []): void
    {
       // ... call customization methods here
    }
}
```

```ruby
class RemoveTimestamps < ForestAdminDatasourceCustomizer::Plugins::Plugin
  def run(datasource, collection, options)
    # ... call customization methods here
  end
end
```

```python
from forestadmin.datasource_toolkit.plugins.plugin import Plugin
# from forestadmin.datasource_toolkit.datasource_customizer.collection_customizer import (
#     CollectionCustomizer
# )
# from forestadmin.datasource_toolkit.datasource_customizer.datasource_customizer import (
#     DatasourceCustomizer
# )

class RemoveTimestamps(Plugin):
    async def run(
        self,
        datasource_customizer: "DatasourceCustomizer",  # noqa: F821
        collection_customizer: Optional["CollectionCustomizer"] = None,  # noqa: F821
        options: Optional[Dict] = {},
    ):
        # ... call customization methods here
```

3 parameters are provided:

{{#nodejs}}
| Name | Description |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataSource<br>([API Reference](https://forestadmin.github.io/agent-nodejs/classes/_forestadmin_datasource_customizer.DataSourceCustomizer.html)) | An object that allows customizing the whole agent. It has the same interface than the `Agent` you manipulate outside of Plugins |
| collection<br>([API Reference](https://forestadmin.github.io/agent-nodejs/classes/_forestadmin_datasource_customizer.CollectionCustomizer.html)) | An object that allows customizing the collection that the Plugin was called from (`null` if the Plugin was called on the Agent). It is the same object than is passed when you call `customizeCollection` |
| options | Options that are provided to the Plugin. There is no set structure for this parameter, as each Plugin will provide specific mandatory or optional options. |
{{/nodejs}}
{{#php}}
| Name | Description |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataSource | An object that allows customizing the whole agent. It has the same interface than the `Agent` you manipulate outside of Plugins |
| collection | An object that allows customizing the collection that the Plugin was called from (`null` if the Plugin was called on the Agent). It is the same object than is passed when you call `customizeCollection` |
| options | An array of options that are provided to the Plugin. Each Plugin will provide specific mandatory or optional options.
{{/php}}
{{#ruby}}
| Name | Description |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| dataSource | An object that allows customizing the whole agent. It has the same interface than the `Agent` you manipulate outside of Plugins |
| collection | An object that allows customizing the collection that the Plugin was called from (`nil` if the Plugin was called on the Agent). It is the same object than is passed when you call `customize_collection` |
| options | An array of options that are provided to the Plugin. Each Plugin will provide specific mandatory or optional options.
{{/ruby}}
{{#python}}
| Name | Description |
| ------------------------------------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| datasource_customizer | An object that allows customizing the whole agent. It has the same interface than the `Agent` you manipulate outside of Plugins |
| collection_customizer | An object that allows customizing the collection that the Plugin was called from (`None` if the Plugin was called on the Agent). It is the same object than is passed when you call `customize_collection` |
| options | An dictionary of options that are provided to the Plugin. Each Plugin will provide specific mandatory or optional options.
{{/python}}

## Making your Plugin act differently depending on the Collection

When making a Plugin, you may want it to generalize to many different Collections.

This can be achieved by adopting different behavior depending on the `schema` of the Collection being targeted.
{{#nodejs}}
Relevant documentation:

- [DataSourceSchema](https://forestadmin.github.io/agent-nodejs/types/_forestadmin_datasource_toolkit.DataSourceSchema.html) (for Plugins working on Charts)
- [CollectionSchema](https://forestadmin.github.io/agent-nodejs/types/_forestadmin_datasource_toolkit.CollectionSchema.html) (for all other Plugins)

{{/nodejs}}

```javascript
export async function removeTimestamps(dataSource, collection, options) {
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

```php
<?php

namespace ...;

use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceCustomizer\DatasourceCustomizer;

class RemoveTimestamps implements Plugin
{
    public function run(DatasourceCustomizer $datasourceCustomizer, ?CollectionCustomizer $collectionCustomizer = null, $options = []): void
    {
        foreach ($datasourceCustomizer->getCollection() as $currentCollection) {
            if ($currentCollection->getFields()['createdAt']) {
                $currentCollection->removeField('createdAt');
            }

            if ($currentCollection->getFields()['updatedAt']) {
                $currentCollection->removeField('updatedAt');
            }
        }
    }
}
```

```ruby
class RemoveTimestamps < ForestAdminDatasourceCustomizer::Plugins::Plugin
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

```python
from forestadmin.datasource_toolkit.plugins.plugin import Plugin


class RemoveTimestamps(Plugin):
    async def run(
        self,
        datasource_customizer: "DatasourceCustomizer",  # noqa: F821
        collection_customizer: Optional["CollectionCustomizer"] = None,  # noqa: F821
        options: Optional[Dict] = {},
    ):
        for collection in datasource_customizer.collections:
            if 'createdAt' collection.schema["fields"].keys():
                collection.remove_field("createdAt")

            if 'updatedAt' collection.schema["fields"].keys():
                collection.remove_field("updatedAt")

```
