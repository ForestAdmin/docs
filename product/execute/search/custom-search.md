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

{{#nodejs,ruby}}
To customize the search behavior, you must define a handler that returns a [`ConditionTree`](../datasources/getting-started/queries/filters.md#condition-trees).
{{/nodejs,ruby}}

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

<details>
<summary><strong>agent.customizeCollection('people', collection => {</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>include ForestAdminDatasourceToolkit::Components::Query::ConditionTree</strong></summary>

```ruby
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

</details>

<details>
<summary><strong>const productReferenceRegexp = /^[a-f]{16}$/i;</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>include ForestAdminDatasourceToolkit::Components::Query::ConditionTree</strong></summary>

```ruby
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

</details>

<details>
<summary><strong>const algoliasearch = require('algoliasearch');</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>require 'algolia'</strong></summary>

```ruby
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

</details>


### Disable the search

By default, the search bar is displayed when at least one field supports the operator used for search based on its type ([see this table](#default-behavior)).

You can remove the search bar by disabling the search on a collection:

<details>
<summary><strong>agent.customizeCollection('Products', collection => {</strong></summary>

```javascript
  collection.disableSearch();
});
```

</details>

<details>
<summary><strong>@create_agent.customize_collection('product') do |collection|</strong></summary>

```ruby
  collection.disable_search
end
```

</details>


