![Minimal example in action](../../../assets/customdatasource-minimal.png)

The idea behind data sources using the "replication" strategy is to maintain a copy of the data from the target API in an internal cache that is under the control of your agent.

| Pros             | Why?                                                                                              |
| ---------------- | ------------------------------------------------------------------------------------------------- |
| Easy             | No need to implement any query translation logic.                                                 |
| Performant       | No need to make network calls to the target API synchronously.                                    |
| Feature-complete | Charts, filtering, search is provided out of the box.                                             |
| Flexible         | You can implement any logic you want to fetch the data from the target API.                       |
| Robust           | Any bad state can be recovered from by reconstructing the replica from scratch in the background. |

# Minimal example

This example shows how to create a custom data source using the "replication" strategy on [{JSON} Placeholder](https://jsonplaceholder.typicode.com/), a free API that returns read-only fake data.

In this trivial example, records will be fetched from the target API when the agent first starts, never to be updated again.

## Implementation

The most stripped-down implementation of a data source using the "replication" strategy is simply providing a function that returns a list of entries to be imported.

```javascript
const { createReplicaDataSource } = require('@forestadmin/datasource-replica');
const axios = require('axios');

// Create the data source
const myCustomDataSource = createReplicaDataSource({
  pullDumpHandler: async () => {
    const url = 'https://jsonplaceholder.typicode.com';
    const collections = ['posts', 'comments', 'albums', 'photos', 'users'];
    const entries = [];

    for (const collection of collections) {
      const response = await axios.get(`${url}/${collection}`);
      entries.push(...response.data.map(record => ({ collection, record })));
    }

    return { more: false, entries };
  },
});

// Register the data source with the agent
agent.addDatasource(myCustomDataSource);
```

## Limitations

This minimal example is a good starting point to understand the selling points of the replication strategy, however, strong limitations remain and you may need to implement more logic to tackle them.

| Limitation                                                             | Mitigation                                                                |
| ---------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| Needs to dump all records at each startup                              | Use a [persistent cache](./persistent-cache.md)                           |
| Empty collections cannot be imported and foreign keys are not detected | Provide a [schema](./schema.md)                                           |
| The data is never updated                                              | Implement [updates](./updates/README.md)                                  |
| The data is readonly                                                   | Implement [write handlers](./write.md)                                    |
| Records from API may contain nested fields and arrays                  | Use the [record flattener](./schema.md#handling-arrays-and-nested-fields) |
