When customizing your Agent behavior, it is quite common to have to perform the same tasks on multiple Fields and Collections.

Plugins are the answer to that need, and you are strongly encouraged to use them everywhere you notice that your customization files could benefit from code factorization.

## Minimal example

### Using plugins

Plugins are used by either importing a module, or installing the relevant package, and then calling the `use` method.

Depending on the plugin, options may be provided.

```javascript
import { createAgent } from '@forestadmin/agent';
import { createFileField } from '@forestadmin/plugin-aws-s3';
import { removeTimestamps } from './plugins/remove-timestamps';

// The .use() method can be called both on the agent and on collections.
createAgent()
  // Some plugins do not require options
  .use(removeTimestamps)

  // Others do
  .customizeCollection('accounts', collection =>
    collection.use(createFileField, { fieldname: 'avatar' }),
  );
```

```php
<?php

use ForestAdmin\AgentPHP\Agent\Utils\Env;
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceDoctrine\DoctrineDatasource;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;
use MyNamespace\Plugins\CreateFileField;
use MyNamespace\Plugins\RemoveTimestamps;

// The use() method can be called both on the agent and on collections
return static function (ForestAgent $forestAgent) {

    $forestAgent->addDatasource(
        new DoctrineDatasource($forestAgent->getEntityManager(), [
                'url'      => Env::get('DATABASE_URL'),
            ]),
        )

        // Some plugins do not require options
        ->use(RemoveTimestamps::class)

        // Others do
        ->customizeCollection(
            'Book',
            fn (CollectionCustomizer $builder) => $builder->use(
                CreateFileField::class,
                ['fieldname': 'avatar']
            )
        )
```

```python
# The .use() method can be called both on the agent and on collections.
agent.use(
    # Some plugins do not require options
    RemoveTimestamps
    # Others do
).customize_collection('accounts').use(CreateFileField, { "fieldname": 'avatar' })
```

### Writing plugins

{{#nodejs}}
A plugin is nothing more than an `async function` that performs customizations.
{{/nodejs}}

{{#php}}
A plugin is nothing more than a `class` that implements our `Plugin interface` and can perform customizations.
{{/php}}

{{#python}}
A Plugin is nothing more than a `class` that implements our `Plugin interface` with an `async method` and can perform customizations.
{{/python}}

The full documentation can be found in the ["Write your own plugin" section](./custom.md).

```javascript
export async function removeTimestamps(dataSource, collection) {
  // Allow the plugin to be used both on the dataSource or on individual collections
  const collections = collection ? [collection] : dataSource.collections;

  // Remove fields
  for (const currentCollection of collections) {
    currentCollection.removeField('createdAt');
    currentCollection.removeField('updatedAt');
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
        // Allow the plugin to be used both on the dataSource or on individual collections
        $collections = $collectionCustomizer ? [$collectionCustomizer] : $datasourceCustomizer->getCollections();

        // Remove fields
        foreach ($datasourceCustomizer->getCollection() as $currentCollection) {
            $currentCollection->removeField('createdAt');
            $currentCollection->removeField('updatedAt');
        }
    }
}
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
        # Allow the plugin to be used both on the dataSource or on individual collections
        collections = (
            [collection_customizer]
            if collection_customizer
            else datasource_customizer.collections
        )


        for current_collection in collections:
            collection.remove_field("createdAt")
            collection.remove_field("updatedAt")
```
