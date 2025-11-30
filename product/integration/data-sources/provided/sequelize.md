The Sequelize data source allows importing collections from a Sequelize instance.

To make everything work as expected, you need to install the package `@forestadmin/datasource-sequelize`.

Note that:

- Sequelize scopes will be mapped to Forest Admin segments
- Sequelize hooks will run
- Sequelize association, field aliasing, relationships, and validation will be respected

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSequelizeDataSource } = require('@forestadmin/datasource-sequelize');
const { Sequelize, Model, DataTypes } = require('@sequelize/core');

// Create a Sequelize instance
const sequelize = new Sequelize('sqlite::memory:');

class User extends Model {}
User.init(
  {
    username: DataTypes.STRING,
    birthday: DataTypes.DATE,
  },
  { sequelize, modelName: 'user' },
);

// Create agent and import collections from sequelize
const agent = createAgent(options).addDataSource(
  createSequelizeDataSource(sequelize),
);
```

## Enable Live Query

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a Live Query component, it should have a clear meaning for your Forest users.

```javascript
const agent = createAgent(options).addDataSource(
  createSequelizeDataSource(sequelize, {
    liveQueryConnections: 'main_database',
  }),
);
```
