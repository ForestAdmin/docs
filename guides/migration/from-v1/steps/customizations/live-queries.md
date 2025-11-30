{% hint style="info" %}
Due to popular demand, Live Query Charts and Live Query Segments are reintroduced in ForestAdmin agents since version {{#python}}`agent-python>=1.19.1`{{/python}}{{#nodejs}}`@forestadmin/agent>=1.56.0`{{/nodejs}}{{#ruby}}`agent-ruby>=v1.0.0-beta.85`{{/ruby}}{{#php}}`agent-php>=1.20.0`{{/php}}
{% endhint %}

ForestAdmin still recommends using [smart charts](./api-charts.md) and [smart segments](./smart-segments.md) over LiveQueries.

As new agents support multiple datasources, a new property is required when defining a LiveQuery component in the UI. This property will allow the agent to reconcile the query with the proper execution context.

## Enabling the feature on the datasources

To make use of LiveQuery feature, you will need to enable the nativeQuery on your datasource.
This ensures a coherent naming scheme for your native query connections as well as only enabling LiveQuery on a restricted set of datasources.
Refer to the following documentation to enable nativeQuery on your desired datasources.

{{#python}}

- [Django Datasource](../../../../datasources/provided/django.md#enable-support-of-live-queries)
  <!-- - [SQLAlchemy Datasource](../../../../datasources/provided/sqlalchemy.md#enable-support-of-live-queries) There is no migration from SQLAlchemy v1, it doesn't exists-->
  {{/python}}{{#nodejs}}
- [Sequelize Datasource](../../../../datasources/provided/sequelize.md#enable-support-of-live-queries)
- [SQL Datasource](../../../../datasources/provided/sql.md#enable-support-of-live-queries)
  {{/nodejs}}{{#ruby}}
- [Active Record Datasource](../../../../datasources/provided/active-record.md#enable-support-of-live-queries)
  {{/ruby}}{{#php}}
- [Doctrine Datasource](../../../../datasources/provided/doctrine.md#enable-support-of-live-queries)
- [Eloquent Datasource](../../../../datasources/provided/eloquent.md#enable-support-of-live-queries)
  {{/php}}

## Migrate your UI components

You will have to configure the native query connection on each Live Query component defined in `collection segments`, `analytics chart`, `dashboard charts` and `workspace charts`.

![Select connection on chart](../../../../assets/live_query_connection_on_chart.png)
_Select connection on chart_

![Select connection on segments](../../../../assets/live_query_connection_on_segments.png)
_Select connection on segments_

## Changes in the SQL requests

### Segments

Previously the segment query must return a list of fields named `id`.Now the field must be named by the `primary key` field.

### Analytics charts

Previously the analytics charts could use `?` in the request, which will be replaced by the value of the current record id. Now you must use `{{recordId}}` instead.
