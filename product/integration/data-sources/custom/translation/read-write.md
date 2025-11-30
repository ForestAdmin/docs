Making your records editable is achieved by implementing the `create`, `update` and `delete` methods.

The 3 methods take a [filter](../../getting-started/queries/filters.md) as a parameter but note that, unlike the `list` method, there is no need to support paging.

```javascript
const { BaseCollection } = require('@forestadmin/datasource-toolkit');
const axios = require('axios'); // client for the target API

/** Naive implementation of create, update and delete on a REST API */
class MyCollection extends BaseCollection {
  constructor() {
    this.addField('id', { /* ... */ isReadOnly: true });
    this.addField('title', { /* ... */ isReadOnly: false });
  }

  async create(caller, records) {
    const promises = records.map(async record => {
      const response = await axios.post('https://my-api/my-collection', record);
      return response.data;
    });

    return Promise.all(promises); // Must return newly created records
  }

  async update(caller, filter, patch) {
    const recordIds = await this.list(caller, filter, ['id']); // Retrieve ids
    const promises = recordIds.map(async ({ id }) => {
      await axios.patch(`https://my-api/my-collection/${id}`, patch);
    });

    await Promise.all(promises);
  }

  async delete(caller, filter) {
    const recordIds = await this.list(caller, filter, ['id']); // Retrieve ids
    const promises = recordIds.map(async ({ id }) => {
      await axios.delete(`https://my-api/my-collection/${id}`);
    });

    await Promise.all(promises);
  }
}
```

```python
from typings import Optional

from forestadmin.agent_toolkit.utils.context import User
from forestadmin.datasource_toolkit.collections import Collection
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

import requests

class MyCollection(Collection):
    def __init__(self):
        self.addField('id', {
            "is_read_only": True,
            # ...
        })
        self.addField('title', {
            "is_read_only": False,
            # ...
        })

    async def create(
        self, caller: User, List[RecordsDataAlias]
    ) -> List[RecordsDataAlias]:
        records_to_return = []
        for record in data:
            response = requests.post('https://my-api/my-collection', json=record)
            records_to_return = response.json()

        return records_to_return

    async def update(
        self, caller: User, filter_: Optional[Filter], patch: RecordsDataAlias
    ) -> None:
        record_ids = await self.list(caller, filter_, ["id"])
        for record_id in record_ids:
            requests.patch(f'https://my-api/my-collection/{record_id}', json=patch)

    async def delete(
        self, caller: User, filter_: Optional[Filter]
    ) -> None:
        record_ids = await self.list(caller, filter_, ["id"])
        for record_id in record_ids:
            requests.delete(f'https://my-api/my-collection/{record_id}')

```

```php
<?php

use ForestAdmin\AgentPHP\DatasourceToolkit\Collection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Caller;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Contracts\DatasourceContract;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Filters\Filter;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\Projection\Projection;
use ForestAdmin\AgentPHP\DatasourceToolkit\Schema\ColumnSchema;
use GuzzleHttp\Client;

class MyCollection extends Collection
{
    public function __construct(DatasourceContract $datasource)
    {
        parent::__construct(
            $datasource,
            'MyCollection'
        );

        $this->addField('id', new ColumnSchema(
            // ...
            isReadOnly: true,
        ));

        $this->addField('title', new ColumnSchema(
            // ...
            isReadOnly: false,
        ));

        $this->client = new Client([
            'base_uri' => 'https://my-api/',
            'timeout'  => 5.0,
        ]);
    }

    public function create(Caller $caller, array $records)
    {
        $results = [];

        foreach ($records as $record) {
            try {
                $response = $this->httpClient->post(
                    'my-collection',
                    ['json' => $record]
                );

                $results[] = json_decode($response->getBody()->getContents(), true);
            } catch (\Exception $e) {
                throw new \Exception("Error creating record: " . $e->getMessage(), 0, $e);
            }
        }

        return $results;
    }

    public function update(Caller $caller, Filter $filter, array $patch)
    {
        $recordIds = $this->list($caller, $filter, new Projection('id'));

        foreach ($recordIds as $record) {
            try {
                $this->client->patch("my-collection/{$record['id']}", [
                    'json' => $patch,
                ]);
            } catch (\Exception $e) {
                throw new \Exception("Error updating record {$record['id']}: " . $e->getMessage());
            }
        }

    }

    public function delete(Caller $caller, Filter $filter): void
    {
        $recordIds = $this->list($caller, $filter, new Projection('id'));

        foreach ($recordIds as $record) {
            try {
                $this->client->delete("my-collection/{$record['id']}");
            } catch (\Exception $e) {
                throw new \Exception("Error updating record {$record['id']}: " . $e->getMessage());
            }
        }
    }
}

```

```ruby
module App
  module Collections
    class MyCollection < ForestAdminDatasourceToolkit::Collection
      include ForestAdminDatasourceToolkit::Schema
      include ForestAdminDatasourceToolkit::Components::Query
      include HTTParty

      base_uri 'https://my-api'

      def initialize(datasource)
        super(datasource, 'Article')

        add_field('id', ColumnSchema.new(
          # ...
          is_read_only: true,
        ))

        add_field('title', ColumnSchema.new(
          # ...
          is_read_only: false,
        ))
      end

      def create(caller, records)
        results = []

        records.each do |record|
          response = self.class.post(
            '/my-collection',
            body: record.to_json,
            headers: { 'Content-Type' => 'application/json' }
          )

          results << JSON.parse(response.body)
        end

        results
      end

      def update(caller, filter, patch)
        record_ids = list(caller, filter, Projection.new(['id']))

        record_ids.each do |record|
          self.class.patch(
            "/my-collection/#{record['id']}",
            body: patch.to_json,
            headers: { 'Content-Type' => 'application/json' }
          )
        end
      end

      def delete(caller, filter)
        record_ids = list(caller, filter, Projection.new(['id']))

        record_ids.each do |record|
          self.class.delete(
            "/my-collection/#{record['id']}",
            headers: { 'Content-Type' => 'application/json' }
          )
        end
      end

      private

      def list(caller, filter, projection)
        # Implémentation de la méthode `list` pour récupérer les IDs des enregistrements
        raise NotImplementedError, 'list method is not implemented'
      end
    end
  end
end
```
