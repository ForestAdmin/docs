## Steps

Creating a custom data source will require you to work on the 3 following steps:

1. Declare the structure of the data
2. Declare the API capabilities
3. Code a translation layer

## Minimal example


```javascript
const {
  BaseCollection,
  BaseDataSource,
} = require('@forestadmin/datasource-toolkit');
const axios = require('axios'); // client for the target API

// The real work is in writing this module
// Expect a full featured query translation module to be over 1000 LOCs
const QueryGenerator = require('./forest-query-translation');

/** Minimal implementation of a readonly data source */
class MyCollection extends BaseCollection {
  constructor(dataSource) {
    // Set name of the collection once imported
    super('myCollection', dataSource);

    this.addField('id', {
      // Structure
      type: 'Column',
      columnType: 'Number',
      isPrimaryKey: true,
      isReadOnly: true, // field is readonly

      // As we are using the query translation strategy => define capabilities
      filterOperators: new Set(), // field is not filterable
      isSortable: false, // field is not sortable
    });

    this.addField('title', {
      type: 'Column',
      columnType: 'String',
      isReadOnly: true,
      filterOperators: new Set(),
      isSortable: false,
    });
  }

  async list(caller, filter, projection) {
    const params = QueryGenerator.generateListQueryString(filter, projection);
    const response = axios.get('https://my-api/my-collection', { params });

    return response.body.items;
  }

  async aggregate(caller, filter, aggregation, limit) {
    const params = QueryGenerator.generateAggregateQueryString(
      filter,
      aggregation,
      limit,
    );
    const response = axios.get('https://my-api/my-collection', { params });

    return response.body.items;
  }
}

class MyDataSource extends BaseDataSource {
  constructor() {
    super();
    this.addCollection(new MyCollection(this)); // For all your collections
  }
}

module.exports = MyDataSource;
```

{% tabs %}
{% tab title="const MyDataSource = require('./datasource');" %}
```javascript
const myDataSourceFactory = async () => new MyDataSource();

const agent = createAgent(options).addDataSource(myDataSourceFactory);
```
{% endtab %}

{% tab title="require 'net/http'" %}
```ruby
require 'json'
require 'uri'

module App
  module Collections
    # Minimal implementation of a readonly data source
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      # The real work is in writing this module
      # Expect a full featured query translation module to be over 1000 LOCs
      include QueryGenerator

      def initialize(datasource)
        # Set name of the collection once imported
        super(datasource, 'MyCollection')

        add_field('id', ColumnSchema.new(
          column_type: 'Number',
          filter_operators: [], # As we are using the query translation strategy => define capabilities
          is_primary_key: true,
          is_read_only: true, # field is readonly
          is_sortable: false # field is not sortable
        ))

        add_field('title', ColumnSchema.new(
          column_type: 'String',
          filter_operators: [],
          is_primary_key: false,
          is_read_only: true,
          is_sortable: false
        ))
      end

      def list(caller, filter, projection)
        params = QueryGenerator.generate_list_query_string(filter, projection)
        uri = URI.parse('http://my-api/my-collection')
        uri.query = URI.encode_www_form(params)

        response = Net::HTTP.get_response(uri)
        body = JSON.parse(response.body)

        body['items']
      end

      def aggregate(caller, filter, aggregation, limit = nil)
        params = QueryGenerator.generate_aggregate_query_string(filter, aggregation, limit)
        uri = URI.parse('http://my-api/my-collection')
        uri.query = URI.encode_www_form(params)

        response = Net::HTTP.get_response(uri)
        body = JSON.parse(response.body)

        body
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

```ruby
module App
  class MyDatasource < ForestAdminDatasourceToolkit::Datasource
    def initialize
      super
      add_collection(Collections::MyCollection.new(self)) # List of your collections
    end
  end
end
```

```ruby
lambda do
    datasource = new MyDatasource;
    agent = new ForestAdminAgent::Builder::AgentFactory([]);
    agent.add_datasource(datasource);
end
```

## Read more

Implementing a data source using the "query translation" strategy is an advanced concept: you will need to have a deep understanding of Forest Admin internals.

This strategy is a good match when writing data sources for full-featured databases.

Before starting, it is highly advised to read and understand the following sections:

- [Data Model Typing](../../../under-the-hood/data-model/typing.md)
- [Data Model Relationships](../../../under-the-hood/data-model/relationships.md)
- [Query interface](../../../datasources/getting-started/queries/README.md)
- [Fields and projections](../../../datasources/getting-started/queries/fields-projections.md)
- [Filters](../../../datasources/getting-started/queries/filters.md)
- [Aggregations](../../../datasources/getting-started/queries/aggregations.md)
