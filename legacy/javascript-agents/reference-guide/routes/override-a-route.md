---
title: Override a route
---
Overriding a route allows you to change or completely replace a Forest Admin's route behavior.

### Changing Forest Admin's behavior

To achieve this, use existing snippets of [default routes](default-routes.md) and modify them according to your needs.

Here are a few examples:

#### Use extended search by default

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
const express = require('express');
const {
  PermissionMiddlewareCreator,
  RecordsGetter,
  RecordsCounter,
} = require('forest-express-sequelize');
const { companies } = require('../models');

const router = express.Router();
const permissionMiddlewareCreator = new PermissionMiddlewareCreator(
  'companies'
);

//...

// Get a list of Companies - Check out our documentation for more details: https://docs.forestadmin.com/documentation/reference-guide/routes/default-routes#get-a-list-of-records
router.get(
  '/companies',
  permissionMiddlewareCreator.list(),
  (request, response, next) => {
    const { query, user } = request;
    query.searchExtended = '1';

    const recordsGetter = new RecordsGetter(companies, user, query);
    recordsGetter
      .getAll()
      .then((records) => recordsGetter.serialize(records))
      .then((recordsSerialized) => response.send(recordsSerialized))
      .catch(next);
  }
);

// Get a number of Companies - Check out our documentation for more details: https://docs.forestadmin.com/documentation/reference-guide/routes/default-routes#get-a-list-of-records
router.get(
  '/companies/count',
  permissionMiddlewareCreator.list(),
  (request, response, next) => {
    const { query, user } = request;
    query.searchExtended = '1';

    const recordsCounter = new RecordsCounter(companies, user, query);
    recordsCounter
      .count()
      .then((count) => response.send({ count }))
      .catch(next);
  }
);

//...
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
const express = require('express');
const {
  PermissionMiddlewareCreator,
  RecordsGetter,
  RecordsCounter,
} = require('forest-express-mongoose');
const { companies } = require('../models');

const router = express.Router();
const permissionMiddlewareCreator = new PermissionMiddlewareCreator(
  'companies'
);

//...

// Get a list of Companies - Check out our documentation for more details: https://docs.forestadmin.com/documentation/reference-guide/routes/default-routes#get-a-list-of-records
router.get(
  '/companies',
  permissionMiddlewareCreator.list(),
  (request, response, next) => {
    const { query, user } = request;
    query.searchExtended = '1';

    const recordsGetter = new RecordsGetter(companies, user, query);
    recordsGetter
      .getAll()
      .then((records) => recordsGetter.serialize(records))
      .then((recordsSerialized) => response.send(recordsSerialized))
      .catch(next);
  }
);

// Get a number of Companies - Check out our documentation for more details: https://docs.forestadmin.com/documentation/reference-guide/routes/default-routes#get-a-list-of-records
router.get(
  '/companies/count',
  permissionMiddlewareCreator.list(),
  (request, response, next) => {
    const { query, user } = request;
    query.searchExtended = '1';

    const recordsCounter = new RecordsCounter(companies, user, query);
    recordsCounter
      .count()
      .then((count) => response.send({ count }))
      .catch(next);
  }
);

//...
```
  </Tab>
</Tabs>

{% hint style="warning" %}
Using extended search is less performant than default search. Use this wisely.
{% endhint %}

#### Protect a specific record

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
router.delete(
  '/companies/:recordId',
  permissionMiddlewareCreator.delete(),
  (request, response, next) => {
    const { params, query, user } = request;

    if (Number(params.recordId) === 82) {
      response
        .status(403)
        .send('This record is protected, you cannot remove it.');
      return;
    }

    const recordRemover = new RecordRemover(companies, user, query);
    recordRemover
      .remove(params.recordId)
      .then(() => response.status(204).send())
      .catch(next);
  }
);
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
router.delete(
  '/companies/:recordId',
  permissionMiddlewareCreator.delete(),
  (request, response, next) => {
    const { params, query, user } = request;

    if (Number(params.recordId) === 82) {
      response
        .status(403)
        .send('This record is protected, you cannot remove it.');
      return;
    }

    const recordRemover = new RecordRemover(companies, user, query);
    recordRemover
      .remove(params.recordId)
      .then(() => response.status(204).send())
      .catch(next);
  }
);
```
  </Tab>
</Tabs>

### Replacing Forest Admin's behavior

To achieve this, simply remove the `next()` statement of any route:

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
...

// Create a Company - Check out our documentation for more details: https://docs.forestadmin.com/documentation/reference-guide/routes/default-routes#create-a-record
router.post('/companies', permissionMiddlewareCreator.create(), (req, res, next) => {
  // >> Add your own logic here <<
});

...
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
...

// Create a Company - Check out our documentation for more details: https://docs.forestadmin.com/documentation/reference-guide/routes/default-routes#create-a-record
router.post('/companies', permissionMiddlewareCreator.create(), (req, res, next) => {
  // >> Add your own logic here <<
});

...
```
  </Tab>
</Tabs>

Here's a more complete example:

<Tabs>
  <Tab title="SQL (Sequelize)">
```javascript
...

const axios = require('axios');
const { RecordSerializer } = require('forest-express-sequelize');
const { users } = require('../models');

...

router.post('/users', permissionMiddlewareCreator.create(), (request, response, next) => {
  const recordSerializer = new RecordSerializer(users);
  const axiosRequest = {
    url: 'https://<your-api>/users',
    method: 'post',
    data: request.body.data.attributes,
  };

  axios(axiosRequest)
    .then(result => recordSerializer.serialize(result.data))
    .then(resultSerialized => response.send(resultSerialized))
    .catch(error => {
      console.log('error:', error);
      next(error);
    });
});
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
```javascript
...

const axios = require('axios');
const { RecordSerializer } = require('forest-express-mongoose');
const { users } = require('../models');

...

router.post('/users', permissionMiddlewareCreator.create(), (request, response, next) => {
  const recordSerializer = new RecordSerializer(users);
  const axiosRequest = {
    url: 'https://<your-api>/users',
    method: 'post',
    data: request.body.data.attributes,
  };

  axios(axiosRequest)
    .then(result => recordSerializer.serialize(result.data))
    .then(resultSerialized => response.send(resultSerialized))
    .catch(error => {
      console.log('error:', error);
      next(error);
    });
});
```
  </Tab>
</Tabs>

