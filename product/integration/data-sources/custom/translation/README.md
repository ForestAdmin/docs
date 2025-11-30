## Steps

Creating a custom data source will require you to work on the 3 following steps:

1. Declare the structure of the data
2. Declare the API capabilities
3. Code a translation layer

## Minimal example

```python
from typings import Optional

from forestadmin.agent_toolkit.utils.context import User
from forestadmin.datasource_toolkit.collections import Collection
from forestadmin.datasource_toolkit.datasources import Datasource
from forestadmin.datasource_toolkit.interfaces.query.aggregation import AggregateResult, Aggregation
from forestadmin.datasource_toolkit.interfaces.query.filter.paginated import PaginatedFilter
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter
from forestadmin.datasource_toolkit.interfaces.query.projections import Projection
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

import requests  # client for the target API

# The real work is in writing this module
# Expect a full featured query translation module to be over 1000 LOCs
from .forest_query_translation import QueryGenerator

# Minimal implementation of a readonly data source
class MyCollection(Collection):
    def __init__(self, datasource):
        # Set name of the collection once imported
        super().__init__("MyCollection", datasource)

        # structure
        self.add_field("id", {
            "type": "Column",
            "column_type": "Number",
            "is_primary_key": True,
            "is_read_only": True,  # field is readonly
            # As we are using the query translation strategy => define capabilities
            "filter_operators": set(), # field is not filterable
            "is_sortable": False, # field is not sortable
        })

        self.add_field("title", {
            "type": "Column",
            "column_type": "String",
            "is_read_only": True,
            "filter_operators": set(),
            "is_sortable": False,
        })

    async def list(
        self,
        caller: User,
        filter_: PaginatedFilter,
        projection: Projection
    ) -> List[RecordsDataAlias]:
        params = QueryGenerator.generate_list_query_string(filter_, projection)
        response = requests.get('https://my-api/my-collection', params)

        return response.json()["items"]

    async def aggregate(
        self,
        caller: User,
        filter_: Filter,
        aggregation: Aggregation,
        limit: Optional[int]
    ) -> List[AggregateResult]:
        params = QueryGenerator.generate_aggregate_query_string(
            filter_,
            aggregation,
            limit
        )
        response = requests.get('https://my-api/my-collection', params)

        return response.json()["items"]


class MyDatasource(Datasource):
    def __init__(self):
        super().__init__()
        self.add_collection(MyCollection(self))

```

```python
from custom_datasources.my_datasource import MyDatasource

agent.add_datasource(MyDatasource())
```

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

```javascript
const MyDataSource = require('./datasource');

const myDataSourceFactory = async () => new MyDataSource();

const agent = createAgent(options).addDataSource(myDataSourceFactory);
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Caller;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Projection\Projection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Schema\ColumnSchema;
use GuzzleHttp\Client;
// The real work is in writing this module
// Expect a full featured query translation module to be over 1000 LOCs
use QueryGenerator;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        parent::__construct(
            $datasource,
            'MyCollection'
        );

        $this->addField('id', new ColumnSchema(
            columnType: 'Number',
            // As we are using the query translation strategy => define capabilities
            filterOperators: [], // field is not filterable
            isPrimaryKey: true,
            isReadOnly: true,  // field is readonly
            isSortable: false // field is not sortable
        ));

        $this->addField('title', new ColumnSchema(
            columnType: 'String',
            filterOperators: [],
            isPrimaryKey: false,
            isReadOnly: true,
            isSortable: false
        ));

        $this->client = new Client([
            'base_uri' => 'https://my-api/',
            'timeout'  => 5.0,
        ]);
    }

    public function list(Caller $caller, Filter $filter, Projection $projection): array
    {
        $params = QueryGenerator::generateListQueryString($filter, $projection);

        try {
            $response = $this->client->get('my-collection', [
                'query' => $params,
            ]);

            $body = json_decode($response->getBody()->getContents(), true);

            return $body['items'] ?? [];
        } catch (\Exception $e) {
            throw new \RuntimeException('Failed to fetch items: ' . $e->getMessage());
        }
    }

    public function aggregate(Caller $caller, Filter $filter, Aggregation $aggregation, ?int $limit = null)
    {
        $params = QueryGenerator::generateAggregateQueryString($filter, $aggregation, $limit);

        try {
            $response = $this->client->get('my-collection', [
                'query' => $params,
            ]);

            $body = json_decode($response->getBody()->getContents(), true);

            return $body['items'] ?? [];
        } catch (\Exception $e) {
            throw new \RuntimeException('Failed to fetch items: ' . $e->getMessage());
        }
    }
}
```

```php
<?php

use Collections\MyCollection;
use ForestAdmin\AgentPHP\DatasourceToolkit\DataSource;

class MyDataSource extends DataSource
{
    public function __construct()
    {
        parent::__construct();
        $this->addCollection(new MyCollection($this)); // List of your collections
    }

```

```php
<?php

use ForestAdmin\AgentPHP\Agent\Builder\AgentFactory;

return static function () {
    $dataSource = new MyDataSource();
    $agent = new AgentFactory([]);
    $agent->addDatasource($dataSource);

    return $dataSource;
};
```

```ruby
require 'net/http'
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
