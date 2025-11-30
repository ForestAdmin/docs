The ActiveRecord data source allows importing collections from all models class that extends the abstract class `ActiveRecord::Base`.

To make everything work as expected, you need to install the gem `forest_admin_datasource_active_record`.

Note that:

- ActiveRecord relationships will be respected

```ruby
def self.setup!
  ForestAdminDatasourceActiveRecord::Datasource.new(
    {
      'adapter' => ENV['DB_ADAPTER'],
      'host' => ENV['DB_HOST'],
      'username' => ENV['DB_USERNAME'],
      'password' => ENV['DB_PASSWORD'],
      'database' => ENV['DB_DATABASE'],
    }
  )
end
```

## Enable support of live queries

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a Live Query component, it should have a clear meaning for your Forest users.

- If a string is provided (e.g., `main_database`), ForestAdmin will bind it to the `primary` database as defined in your Ruby on Rails configuration.
- If a hash is provided, it should be a mapping of `{"primary" => "main_database"}`.

```ruby
def self.setup!
  @create_agent = ForestAdminAgent::Builder::AgentFactory.instance.add_datasource(
    ForestAdminDatasourceActiveRecord::Datasource.new(
      {
        'adapter' => ENV['DB_ADAPTER'],
        'host' => ENV['DB_HOST'],
        'username' => ENV['DB_USERNAME'],
        'password' => ENV['DB_PASSWORD'],
        'database' => ENV['DB_DATABASE'],
      },
      live_query_connections: 'main_database'
    )
  )
  customize
  @create_agent.build
end
```

### Multi databases

If you are working with [multiple databases](https://guides.rubyonrails.org/active_record_multiple_databases.html), here is an example of how you can configure live queries to target specific connections for each database :

```ruby
ForestAdminDatasourceActiveRecord::Datasource.new(
  {
    'adapter' => ENV['DB_ADAPTER'],
    'host' => ENV['DB_HOST'],
    'username' => ENV['DB_USERNAME'],
    'password' => ENV['DB_PASSWORD'],
    'database' => ENV['DB_DATABASE'],
  },
  live_query_connections: {
    'main_database' => 'primary',
    'replica_database' => 'primary_replica'
  }
)
```

Understanding the configuration

- keys (e.g., 'main_database', 'replica_database'): These are the display names that will appear in the Forest Admin UI when selecting a database for live queries.
- values (e.g, 'primary', 'primary_replica'): These correspond to the connection names defined in your Rails application `config/database.yml`file.
  For example if your `database.yml` contains:

```
production:
  primary:
    <<: *default
    database: my_main_db
  primary_replica:
    <<: *default
    database: my_replica_db
    replica: true
```

Then users will see "main_database" and "replica_database" as options in the Forest Admin interface, which will connect to primary and primary_replica respectively.
