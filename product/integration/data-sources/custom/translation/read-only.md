Developing your query translation layer is much easier when you can preview your work and have intermediary deliverables.

Emulation comes to the rescue: all features that need to be implemented when making a translating data source can be emulated inside your Node.js, at the cost of performance.

This enables you to be up and running in minutes and then optimize your code as you go.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
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
{% endtab %}

{% tab title="Ruby" %}
```ruby
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
{% endtab %}
{% endtabs %}


# Tips

## Count queries

The `aggregate` method is used by Forest Admin both to count records and to extract the data which is needed to generate charts.

If the API/Database you are targeting has an efficient API that is made for counting records, you may want to handle this case first.

{% tabs %}
{% tab title="JavaScript" %}
```javascript
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
{% endtab %}

{% tab title="Ruby" %}
```ruby
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
{% endtab %}
{% endtabs %}


