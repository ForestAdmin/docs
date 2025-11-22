---
title: Quick start

description: Let's get you up and running on Forest Admin in minutes!
---
### Step 1: Create an account and follow the onboarding

Go to [https://app.forestadmin.com/signup](https://app.forestadmin.com/signup), create an account and install your project.

{% hint style="info" %}

For the purpose of this tutorial, we have used [this database](../how-tos/databases/use-a-demo-database.md). Feel free to use it if you don't have one.

{% endhint %}

At the end of your onboarding, you will **out-of-the-box** be able to:

* Access all your data **(1)**
* Export your data **(2)**
* Add a record **(3)**
* View and edit a record **(4)**
* Edit your UI **(5)**
* Search and filter **(6)**

![](</images/legacy/javascript-agents/image (547).png>)

However, your business logic likely requires more features. What if you need to...

* refund an order
* upload new documents, accept or reject them, or ask customers to update their documents,
* contact a customer or ask a team member to perform an action,
* and much more?

It's possible with **smart actions** :point_down:

### Step 2: Create a Smart Action

Let's say you want to let your customer support team to easily refund orders, you can quickly create a smart action.

<Tabs>
  <Tab title="SQL (Sequelize)">
Declare it in your `/forest/orders.js` file:

```javascript
const { collection } = require('forest-express-sequelize');

collection('orders', {
  actions: [{
    name: 'Refund order',
  }],
});
```

Then implement it according to your business logic:

```javascript
const { PermissionMiddlewareCreator } = require('forest-express-sequelize');
const permissionMiddlewareCreator = new PermissionMiddlewareCreator('orders');

...

router.post('/actions/refund-order', permissionMiddlewareCreator.smartAction(), (req, res) => {
  // Add your own logic, like calling a Stripe API for instance
  res.send({ success: 'Order refunded!' });
});

...

module.exports = router;
```
  </Tab>
  <Tab title="MongoDB (Mongoose)">
Declare it in your `/forest/orders.js` file:

```javascript
const { collection } = require('forest-express-mongoose');

collection('orders', {
  actions: [{
    name: 'Refund order',
  }],
});
```

Then implement it according to your business logic:

```javascript
const { PermissionMiddlewareCreator } = require('forest-express-mongoose');
const permissionMiddlewareCreator = new PermissionMiddlewareCreator('orders');

...

router.post('/actions/refund-order', permissionMiddlewareCreator.smartAction(), (req, res) => {
  // Add your own logic, like calling a Stripe API for instance
  res.send({ success: 'Order refunded!' });
});

...

module.exports = router;
```
  </Tab>
</Tabs>

{% hint style="warning" %}

You must make sure that all your Smart Actions routes are configured with the Smart Action middleware:
`permissionMiddlewareCreator.smartAction()`. This is mandatory to ensure that all features built on top of Smart Actions work as expected (permissions, approval workflows,...).

{% endhint %}

{% hint style="info" %}

You may have to [add CORS headers](../how-tos/setup/configuring-cors-headers.md) to enable the domain `app.forestadmin.com` to trigger API call on your Application URL, which is on a different domain name (e.g. _localhost:8000_).

{% endhint %}

<Check>
Congrats! Now it's possible to refund an order!
</Check>

![](</images/legacy/javascript-agents/image (531).png>)

### Step 3: Deploy to Production

Now that you have a fully functional admin panel, the next logical step is to **make it live**, with your live (production) data. Click on **Deploy to Production** and follow the flow.

![](</images/legacy/javascript-agents/image (487).png>)

<Check>
That's it! You are now fully operational on Forest Admin.
</Check>

Next, we recommend reading about our recommended [development workflow](development-workflow.md).
