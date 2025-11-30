The simplest way to update the data is to fetch all the records from the target API at regular intervals.

This approach is trivial to implement and should work with any API, as the only requirement is to be able to fetch all the records from the target API.

Its main drawback is that it is not very efficient: if you have a large number of records, you will need to fetch all of them at each interval, even if only a few of them changed.

# Choosing when to rebuild your replica

This can be achieved with two options:

- `pullDumpOnRestart`: if set to `true`, the data will be fetched each time the agent starts. This is always true if you are using the default in-memory cache.
- `pullDumpOnSchedule`: if set to a cron-like schedule, the data will be fetched at the given interval

```javascript
const myCustomDataSource = createReplicaDataSource({
  // Update the data each time the agent starts.
  pullDumpOnRestart: true,

  // Update the data each day at midnight and 6.30pm.
  pullDumpOnSchedule: ['0 0 0 * * *', '0 30 18 * * *'],

  // Handler to fetch all records from the target API.
  pullDumpHandler: async () => {
    // Implement handler here
  },
});
```

<details>
<summary>Schedule syntax examples</summary>

Under the hood, we are using the [croner](https://www.npmjs.com/package/croner#pattern) NPM package to handle schedules.

You can use the following syntax to define a schedule:

```
┌─────────────────── (optional) second (0 - 59)
│    ┌────────────── minute (0 - 59)
│    │ ┌──────────── hour (0 - 23)
│    │ │ ┌────────── day of month (1 - 31)
│    │ │ │ ┌──────── month (1 - 12, JAN-DEC)
│    │ │ │ │ ┌────── day of week (0 - 6, SUN-Mon)
│    │ │ │ │ │       (0 to 6 are Sunday to Saturday; 7 is Sunday again)
│    │ │ │ │ │
*    * * * * *

Examples
*    * * * * *   every second
*/10 * * * * *   every 10 seconds
0    * * * * *   every minute
0    0 9 * * 1   on mondays at 9am
0    0 0 L * *   last day of each month at midnight
```

</details>

# Programming your handler

{% hint style="info" %}
If the `pullDumpHandler` method takes a long time to execute, the old data will still be served to the users until the new data is ready.
{% endhint %}

This handler is the simplest one to implement: it is simply expected to return a list of entries to be imported, each entry containing the collection name and the record to import.

As the handler may return a large amount of data, you are allowed to paginate the results.

<details>
<summary>Structure of the `request` object</summary>

```javascript
{
  // It is up to you to decide what to store in it so that you can detect changes.
  // It's value is:
  // - The value you provided on on pullDumpHandler on the previous call
  // - null otherwise
  previousDumpState: { /* Any JSON serializable object */ },

  // Interface to read from the cache
  cache: { },

  // List of reasons why the handler was called
  reasons: [
    { name: 'startup' },
    { name: 'schedule' },
  ]
}
```

</details>
<details>
<summary>Structure of the `response` object</summary>

```javascript
{
  // If all records could not be fetched in a single call, you can set the `more`
  // flag to true.
  // This will cause the handler to be called again immediately.
  more: true,

  // NOTE: This field is only relevant if more is set to true.
  // Value that should be used as `previousDumpState` field on the next call to the
  // handler.
  nextDumpState: { /* Any JSON serializable object */ },

  // NOTE: This field is only relevant if more is set to false.
  // Value that should be persisted in the `previousDeltaState` field on the first
  // call to the pullDeltaHandler (see the "Change polling" section).
  nextDeltaState: { /* Any JSON serializable object */ },

  // List of records to import
  entries: [
    { collection: 'posts', record: { id: 134, title: '...' } },
    { collection: 'comments', record: { id: 15554, content: '...' } },
    { collection: 'comments', record: { id: 15555, content: '...' } },
  ],
};
```

</details>

```javascript
const myCustomDataSource = createReplicaDataSource({
  // Update the data each time the agent starts.
  pullDumpOnRestart: true,

  // Update the data each day at midnight and 6.30pm.
  pullDumpOnSchedule: ['0 0 0 * * *', '0 30 18 * * *'],

  // Handler to fetch all records from the target API.
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
```
