Developing your query translation layer is much easier when you can preview your work and have intermediary deliverables.

Emulation comes to the rescue: all features that need to be implemented when making a translating data source can be emulated inside your Node.js, at the cost of performance.

This enables you to be up and running in minutes and then optimize your code as you go.

```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');
const axios = require('axios');

/**
 * This collection will have terrible performance, but is perfect to test that the
 * structure declaration is well done.
 */
class MyCollection extends BaseCollection {
  // [... Declare structure and capabilities]

  async list(caller, filter, projection) {
    // Fetch all records on all requests (this is _very_ inefficient)
    const response = await axios.get('https://my-api/my-collection');
    const result = response.data.items;

    // Use "in-process emulation" for everything else.
    if (filter.conditionTree)
      result = filter.conditionTree.apply(result, this, caller.timezone);
    if (filter.sort) result = filter.sort.apply(result);
    if (filter.page) result = filter.page.apply(result);
    if (filter.segment)
      throw new Error('This collection does not implements native segments');
    if (filter.search) throw new Error('This collection is not natively searchable');

    return projection.apply(result);
  }

  async aggregate(caller, filter, aggregation, limit) {
    // Fetch all records which should be aggregated
    const records = await this.list(caller, filter, aggregation.projection);

    // Use "in-process emulation" to aggregate the results
    return aggregation.apply(records, caller.timezone, limit);
  }
}
```

```python
from typings import Optional

from forestadmin.agent_toolkit.utils.context import User
from forestadmin.datasource_toolkit.collections import Collection
from forestadmin.datasource_toolkit.interfaces.query.aggregation import AggregateResult, Aggregation
from forestadmin.datasource_toolkit.interfaces.query.filter.paginated import PaginatedFilter
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter
from forestadmin.datasource_toolkit.interfaces.query.projections import Projection
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

import requests

class MyCollection(Collection):
"""
    This collection will have terrible performance, but is perfect to test that the
    structure declaration is well done.
"""
    # [... Declare structure and capabilities]
    async def list(
        self,
        caller: User,
        filter_: PaginatedFilter,
        projection: Projection
    ) -> List[RecordsDataAlias]:
        # Fetch all records on all requests (this is _very_ inefficient)
        response = requests.get('https://my-api/my-collection')
        result = response.json()["items"]

        # Use "in-process emulation" for everything else.
        if filter_.condition_tree:
            result = filter_.condition_tree.apply(result, self, str(caller.timezone))
        if filter_.sort:
            result = filter_.sort.apply(result)
        if filter_.page:
            result = filter_.page.apply(result)
        if filter_.segment:
            raise Exception('This collection does not implements native segments')
        if filter_.search:
            raise Exception('This collection is not natively searchable')

        return projection.apply(result)

    async def aggregate(
        self,
        caller: User,
        filter_: Filter,
        aggregation: Aggregation,
        limit: Optional[int]
    ) -> List[AggregateResult]:
      # Fetch all records which should be aggregated
      records = await self.list(
        caller, PaginatedFilter.from_base_filter(filter_), aggregation.projection
      )

      # Use "in-process emulation" to aggregate the results
      return aggregation.apply(records, str(caller.timezone), limit)

```

```ruby
require 'httparty'

module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      # [... Declare structure and capabilities]

      def list(caller, filter, projection)
        # Fetch all records on all requests (this is _very_ inefficient)
        response = HTTParty.get('https://my-api/my-collection')
        result = response.parsed_response['items']

        if filter.condition_tree
          result = filter.condition_tree.apply(response, self, caller.timezone)
        end

        if filter.sort
          result = filter.sort.apply(result)
        end

        if filter.page
          result = filter.page.apply(result)
        end

        if filter.segment
          raise 'This collection does not implement native segments'
        end

        if filter.search
          raise 'This collection is not natively searchable'
        end

        projection.apply(result)
      end

      def aggregate(caller, filter, aggregation, limit = nil)
        # Fetch all records which should be aggregated
        records = list(caller, filter, aggregation.projection)

        # Use "in-process emulation" to aggregate the results
        aggregation.apply(records, caller.timezone, limit)
      end
    end
  end
end
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Caller;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Projection\Projection;

/**
 * This collection will have terrible performance, but is perfect to test that the
 * structure declaration is well done.
 */
class MyCollection extends Collection
{
    // [... Declare structure and capabilities]
    //  $this->client = new \GuzzleHttp\Client([...]);

    public function list(Caller $caller, Filter $filter, Projection $projection): array
    {
        // Fetch all records on all requests (this is _very_ inefficient)
        try {
            $response = $this->client->get('https://my-api/my-collection');
            $result = json_decode($response->getBody()->getContents(), true)['items'] ?? [];

            if ($filter->getConditionTree()) {
                $result = $filter->getConditionTree()->apply($response, $this, $caller->getTimezone());
            }
            if ($filter->getSort()) {
                $result = $filter->getSort()->apply($result);
            }
            if ($filter->getPage()) {
                $result = $filter->getPage()->apply($result);
            }
            if ($filter->getSegment()) {
                throw new \RuntimeException('This collection does not implements native segments');
            }
            if ($filter->getSearch()) {
                throw new \RuntimeException('This collection is not natively searchable');
            }

            return $projection->apply($result);
        } catch (\Exception $e) {
            throw new \RuntimeException('Failed to fetch or process items: ' . $e->getMessage());
        }
    }

    public function aggregate(Caller $caller, Filter $filter, Aggregation $aggregation, ?int $limit = null)
    {
        // Fetch all records which should be aggregated
        $records = $this->list($caller, $filter, $aggregation->getProjection());

        return $aggregation->apply($records, $caller->getTimezone(), $limit);
    }
```

