The SQL data source allows importing tables and views from SQL databases.

Each table and view in the database will be mapped to a collection in Forest Admin.

The views will be read-only, while the tables will be read-write.

# Installation

To make everything work as expected, you need to:

- install the package `@forestadmin/datasource-sql`.
- install the native drivers for the vendors you are aiming to connect to.

| Vendor               | Install extra package |
| -------------------- | --------------------- |
| MariaDB              | `mariadb`             |
| Microsoft SQL Server | `tedious`             |
| MySQL                | `mysql2`              |
| Oracle               | `oracledb`            |
| PostgreSQL           | `pg` + `pg-hstore`    |
| SQLite               | `sqlite3`             |

# Usage

Get started by using the following code sample in your agent.

You can then tune the configuration to your needs.

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSqlDataSource } = require('@forestadmin/datasource-sql');

// Create agent and import collections from SQL database
const agent = createAgent(options).addDataSource(
  createSqlDataSource({
    uri: 'postgres://user:pass@localhost:5432/myDatabase',
    sslMode: 'preferred',
  }),
);
```

# Automatic schema discovery

By default, when using this data source, there is no need to provide a schema, as the agent will extract the list of tables, columns, and relations when the agent restarts.

However, to be able to work, the credentials which are used must be able to access the `information_schema` or the database. Using a role that has ownership of the tables you would like to use in Forest Admin is highly recommended.

{% hint style="info" %}
The introspection supports tables and views from Microsoft SQL Server, PostgreSQL, MySQL, and MariaDB.

If you wish to use a different database you will need to provide the schema yourself.
{% endhint %}

## Example: Caching the introspection

If your database schema does not change or if you want to skip the introspection step when running in production, you may want to cache it.

To do so, you can call the introspection function yourself and pass the result to the data source constructor.

Note that you will need to delete the cache file if you change the database structure.

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSqlDataSource, introspect } = require('@forestadmin/datasource-sql');

// Options to connect to the db (see above).
const connectionOptions = { uri: 'postgres://user:pass@localhost:5432/myDatabase' };

let introspection;
try {
  // The introspection is JSON serializable. You can store it in a file.
  // Read it from the file if it exists.
  introspection = JSON.parse(fs.readFileSync('./my-database-introspection.json'));
} catch (e) {
  if (e.code === 'ENOENT') {
    // The file does not exist, we need to introspect the database.
    introspection = await introspect(connectionOptions);
    fs.writeFileSync(
      './my-database-introspection.json',
      JSON.stringify(introspection),
    );
  } else {
    throw e;
  }
}

const agent = createAgent(options).addDataSource(
  createSqlDataSource(connectionOptions, { introspection }),
);
```

## Example: Using a different account for the schema introspection and the agent

You may be running Forest Admin with database credentials that have restricted access to the database. In this case, you will need to provide a different set of credentials to the data source to be able to introspect the database.

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSqlDataSource, introspect } = require('@forestadmin/datasource-sql');

// Options to connect to the db (see above).
const startupConnectionOptions = {
  uri: 'postgres://user-with-privileges@localhost:5432/myDatabase',
};
const runtimeConnectionOptions = {
  uri: 'postgres://normal-user@localhost:5432/myDatabase',
};
const introspection = await introspect(startupConnectionOptions);

const agent = createAgent(options).addDataSource(
  createSqlDataSource(runtimeConnectionOptions, { introspection }),
);
```

# Viewing soft deleted records

In database design, it's common to include columns in your tables to track timestamps for record creation, updates, and deletions. These columns help manage the lifecycle of your data. By default, records that are soft-deleted (marked as deleted but not actually removed from the database) are not displayed in queries. However, you can override this default behavior by configuring an option when setting up your SQL data source. This allows you to include soft-deleted records in your results if needed.

## Example: Display soft deleted records on one or many collections

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSqlDataSource } = require('@forestadmin/datasource-sql');

const agent = createAgent(options).addDataSource(
  createSqlDataSource('postgres://normal-user@localhost:5432/myDatabase', {
    displaySoftDeleted: ['user', 'project'],
  }),
);
```

