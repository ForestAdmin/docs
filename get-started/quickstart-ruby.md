{{#ruby}}

## Into your gemfile file, add the following lines: 

```bash
gem "forest_admin_rails", ">= 1.0.0"
gem "forest_admin_agent", ">= 1.0.0"
gem "forest_admin_datasource_toolkit", ">= 1.0.0"
gem "forest_admin_datasource_active_record", ">= 1.0.0"
gem "forest_admin_datasource_customizer", ">= 1.0.0"
```

and run `bundle install`

Then run the following commands in your terminal

```bash
rails g forest_admin_rails:install THE-KEY-PROVIDED-BY-FORESTADMIN
```

Once the installation is finish, you have a new configuration file (`lib/forest_admin_rails/create_agent.rb`) and second file (`config/initializers/forest_admin_rails.rb`) to handle your environment variables.

{% tabs %} {% tab title="lib/forest_admin_rails/create_agent.rb" %}

```ruby
module ForestAdminRails
  class CreateAgent
    def self.setup!
      database_configuration = Rails.configuration.database_configuration
      datasource = ForestAdminDatasourceActiveRecord::Datasource.new(database_configuration[Rails.env])

      @create_agent = ForestAdminAgent::Builder::AgentFactory.instance.add_datasource(datasource)
      customize
      @create_agent.build
    end

    def self.customize
      # @create_agent.add_datasource....
    end
  end
end


```

{% endtab %} {% tab title="config/initializers/forest_admin_rails.rb" %}

```ruby
ForestAdminRails.configure do |config|
  config.auth_secret = 'YOUR_AUTH_SECRET'
  config.env_secret = 'YOUR_ENV_SECRET'
end

```

{% endtab %} {% endtabs %}

Running

```bash
rails server
```

should be enough to be redirected to the "rate-install" page.

{{/ruby}}
