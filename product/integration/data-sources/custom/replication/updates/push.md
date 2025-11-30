Some APIs expose the capability of following changes.

This can take the form of webhooks, WebSockets, long polling, or any other mechanism.

When this is the case, you can use the "push" strategy to keep the replica up-to-date.

# Programming your handler

Unlike with the "pull" strategy, you are responsible for setting up the subscriptions to the target API.

When the agent starts, it will call your handler to set up the subscriptions.

Then, each time a change is received, you are responsible for sending it to the agent so that the replica can be updated.

<details>
<summary>Structure of the `request` object</summary>

```javascript
{
  // Unlike with the pull strategy, you can fetch the delta state at any time.
  // This is useful if you are mixing the push strategy with the pull strategy.
  getPreviousDeltaState: () => Promise({ /* Any JSON serializable object */ }),

  // Interface to read from the cache
  cache: { },
}
```

</details>
<details>
<summary>Structure of the `onChange` payload</summary>

```javascript
{
  // Unlike with the pull strategy, this field is optional
  nextDeltaState: { /* Any JSON serializable object */ },

  // List of records that were updated or created since the last call to the
  // handler.
  newOrUpdatedEntries: [
    { collection: 'posts', record: { id: 134, title: '...' } },
    { collection: 'comments', record: { id: 15554, content: '...' } },
    { collection: 'comments', record: { id: 15555, content: '...' } },
  ],

  // List of records that were deleted since the last call to the handler.
  // This list is used to remove the records from the cache, so providing the
  // full record is not necessary.
  deletedEntries: [
    { collection: 'posts', record: { id: 34 } },
    { collection: 'comments', record: { id: 554 } },
  ]
};
```

</details>

# Examples

## Example 1: With an API that provides a change feed

Some APIs conveniently provide a change feed that you can subscribe to using a WebSocket or a long polling mechanism.

This is the case for example with [CouchDB](https://docs.couchdb.org/en/stable/api/database/changes.html), among others.

In this example, we will use the [nano](https://github.com/apache/couchdb-nano) library to subscribe to the changes feed and replicate all changes to our replica.

```javascript
const { createReplicaDataSource } = require('@forestadmin/datasource-replica');
const nano = require('nano');

const myCustomDataSource = createReplicaDataSource({
  // This handler will be called when the agent starts so that you can set up
  // your subscriptions to the target API.
  pushDeltaHandler: async (request, onChanges) => {
    // Ask CouchDB to stream all the changes since the last time we asked.
    const stream = nano.db.changesAsStream('books', {
      // Include the full document in the change
      include_docs: true,

      // Start from the last time we asked for changes (or from the beginning
      // if this is the first time)
      since: await request.getPreviousDeltaState(),
    });

    // Each time a change is received ...
    stream.on('data', change => {
      // ... we send it to the agent so that the replica can be updated.
      onChange({
        // The next time we ask for changes, we want to start from this point.
        // This is used to recover when the agent restarts.
        nextDeltaState: change.seq,

        // The list of changes that should be applied to the replica.
        newOrUpdatedEntries: !change.deleted
          ? [{ collection: 'books', record: { _id: change.id, ...change.doc } }]
          : [],

        deletedEntries: change.deleted
          ? [{ collection: 'books', record: { _id: change.id } }]
          : [],
      });
    });
  },
});
```

## Example 2: With an API that provides webhooks

Dealing with webhooks is a bit more complex than with a change feed, as you need to expose an HTTP server to receive the webhooks.

In this example, we will use [Express](https://expressjs.com/) to expose an HTTP server on a different port than the agent, and use it to receive the webhooks.

You may also want to share an HTTP server with the agent (using different URL prefixes), but this is out of the scope of this example, as it brings complexity that is not related to the replication strategy, and is out of the scope of this example.

```javascript
const { createReplicaDataSource } = require('@forestadmin/datasource-replica');

const myCustomDataSource = createReplicaDataSource({
  pushDeltaHandler: async (request, onChanges) => {
    const app = express();
    app.use(express.json());

    app.post('/webhooks/on-book-:type(created|change|deleted)', (req, res) => {
      onChanges({
        newOrUpdatedEntries:
          req.params.type === 'created' || req.params.type === 'change'
            ? [{ collection: 'book', record: req.body }]
            : [],
        deletedEntries:
          req.params.type === 'deleted'
            ? [{ collection: 'book', record: { id: req.body.id } }]
            : [],
      });

      res.status(204).send();
    });

    // Use a different port than the agent
    app.listen(3000);
  },
});
```
