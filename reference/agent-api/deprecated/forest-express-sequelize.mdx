---
title: forest-express-sequelize Reference (LEGACY)
---
> **Status:** LEGACY - DO NOT USE FOR NEW PROJECTS
>
> **Audience:** Legacy users (migration reference only)

## ⚠️ Legacy Notice

**forest-express-sequelize is legacy and will reach End of Life on [Date].**

**Migrate to @forestadmin/agent:** [Migration Guide](/guides/migration/from-v1/overview)

---

## Legacy Documentation

This page provides reference for existing implementations only. **Do not use for new projects.**

### Installation (Legacy)

```bash
npm install forest-express-sequelize --save
```

### Basic Setup (Legacy)

```javascript
const express = require('express');
const { Liana } = require('forest-express-sequelize');
const models = require('./models');

const app = express();

app.use(Liana.init({
  modelsDir: __dirname + '/models',
  envSecret: process.env.FOREST_ENV_SECRET,
  authSecret: process.env.FOREST_AUTH_SECRET,
  sequelize: require('./models').sequelize
}));

app.listen(3000);
```

### Smart Actions (Legacy)

**Forest/routes/users.js:**
```javascript
const express = require('express');
const router = express.Router();
const Liana = require('forest-express-sequelize');

router.post('/actions/send-email',
  Liana.ensureAuthenticated,
  (req, res) => {
    const userIds = req.body.data.attributes.ids;
    const values = req.body.data.attributes.values;

    // Action logic here

    res.send({
      success: 'Email sent!'
    });
  }
);

module.exports = router;
```

**Forest/users.js:**
```javascript
const { collection } = require('forest-express-sequelize');

collection('users', {
  actions: [{
    name: 'Send email',
    type: 'bulk',
    fields: [{
      field: 'subject',
      type: 'String',
      isRequired: true
    }]
  }]
});
```

### Smart Fields (Legacy)

```javascript
const { collection } = require('forest-express-sequelize');

collection('users', {
  fields: [{
    field: 'fullName',
    type: 'String',
    get: (user) => {
      return `${user.firstName} ${user.lastName}`;
    }
  }]
});
```

### Smart Segments (Legacy)

```javascript
const { collection } = require('forest-express-sequelize');

collection('users', {
  segments: [{
    name: 'Active users',
    where: { status: 'active' }
  }]
});
```

### Smart Relationships (Legacy)

```javascript
const { collection } = require('forest-express-sequelize');

collection('users', {
  fields: [{
    field: 'recentOrders',
    type: ['String'],
    reference: 'orders.id',
    get: (user) => {
      return user.getOrders({
        where: {
          createdAt: {
            [Op.gte]: new Date(Date.now() - 30*24*60*60*1000)
          }
        }
      });
    }
  }]
});
```

---

## Migration to @forestadmin/agent

### Key Differences

| forest-express-sequelize | @forestadmin/agent |
|-------------------------|-------------------|
| `Liana.init()` | `createAgent()` |
| `collection('users', {...})` | `agent.customizeCollection('users', ...)` |
| Smart Actions in routes | `collection.addAction()` |
| Smart Fields in collection config | `collection.addField()` |
| `res.send({ success: '...' })` | `resultBuilder.success('...')` |

### Migration Example

**Before (forest-express-sequelize):**
```javascript
collection('users', {
  actions: [{
    name: 'Send email',
    type: 'single',
    fields: [{ field: 'message', type: 'String' }]
  }]
});

router.post('/actions/send-email', (req, res) => {
  const userId = req.body.data.attributes.ids[0];
  const message = req.body.data.attributes.values.message;

  // Send email...

  res.send({ success: 'Email sent!' });
});
```

**After (@forestadmin/agent):**
```typescript
agent.customizeCollection('users', collection =>
  collection.addAction('Send email', {
    scope: 'Single',
    form: [
      { label: 'Message', type: 'String' }
    ],
    execute: async (context, resultBuilder) => {
      const user = await context.getRecord(['email']);
      const { message } = context.formValues;

      // Send email...

      return resultBuilder.success('Email sent!');
    }
  })
);
```

---

## Complete Migration Guide

**Follow these steps to migrate:**

1. **[Step 1: Dependencies](/guides/migration/from-v1/steps/dependencies)**
2. **[Step 2: Datasources](/guides/migration/from-v1/steps/datasources)**
3. **[Step 3: Smart Actions](/guides/migration/from-v1/steps/smart-actions)**
4. **[Step 4: Smart Fields](/guides/migration/from-v1/steps/smart-fields)**
5. **[And more...](/guides/migration/from-v1/overview)**

---

## Sources

- forest-express-sequelize npm package
- v1 documentation archive
- Migration guides

## Related Pages

- **[Migration Guide](/guides/migration/from-v1/overview)** ← START HERE
- [Node.js Agent API](/reference/agent-api/nodejs) - New agent reference
- [Node.js Setup](/product/integration/setup/nodejs) - New agent setup
- [Legacy Overview](/reference/agent-api/deprecated/overview)
