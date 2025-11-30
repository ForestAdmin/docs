Making your records editable is achieved by implementing the `create`, `update` and `delete` methods.

The 3 methods take a [filter](../../getting-started/queries/filters.md) as a parameter but note that, unlike the `list` method, there is no need to support paging.

<details>
<summary><strong>const { BaseCollection } = require('@forestadmin/datasource-toolkit');</strong></summary>

```javascript
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

</details>

<details>
<summary><strong>module App</strong></summary>

```ruby
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

</details>
