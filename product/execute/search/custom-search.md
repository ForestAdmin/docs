In Forest Admin, pages that show lists of records have a free-text search bar on top of them.

![A search bar on the main Table View](../assets/search-bar.png)

## Search modes

2 search modes are supported: "normal" and "extended".

- All searches start by being a "normal" search.
- If the records users are looking for are not found, it is possible to trigger an "extended" search from the footer.

![Extended search call to action](../assets/search-bar-extended.png)

## Default behavior

Natively, the search behavior is to seek value occurrences within columns of the Collection (in normal mode), and columns of the Collections of direct relations (in extended mode).

By default, Forest Admin will search only in specific columns, depending on their type:

| Column Type | Default search behavior                                                 |
| ----------- | ----------------------------------------------------------------------- |
| Enum        | Column is equal to the search string (case-insensitive).                |
| Number      | Column is equal to the search string (if the search string is numeric). |
| String      | Column contains the search string (case-insensitive).                   |
| Uuid        | Column is equal to the search string.                                   |
| Other types | Column is ignored by the default search handler.                        |

## Customization

If you want to make a column searchable, you must define the right operator to allow the search to be performed.
Please refer to the [Operators to support](fields/filter.md#operators-to-support-to-enable-the-search) table to know which operator to define.

Alternatively, you may want to change how the search behaves in your admin panel.

For instance:

- search only on the columns that are relevant to your use case.
- use full-text indexes (i.e. Postgres `tsquery` and `tsvector`, Algolia, Elastic search, ...)

{{#nodejs,php,python,ruby}}
To customize the search behavior, you must define a handler that returns a [`ConditionTree`](../datasources/getting-started/queries/filters.md#condition-trees).
{{/nodejs,php,python,ruby}}

{{#nodejs}}

### Extending the default search

The context received by the function `replaceSearch` can have access to the default search behavior and extend it:

```javascript
agent.customizeCollection('people', collection => {
  collection.replaceSearch((searchString, extendedMode, context) => {
    // This function in the context gives access to the default search behavior
    // including the advanced search
    return context.generateSearchFilter(searchString, {
      // Include fields from the first level of relations when true
      extended: extendedMode,
      // Remove these fields from the default search fields
      excludeFields: ['gender'],
      // Add these fields to the default search field
      includeFields: ['address:street'],
      // Replace the list of default searched field by these fields
      // onlyFields: [],
    });
  });
});
```

The condition tree returned by `generateSearchFilter` can be combined with other custom conditions:

```javascript
agent.customizeCollection('people', collection => {
  collection.replaceSearch((searchString, extendedMode, context) => {
    const defaultCondition = context.generateSearchFilter(searchString, {
      extended: extendedMode,
    });

    return {
      aggregator: 'Or',
      conditions: [
        defaultCondition,
        { field: 'address:street', operator: 'IContains', value: searchString },
      ],
    };
  });
});
```

{{/nodejs}}

### Making the search case-sensitive by default

In this example, we use the `searchExtended` condition to toggle between case-sensitive and insensitive searches.

```javascript
agent.customizeCollection('people', collection => {
  collection.replaceSearch((searchString, extendedMode) => {
    const operator = extendedMode ? 'Contains' : 'IContains';

    return {
      aggregator: 'Or',
      conditions: [
        { field: 'firstName', operator, value: searchString },
        { field: 'lastName', operator, value: searchString },
      ],
    };
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'People',
    function (CollectionCustomizer $builder) {
        $builder->replaceSearch(
            function ($searchString, $extendedMode) {
           	    $operator = $extendedMode ? Operators::CONTAINS : Operators::ICONTAINS;

                return [
                	'aggregator' => 'Or',
                	'conditions' => [
                	    ['field' => 'firstName', 'operator' => $operator, 'value' => $searchString],
                	    ['field' => 'lastName', 'operator' => $operator, 'value' => $searchString],
                	]
                ];
            }
        );
    }
);
```

```ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('people') do |collection|
  collection.replace_search do |search_string, extended_search|
    operator = extended_search ? OPERATORS::CONTAINS : OPERATORS::I_CONTAINS

    {
      aggregator: 'Or',
      conditions: [
        { field: 'firstName', operator: operator, value: search_string },
        { field: 'lastName', operator: operator, value: search_string },
      ]
    }
  end
end
```

```python
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.base import ConditionTree
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.branch import ConditionTreeBranch
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf

def search_in_people(
    search_string, extended_search: bool, context: CollectionCustomizationContext
) -> ConditionTree:
    return ConditionTreeBranch(
        "or",
        [
            ConditionTreeLeaf("firstName", "contains", search_string),
            ConditionTreeLeaf("lastName", "contains", search_string),
        ],
    )


agent.customize_collection("people").replace_search(search_in_people)
```

### Changing searched columns

```javascript
const productReferenceRegexp = /^[a-f]{16}$/i;
const barCodeRegexp = /^[0-9]{10}$/i;

agent.customizeCollection('products', collection => {
  collection.replaceSearch(async (searchString, extendedMode, context) => {
    // User is searching using a product reference.
    if (productReferenceRegexp.test(searchString))
      return context.generateSearchFilter(searchString, {
        onlyFields: ['reference'],
      });

    // User is search a barcode
    if (barCode.test(searchString))
      return context.generateSearchFilter(searchString, {
        onlyFields: ['barCode'],
      });

    // User is searching something else, let's search in the product name only
    if (!extendedMode)
      return context.generateSearchFilter(searchString, { onlyFields: ['name'] });

    // In "extended" mode, we search on name, description and brand name
    return context.generateSearchFilter(searchString, {
      onlyFields: ['name', 'description', 'brand:name'],
    });
  });
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Product',
    function (CollectionCustomizer $builder) {
        $builder->replaceSearch(
            function ($searchString, $extendedMode) {
                $productReferenceRegexp = '/^[a-f]{16}$/i';
                $barCodeRegexp = '/^[0-9]{10}$/i';

           	    // User is searching using a product reference.
           	    if (preg_match($productReferenceRegexp, $str)) {
           	        return ['field' => 'reference', 'operator' => Operators::EQUAL, 'value' => $searchString];
           	    }

           	    // User is search a barcode
           	    if (preg_match($barCodeRegexp, $str)) {
           	        return ['field' => 'barCode', 'operator' => Operators::EQUAL, 'value' => $searchString];
           	    }

           	    // User is searching something else, in "normal" mode, let's search in the product name only
           	    if (! $extendedMode) {
           	        return ['field' => 'name', 'operator' => Operators::CONTAINS, 'value' => $searchString];
           	    }

           	    // In "extended" mode, we search on name, description and brand name
                return [
                	'aggregator' => 'Or',
                	'conditions' => [
                	    ['field' => 'name', 'operator' => Operators::CONTAINS, 'value' => $searchString],
                	    ['field' => 'description', 'operator' => Operators::CONTAINS, 'value' => $searchString],
                	    ['field' => 'brand:name', 'operator' => Operators::EQUAL, 'value' => $searchString],
                	]
                ];
            }
        );
    }
);
```

```ruby
include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('product') do |collection|
  product_reference_regexp = /^[a-f]{16}$/i
  barcode_regexp = /^[0-9]{10}$/i

  collection.replace_search do |search_string, extended_search|
    # User is searching using a product reference.
    if product_reference_regexp.match?(search_string)
      return { field: 'reference', operator: OPERATORS::EQUAL, value: search_string }
    end

    # User is search a barcode
    if barcode_regexp.match?(search_string)
      return { field: 'barCode', operator: OPERATORS::EQUAL, value: search_string }
    end

    # User is searching something else, let's search in the product name only
    if !extended_search
      return { field: 'name', operator: OPERATORS::CONTAINS, value: search_string }
    end

    # In "extended" mode, we search on name, description and brand name
    {
      aggregator: 'Or',
      conditions: [
        { field: 'name', operator: OPERATORS::CONTAINS, value: search_string },
        { field: 'description', operator: OPERATORS::CONTAINS, value: search_string },
        { field: 'brand:name', operator: OPERATORS::EQUAL, value: search_string },
      ]
    }
  end
end

```

```python
import re
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.base import ConditionTree
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.branch import ConditionTreeBranch
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf


product_reference_regexp = r"^[a-f]{16}$"
barcode_regexp = r"^[0-9]{10}$"

async def search_in_products(
    search_string, extended_search: bool, context: CollectionCustomizationContext
) -> ConditionTree:
    # User is searching using a product reference.
    if re.match(product_reference_regexp, search_string):
        return ConditionTreeLeaf("reference", "equal", search_string)

    # User is search a barcode
    if re.match(barcode_regexp, search_string):
        return ConditionTreeLeaf("barCode", "equal", int(search_string))

    # User is searching something else, let's search in the product name only
    if not extended_search:
        return ConditionTreeLeaf("name", "contains", search_string)

    # In "extended" mode, we search on name, description and brand name
    return ConditionTreeBranch(
        "or",
        [
            ConditionTreeLeaf("name", "contains", search_string),
            ConditionTreeLeaf("description", "contains", search_string),
            ConditionTreeLeaf("brand:name", "equal", search_string),
        ],
    )


agent.customize_collection("products").replace_search(search_in_products)
```

### Calling an external API

If your data is indexed using a SaaS, an external store, or a full-text index, you can call it in the search handler.

```javascript
const algoliasearch = require('algoliasearch');
const client = algoliasearch('APPLICATION_ID', 'WRITE_API_KEY');
const index = client.initIndex('indexName');

agent.customizeCollection('products', collection =>
  collection.replaceSearch(async (searchString, extendedMode, context) => {
    const { hits } = index.search(searchString, {
      attributesToRetrieve: ['id'],
      hitsPerPage: 50,
    });

    return { field: 'id', operator: 'In', value: hits.map(h => h.id) };
  });
);
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use Algolia\AlgoliaSearch\SearchClient;

$forestAgent->customizeCollection(
    'Product',
    function (CollectionCustomizer $builder) {
        $builder->replaceSearch(
            function ($searchString, $extendedMode) {
                $client = SearchClient::create("YourApplicationID", "YourWriteAPIKey");
                $index = $client->initIndex("test_index");
                $results = $index->search(
                    $searchString,
                    [
                        'attributesToRetrieve' => ['id'],
                        'hitsPerPage' => 50,
                    ]
                );

                return ['field' => 'id', 'operator' => Operators::IN, 'value' => array_map(fn ($hit) => $hit['id'], $results)];
            }
        );
    }
);
```

```ruby
require 'algolia'

include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

@create_agent.customize_collection('product') do |collection|
  collection.replace_search do |search_string, extended_search|
    client = Algolia::Search::Client.create('YourApplicationID', 'YourWriteAPIKey')
    index = client.init_index('test_index')
    hits = index.search(
      search_string,
      {
        attributesToRetrieve: ['id'],
        hitsPerPage: 50,
      }
    )

    {
      field: 'id',
      operator: OPERATORS::IN,
      value: hits.map { |hit| hit['id'] }
    }
  end
end
```

```python
from forestadmin.datasource_toolkit.context.collection_context import CollectionCustomizationContext
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.base import ConditionTree
from forestadmin.datasource_toolkit.interfaces.query.condition_tree.nodes.leaf import ConditionTreeLeaf
from algoliasearch.search_client import SearchClient

client = SearchClient.create("YourApplicationID", "YourWriteAPIKey")
index = client.init_index("test_index")


async def search_in_products(
    search_string, extended_search: bool, context: CollectionCustomizationContext
) -> ConditionTree:
    hits = index.search(
        search_string,
        {"attributesToRetrieve": ["id"], "hitsPerPage": 50},
    )
    return ConditionTreeLeaf("id", "in", [hit["id"] for hit in hits])


agent.customize_collection("products").replace_search(search_in_products)
```

### Disable the search

By default, the search bar is displayed when at least one field supports the operator used for search based on its type ([see this table](#default-behavior)).

You can remove the search bar by disabling the search on a collection:

```javascript
agent.customizeCollection('Products', collection => {
  collection.disableSearch();
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;

$forestAgent->customizeCollection(
    'Product',
    function (CollectionCustomizer $builder) {
        $builder->disableSearch();
    }
);
```

```ruby
@create_agent.customize_collection('product') do |collection|
  collection.disable_search
end
```

```python
agent.customize_collection("product").disable_search()
```
