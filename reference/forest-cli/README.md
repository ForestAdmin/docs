You've seen in the previous section how [developing on Forest Admin](../) leverages our powerful **Forest Admin CLI** to manage your UI changes.

### Installing the Forest Admin CLI

To install the Forest Admin CLI, run the following command in your terminal:

```
npm install -g forest-cli
```

For further details on the package, check out [this page](https://www.npmjs.com/package/forest-cli).

### Using the Forest Admin CLI

In the following pages, we'll cover in details the most important commands available with Forest Admin CLI, from introduction to advanced usage. Here are the main commands available:

- [login](login.md)
- [init](init.md)
- [branch](branch.md)
- [switch](switch.md)
- [set-origin](set-origin.md)
- [push](push.md)
- [environments:create](environments-create.md)
- [environments:reset](environments-reset.md)
- [deploy](deploy.md)
- projects
- schema
- help
- user
- logout

Some additional commands might be added in the future. In the meantime, those should be largely sufficient to manage your development workflow.

{% hint style="info" %}
Be aware that almost all commands take the `FOREST_ENV_SECRET` env variable, provided on the command or inside your _.env_ file, to know in which environment the command will apply.
{% endhint %}
