You may want to migrate from a legacy agent to this new agent. This guide will help you do so.

The new agent is built with a different architecture and breaks API retro compatibility with the legacy agents in many ways. However, as this new agent provides a more efficient API to let builders focus on their operational needs, we encourage you to migrate to speed up your future admin panel evolutions.

# Am I using a legacy agent?

{{#nodejs}}
You are using a legacy agent if you are using either the [forest-express-sequelize](https://github.com/ForestAdmin/forest-express-sequelize) or the [forest-express-mongoose](https://github.com/ForestAdmin/forest-express-mongoose) packages.
{{/nodejs}}

{{#ruby}}
You are using a legacy agent if you are using the [forest-rails](https://github.com/ForestAdmin/forest-rails) package.
{{/ruby}}


# When to migrate?

{{#nodejs,ruby}}
Legacy agents are still supported and will continue to be for a while.

No end-of-life date has been set yet for customers using the latest major version of the legacy agents.
{{/nodejs,ruby}}
{{#php,python}}
Legacy agents will reach their end-of-support in July 2024 and end-of-life in January 2025.
{{/php,python}}

To give more visibility to our developers' community, about agent usability and support in the future, you will find [on this page](https://docs.forestadmin.com/documentation/how-tos/releases-support) the important lifecycle dates per agent stack and versions.

# Missing features

The new agent brings a lot of new features, but a small subset of what was available in the legacy agents is not yet there.

## Route overrides

[Route overrides](https://docs.forestadmin.com/documentation/reference-guide/routes/override-a-route) allowed customizing the behavior of the routes exposed by the agent.

Because our new agent API is higher-level, the protocol used to communicate between the agent and the application can no longer be manipulated.

All use cases that we have seen so far can be implemented using the new [agent customization](../../agent-customization) system, but because the former system was lower-level, we cannot guarantee it.

A specific [migration guide](./steps/customizations/route-overrides.md) is available for this feature.

### Charts

![Live Query Chart configuration screen](../../assets/migration-chart-sql.png)

[Live Query Charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql) allowed the creation of charts from SQL queries from the UI.

{{#nodejs}}
You will need to follow the specific [migration guide](./steps/customizations/smart-charts.md) to convert `Live query charts` into `API charts`.
{{/nodejs}}

### Segments

![SQL Query Segment configuration screen](../../assets/migration-segment-sql.png)

[SQL Query Segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments) allowed the creation of segments from SQL queries from the UI.

{{#nodejs}}
You will need to convert `SQL Query segments` into [`Smart Segments`](./steps/customizations/smart-segments.md).
{{/nodejs}}
