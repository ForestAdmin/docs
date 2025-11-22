---
title: Include/exclude models
---
By default, all models declared in your app are analyzed by the Forest Admin agent in order to display them as collections in your admin panel.

You can exclude some of them from the analysis to never send their metadata to Forest Admin. By doing this, these models will therefore never be available in your admin panel.

To do so, add the following code to **either** define which models are included **or** excluded.

### SQL

#### Include models

```javascript
...

app.use(require('forest-express-sequelize').init({
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  objectMapping,
  connections,
  includedModels: ['customers']
}));

...
```

#### Exclude models

```javascript
...

app.use(require('forest-express-sequelize').init({
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  objectMapping,
  connections,
  excludedModels: ['documents', 'transactions']
}));

...
```

### Mongodb

#### Include models

```javascript
...

app.use(require('forest-express-mongoose').init({
  modelsDir: __dirname + '/models',
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  sequelize: require('./models').sequelize,
  includedModels: ['customers']
}));

...
```

#### Exclude models

```javascript
...

app.use(require('forest-express-mongoose').init({
  modelsDir: __dirname + '/models',
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  sequelize: require('./models').sequelize,
  excludedModels: ['documents', 'transactions']
}));

...
```

