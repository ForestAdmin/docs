{{#nodejs}}

## Step 1: Run the new agent in "parallel" with the old one

The first step of every migration should be the creation of and the installation of a new environment.
You can first, try our new experience by using a development environment, when you are satisfied create a temporary new remote environment, that will become your new production later.

Follow the [dedicated guide](./run-parallel.md) to learn more about running both agents.

## Step 2: Connect the new agent to your database

Some thoughts should be given to the way you connect your new agent to your databases.

Follow the [dedicated guide](./datasource.md) to learn more about the differences between the two agents and how to connect your new agent to your databases.

## Step 3: Port your code to the new API

A translation guide for most features is available in the `Code transformations` section.

## Step 4: Ensure compatibility between agents

Making an agent which works is not enough: you also need to make sure that it generates a schema where the naming of most entities is the same as the old one.

More information is in the [dedicated guide](./compare.md)

## Step 5: Replace the old agent with the new one

Once all those steps are done, you can go ahead and replace the old agent with the new one in your development environment!

More information in this last [dedicated guide](./replace.md)
{{/nodejs}}


{{#ruby}}

## Step 1: Remove the previous agent

### Settings

{% hint style="warning" %}
Please keep your `FOREST_ENV_SECRET` before deleting the old agent. You will need it to configure the new one.
You can find it in you `config/secrets.yml` file.
{% endhint %}

```bash
# uninstall the old agent
bundle remove forest_liana
# remove the old configuration files
rm -f config/initializers/forest_liana.rb
rm -f config/secrets.yml
```

### Route configuration

Remove the 'ForestLiana' route from your `config/routes.rb` file.

```ruby
# remove this line
mount ForestLiana::Engine => '/forest'
```

### Cache

The previous agent required cache to be activated in development mode. This is no longer necessary with the new agent. You can disable it with the following command:

```bash
rails dev:cache
```

## Step 2: Install the new agent

add the following line to your Gemfile:

```ruby
gem "forest_admin_rails", "~> 1.0.0"
gem "forest_admin_agent", "~> 1.0.0"
gem "forest_admin_datasource_toolkit", "~> 1.0.0"
gem "forest_admin_datasource_active_record", "~> 1.0.0"
gem "forest_admin_datasource_customizer", "~> 1.0.0"
```

Then run:

```bash
rails g forest_admin_rails:install FOREST_ENV_SECRET
```

## Step 3: Customization

Previously, to customize a SmartAction, you set it for each model `lib/forest_liana/collections/` and `app/controllers/forest/` directories. Now, you set it in the `lib/forest_admin_rails/create_agent.rb` file.
You will find more information in the [dedicated section](../../../agent-customization/README.md) on moving your previous SmartAction customizations to the new agent.
{{/ruby}}