## Example display soft deleted reccords on all collections

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSqlDataSource } = require('@forestadmin/datasource-sql');

const agent = createAgent(options).addDataSource(
  createSqlDataSource('postgres://normal-user@localhost:5432/myDatabase', {
    displaySoftDeleted: true,
  }),
);
```

# Advanced configuration

Configuration can be as simple as passing a URI to the data source constructor, but more options are available:

```javascript
const { createAgent } = require('@forestadmin/agent');
const { createSqlDataSource } = require('@forestadmin/datasource-sql');

// Create agent and import collections from SQL database
const agent = createAgent(options).addDataSource(
  createSqlDataSource({
    /** URI where the database can be reached */
    uri: 'postgres://user:pass@localhost:5432/myDatabase',

    /**
     * SSL mode to use when connecting to the database
     * Possible values: 'preferred', 'verify', 'required', 'disabled', 'manual'
     *
     * - 'preferred' will use SSL if the server supports it, but will fall back to an
     *   unencrypted connection otherwise.
     * - 'verify' will use SSL and verify the server certificate.
     * - 'required' will use SSL but won't verify the certificate.
     * - 'disabled' will use an unencrypted connection.
     * - 'manual' will rely on settings from dialectOptions (see below).
     */
    sslMode: 'preferred',

    /** The dialect of the database you are connecting to */
    dialect: 'postgres',

    /** The name of the database */
    database: 'myDatabase',

    /** The username which is used to authenticate against the database */
    username: 'user',

    /** The password which is used to authenticate against the database */
    password: 'pass',

    /** The host of the relational database */
    host: 'localhost',

    /** The port of the relational database */
    port: 5432,

    /** Use the provided schema instead of the default ("public") */
    schema: 'public',

    /** Only for PostgreSQL: A flag that defines if `pg-native` shall be used */
    native: false,

    /** Only for SQLite: path where the database file is located */
    storage: '/tmp/my-sqlite-database.db',

    /** Connection pool options */
    pool: {
      /** Maximum number of connection in pool */
      max: 5,

      /** Minimum number of connection in pool */
      min: 0,

      /** Maximum time that pool will try to get connections before throwing error */
      acquire: 30000, // milliseconds

      /** Maximum time, that a connection can be idle before being released */
      idle: 10000, // milliseconds
    },

    /**
     * Use read / write replication.
     * @see https://sequelize.org/docs/v6/other-topics/read-replication/
     */
    replication: false,

    /**
     * Use a proxy socks5 to connect to the database.
     * You must have a running socks5 proxy server.
     * ProxySocks can be used with the ssh tunnel.
     */
    proxySocks: {
      userId: 'aUserOptional',
      password: 'aPasswordOptional',
      host: 'myhost.com',
      port: 1083,
    },

    /**
     * Use a ssh tunnel to connect to the database.
     * You must have a running ssh server.
     * Ssh tunnel can be used with the proxySocks.
     */
    ssh: {
      host: 'myhost.com',
      port: 22,
      username: 'myUserName',
      privateKey: Buffer.from('myPrivateKey'),
    },

    /**
     * Set the connection timeout in milliseconds.
     * It is useful to avoid long connection time when the database is not reachable.
     */
    connectionTimeoutInMs: 5000, // milliseconds
  }),
);
```

Note that under the hood, the data source uses the [Sequelize](https://sequelize.org/) ORM to connect to the database. So, you can pass any option that is supported by Sequelize.

## Enable support of live queries

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a Live Query component, it should have a clear meaning for your Forest users.

```javascript
const agent = createAgent(options).addDataSource(
  createSqlDataSource(process.env.DATABASE_URL, {
    liveQueryConnections: 'main_database',
  }),
);
```
