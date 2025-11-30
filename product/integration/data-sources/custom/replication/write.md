You may want to be able to create or edit new records from the Forest Admin GUI.

# Implementation

The implementation is very straightforward: you need to implement 3 handlers: `createRecordHandler`, `updateRecordHandler` and `deleteRecordHandler`.

All handlers are optional and can be omitted if you don't want to support a specific operation.

{% hint style="info" %}
The `createRecordHandler` handler can return a value. This is useful if the target API generates an `id` for the record.
{% endhint %}

# Example

In this example, we will use the [JSON Placeholder](https://jsonplaceholder.typicode.com/) API to create, update and delete records.

```javascript
const axios = require('axios');
const { createReplicaDataSource } = require('@forestadmin/datasource-replica');

const url = 'https://jsonplaceholder.typicode.com';

const myCustomDataSource = createReplicaDataSource({
  // ... Implement record synchronization as explained in the previous sections

  // Create the record in the target API
  createRecordHandler: async (collectionName, record) => {
    const response = await axios.post(`${url}/${collectionName}`, record);

    // Return the record as it was created in the target API
    return response.data;
  },

  // Update the record in the target API
  createRecordHandler: async (collectionName, record) => {
    await axios.put(`${url}/${collectionName}/${record.id}`, record);
  },

  // Delete the record in the target API
  createRecordHandler: async (collectionName, record) => {
    await axios.delete(`${url}/${collectionName}/${record.id}`);
  },
});
```
