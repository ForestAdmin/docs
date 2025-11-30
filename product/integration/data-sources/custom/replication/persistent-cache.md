The cache that is used under the hood is a SQL database. By default, an in-memory SQLite database will be used.

This is fine for many use cases but comes with a strong limitation: the data is not persisted between restarts.

This causes two issues:

- Longer start time, as the agent will need to fetch all the data from the target API at each startup.
- High memory usage, as the agent will need to keep all the data in memory.

{% hint style="info" %}
Depending on which API you are targeting, it may be absolutely fine to use an in-memory cache. For example, it is unlikely that your payment processing API contains more than a few dozen thousand records which would result in minimal memory overhead.

Larger APIs, such as a CRM or a database, may contain millions of records: you may want to use a persistent cache in this case.
{% endhint %}

# Cache initialization routine

```mermaid
flowchart LR
  connect([Connect])
  dropStructure([Reset Cache])
  createStructure([Create tables/indexes\n+ Import Data])
  catchupData([Catchup data])

  connect -- outdated structure --> dropStructure --> createStructure
  connect -- empty --> createStructure
  connect -- structure match --> catchupData

  createStructure --> catchupData
```

To ease development, Forest Admin will automatically detect when the schema of the tables in the caching database does not match the schema of the target API.

When this happens, it will drop all the tables and indexes, recreate them, and re-import all the data from the target API

# Configuration

Two options are available to use a persistent cache:

- `cacheInto`: a connection string, or a [configuration object](../../provided/sql.md#advanced-configuration) for the `@forestadmin/datasource-sql` connector.
- `cacheNamespace`: a string that will be used as a prefix for all the tables created by the tool. This is useful if you either want to share the same database with other tools, or if you want to use the same database for multiple replicas.

{% hint style="warning" %}
As of today, when sharing the same cache between multiple instances of an agent, no locking mechanism is in place to prevent concurrent writes.

This means that if two agents are running at the same time, with the same configuration, they won't collaborate to keep the cache up-to-date while avoiding double work.
{% endhint %}

# Examples

## Example 1: Using an SQLite file

The simplest way to use a persistent cache is to use an SQLite file.

Those have the advantage of being trivial to set up, at the cost of being limited to a single process.

```javascript
const { createReplicaDataSource } = require('@forestadmin/datasource-replica');

const myCustomDataSource = createReplicaDataSource({
  // Store the replica in a SQLite file.
  // It will be created if it does not exist and survive restarts.
  cacheInto: 'sqlite:/tmp/my-cache.db',

  // Stub of a pullDumpHandler implementation.
  pullDumpHandler: async () => {
    return { more: false, entries: [] };
  },
});
```

## Example 2: Using a Postgres database

You may also want to use a SaSS database to store the replica.

In this example, we use a Postgres database hosted on [Neon.tech](https://neon.tech), but the same can be achieved with any cloud and database vendor.

```javascript
const { createReplicaDataSource } = require('@forestadmin/datasource-replica');

const myCustomDataSource = createReplicaDataSource({
  // Store the replica in a postgresql database hosted on Neon.tech.
  cacheInto: {
    uri: 'postgres://xxxx:xxxx@aa-aaaa-aaaaa-000000.us-east-1.aws.neon.tech/neondb',
    sslMode: 'verify',
  },

  // Use a custom namespace for the tables created by the tool so that
  // this database can be used for other replicas.
  cacheNamespace: 'my-custom-data-source',

  // Stub of a pullDumpHandler implementation.
  pullDumpHandler: async () => {
    return { more: false, entries: [] };
  },
});
```
