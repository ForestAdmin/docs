The Doctrine data source allows importing collections from a Doctrine instance.

To make everything work as expected, you need to install the package `forestadmin/php-datasource-doctrine`.

Note that:

- Doctrine relationships will be respected

```php
use ForestAdmin\AgentPHP\Agent\Utils\Env;
use ForestAdmin\AgentPHP\DatasourceDoctrine\DoctrineDatasource;
use ForestAdmin\SymfonyForestAdmin\Service\ForestAgent;

return static function (ForestAgent $forestAgent) {

    $forestAgent->addDatasource(
        new DoctrineDatasource(
            $forestAgent->getEntityManager(),
            ['url' => Env::get('DATABASE_URL')]
        )
    )
        ->build();
};
```

## Enable support of live queries

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a LiveQuery component, it should have a clear meaning for your Forest users.
