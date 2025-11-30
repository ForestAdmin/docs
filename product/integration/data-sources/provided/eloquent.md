The Eloquent data source allows importing collections from all models class that extends the abstract class `Illuminate\Database\Eloquent\Model`.

To make everything work as expected, you need to install the package `forestadmin/php-datasource-eloquent`.

Note that:

- Eloquent relationships will be respected

```php
use ForestAdmin\AgentPHP\Agent\Builder\AgentFactory;
use ForestAdmin\AgentPHP\DatasourceEloquent\EloquentDatasource;

return static function () {
    $forestAgent = app()->make(AgentFactory::class);
    $forestAgent->addDatasource(
        new EloquentDatasource(
            [
                'driver' => env('DB_CONNECTION'),
                'host'   => env('DB_HOST'),
                'port'   => env('DB_PORT'),
                'database' => env('DB_DATABASE'),
                'username' => env('DB_USERNAME'),
                'password' => env('DB_PASSWORD'),
            ]
        ),
    );
};
```

## Enable support of live queries

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a LiveQuery component, it should have a clear meaning for your Forest users.

The live_query_connections parameter determines which database will be used for live queries.

- If a string is provided (e.g., `mainDatabase`), ForestAdmin will bind it to the `config('database.default')` database as defined in your Laravel database configuration.
- If an array is provided, it should be an associative array, like ['EloquentDatasource' => 'pgsql'].

```php
$forestAgent = app()->make(AgentFactory::class);
$forestAgent->addDatasource(
    new EloquentDatasource(
        databaseConfig: [
            'driver' => env('DB_CONNECTION'),
            'host'   => env('DB_HOST'),
            'port'   => env('DB_PORT'),
            'database' => env('DB_DATABASE'),
            'username' => env('DB_USERNAME'),
            'password' => env('DB_PASSWORD'),
        ],
        liveQueryConnections: 'mainDatabase'
    ),
);
```

### Multi databases

If you are working with multiple databases here is an example of how you can configure live queries to target specific connections for each database :

```php
$forestAgent = app()->make(AgentFactory::class);
$forestAgent->addDatasource(
    new EloquentDatasource(
        databaseConfig: [
            'driver' => env('DB_CONNECTION'),
            'host'   => env('DB_HOST'),
            'port'   => env('DB_PORT'),
            'database' => env('DB_DATABASE'),
            'username' => env('DB_USERNAME'),
            'password' => env('DB_PASSWORD'),
        ],
        liveQueryConnections: [
            'mainDatabase' => 'pgsql',
            'otherEloquentDatasource' => 'pgsql2',
        ]
    ),
);
```
