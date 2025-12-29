A well-known adage says, "a picture is worth a thousand words": charts help people better understand and remember information.

Forest Admin Dashboards and Analytics are meant to answer that need.

![Dashboard example](../../assets/chart-dashboard-on-live-demo.png)

# From your admin panel

Charts can be configured from the interface, without the need to write any code.

This is documented in the [User Guide](https://docs.forestadmin.com/user-guide/dashboards/charts)

# From your agent

Sometimes, chart data are closely tied to your business. Forest Admin allows you to code how the data fueling any given chart is computed.

This is done in 3 steps:

- Implement the chart data retrieval using the Agent API,
- Either:
  - create a new Chart on a Dashboard, and choose "API" as the data source,
  - or create a new Chart in the "Analytics" tab of a Collection,
- Enter the URL of the Chart you just implemented (`/forest/_charts/<chartName>` or `/forest/_charts/<collectionName>/<chartName>`).

![](../../assets/chart-api.png)

Note that, when defining a chart from your agent:

- The type of chart defined in your agent must match your selection when adding it to a dashboard or record.
- The name of the chart must be URL-safe.

## Relation to smart charts

[Smart Charts](../../frontend-customization/smart-charts/README.md) allow the implementation of any charts types that are not supported natively (density maps, cohorts, ...).

Coding a Chart handler from your agent, on the other hand, gives you the full freedom in how the data powering a native chart is computed.
