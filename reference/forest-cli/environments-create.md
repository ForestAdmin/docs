## Creating a test environment

You might consider having Test Environments created (automatically in CI/CD or manually) for a short period of time to help your team on testing your new admin panel evolutions.

Remote Environments can be used for testing as-well, but in this kind of environment you will have to handle roles and permissions to give access to a newly created Smart Action or a new Collection. This can lead to frustration as it can break your continuous delivery process because as a developer, you might not have access to the roles settings of your project. This would require you, for every of the environment created, to ask for an admin to set the roles and permissions up.

Test Environments have been designed to tackle this topic. They basically are Remote Environments, but without the roles and permissions system.

These Environments can be accessed by developers, admins and editors from the environments' dropdown. Users can have access as-well, but the environment will not be proposed in their dropdown for convenience. So you will need to share the entire layout url to them.

This kind of Environment should be created by using the Forest Admin CLI.
You’ll have to use the `--disabledRoles` option with the `environments:create` command in order to disable the roles of the new Environment.

Here is an example:

```
forest environments:create --name 'pr-1930' -u 'https://pr-1930.company.com' --disableRoles
```

And that is it! Make sure your test server is up and running started and reach your admin panel to observe that a new Environment appeared with everything (data, actions,...) accessible by default.

{% hint style="danger" %}
As any user invited to join this Test Environment will have full permission to read and write on the targeted data sources, be sure that the data sources content is not sensitive and can be read, updated, deleted without any data security concerns.
{% endhint %}