# Tips

## Count queries

The `aggregate` method is used by Forest Admin both to count records and to extract the data which is needed to generate charts.

If the API/Database you are targeting has an efficient API that is made for counting records, you may want to handle this case first.

```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');
const axios = require('axios');

 class MyCollection extends BaseCollection {
  // [... Declare structure, capabilities and list method]

  async aggregate(caller, filter, aggregation, limit) {
    const { operation, fields, groups } = aggregation;

    if (operation === 'Count' && groups.length === 0 && !field) {
      return [{ value: await this.count(caller, filter) }];
    }

    // [... handle the general case]
  }

  async count(caller, filter) {
    const response = await axios.get(
      `https://my-api/my-collection/count`,
      { params: { filter: this._translateFilter(caller, filter) } }
    );

    return response.data.count;
  }

  private _translateFilter(caller, filter) {
    // [... translate filter]
  }
}
```

```python
from forestadmin.agent_toolkit.utils.context import User
from forestadmin.datasource_toolkit.collections import Collection
from forestadmin.datasource_toolkit.interfaces.query.aggregation import AggregateResult, Aggregation
from forestadmin.datasource_toolkit.interfaces.query.filter.unpaginated import Filter

import requests

class MyCollection(Collection):
    # [... Declare structure and capabilities]

    async def aggregate(
        self,
        caller: User,
        filter_: Filter,
        aggregation: Aggregation,
        limit: Optional[int]
    ) -> List[AggregateResult]:
        if aggregation.operation.value == "Count" and len(aggregation.groups) == 0:
            return [{"value": await self.count(caller, filter_)}]

    async def count(self, caller: User, filter_: Filter):
        response = requests.get('https://my-api/my-collection/count', self._translate_filter(caller, filter_))
        records = response.json()["items"]

    def _translate_filter(self, caller: User, filter_: Filter):
        # [... translate filter]

```

```ruby
require 'httparty'

module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      # [... Declare structure, capabilities and list method]

      def aggregate(caller, filter, aggregation, limit = nil)
        if aggregation.operation == 'Count' && aggregation.groups.empty? && !aggregation.field
          return [{ 'value' => count(caller, filter) }]
        end

        # [... handle the general case]
      end

      def count(caller, filter)
        response = HTTParty.get('https://my-api/my-collection/count', query: { filter: translate_filter(caller, filter) })

        JSON.parse(response.body)['count']
      end

      private

      def translate_filter(caller, filter)
        # [... translate filter]
      end
    end
  end
end
```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Caller;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Aggregation;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Projection\Projection;

class MyCollection extends Collection
{
    // [... Declare structure, capabilities and list method]
    //  $this->client = new \GuzzleHttp\Client([...]);

    public function aggregate(Caller $caller, Filter $filter, Aggregation $aggregation, ?int $limit = null)
    {
        if ($aggregation->getOperation() === 'Count' && empty($aggregation->getGroups()) && ! $aggregation->getField()) {
            return [
                ['value' => $this->count($caller, $filter)],
            ];
        }

        // [... handle the general case]
    }

    public function count(Caller $caller, Filter $filter)
    {
        $response = $this->client->get('https://my-api/my-collection/count', [
            'query' => ['filter' => $this->translateFilter($caller, $filter)],
        ]);

        return json_decode($response->getBody()->getContents(), true)['count'];
    }

    private function translateFilter(Caller $caller, Filter $filter)
    {
        // [... translate filter]
    }
}

```
