{% hint style="info" %}
Due to popular demand, Live Query Charts are planned to be reintroduced in a future version of the agent, but we don't have a timeline for it yet.
{% endhint %}

[Live Query Charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql) allowed the creation of charts from SQL queries from the UI.

{% hint style="info" %}
You can find the full documentation of chart customization [here](../../../../agent-customization/charts/README.md).
{% endhint %}

# Steps

## Step 1: Retrieve the SQL query from the UI

![Live Query Chart configuration screen](../../../../assets/migration-chart-sql.png)

You can retrieve the SQL query of a `Live query chart` by clicking on the `Cog` icon of the chart when using the `Edit Layout` mode.

## Step 2: Create a new API chart

The next step will be to create a new [API chart](../../../../agent-customization/charts/README.md) using the SQL query you retrieved in the previous step.

{% tabs %}
{% tab title="agent.addChart('appointments', async (context, resultBuilder) => {" %}
```javascript
const rows = await context.dataSource.getCollection('appointments').nativeDriver
    .rawQuery(`
      SELECT current.count AS value, previous.count AS previous
      FROM (
        SELECT COUNT(*)
        FROM appointments
        WHERE start_date BETWEEN '2018-01-01' AND '2018-02-01'
      ) as current, (
        SELECT COUNT(*)
        FROM appointments
        WHERE start_date BETWEEN '2017-12-01' AND '2018-01-01'
      ) as previous;`);

  return resultBuilder.value(rows[0].value, rows[0].previous);
});
```
{% endtab %}

{% tab title="module ForestAdminRails" %}
```ruby
class CreateAgent
    def self.customize
      @create_agent.add_chart('appointments') do |_context, result_builder|
        rows = ActiveRecord::Base.connection.execute("
          SELECT current.count AS value, previous.count AS previous
          FROM (
            SELECT COUNT(*)
            FROM appointments
            WHERE start_date BETWEEN '2018-01-01' AND '2018-02-01'
          ) as current, (
            SELECT COUNT(*)
            FROM appointments
            WHERE start_date BETWEEN '2017-12-01' AND '2018-01-01'
          ) as previous;")

          result_builder.value(rows[0]['value'], rows[0]['previous'])
      end
    end
  end
end
```
{% endtab %}
{% endtabs %}

## Step 3: Retrieve the URL of the generated chart

{{#nodejs}}
When you create a new API chart, you will need to retrieve the URL of the generated chart.

It is printed to the console when the agent starts (in development mode).

```console
sandro@forestadmin $ yarn start
yarn run v1.22.19
info: Successfully mounted on Standalone server (http://0.0.0.0:3351)
info: Schema was updated, sending new version
info: Chart 'appointments' was mounted at '/forest/_charts/appointments'
```

{{/nodejs}}
{{#ruby}}
The url of your newly created api chart is `/forest/_charts/$chartName`. The `$chartName` is the name given to the {{#ruby}}`add_chart`{{/ruby}} method.
For the previous example the url should be `/forest/_charts/appointments`
{{/ruby}}

## Step 4: Change the old chart configuration

{% hint style="info" %}
If you are following this guide step by step, you are probably testing your agent on a temporary project.

You may want to postpone this step until you have replaced your old agent with the new one.
{% endhint %}

Instead of `Query`, select `Smart` in the data source selector, and enter the path that was printed to the console in the previous step.
