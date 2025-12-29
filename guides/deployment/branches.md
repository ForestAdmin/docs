As we've explained in the previous page, your Admin panel's layouts configurations are saved on Forest Admin servers. As a result, you can't version them. But don't worry, we've got you covered with some great tools!

### What is a layout?

The notion of a branch cannot be explained without first explaining what a _layout_ is.

A **layout** is all the configuration that **define your user interface (UI)**. In Forest Admin, there is 1 layout per environment and per team:

![](../assets/branch-layout.png)

The [Forest Admin CLI](forest-cli-commands/) will help you manage layouts across environments.

### What is a branch?

A Branch is a fork (i.e. copy) of the layout of the Environment it is attached to.
A Branch can only be created in your own Development Environment.

{% hint style="info" %}
The **origin** of a branch is either specified using the `--origin` option or selected when prompted otherwise). You should choose the environment you want to make some layout changes on.
{% endhint %}

Once you've created a Branch, your layout will look exactly like the layout of its origin Environment.

### How do branches work?

Any **layout change** you make on your current Branch using the [Layout Editor](https://docs.forestadmin.com/user-guide/getting-started/master-your-ui/using-the-layout-editor-mode) will be **saved on your current Branch** and will not affect its origin Environment.

Imagine the following situation where you have 3 Environments:

![](../assets/branch-how-it-works.png)

The branch `my-branch` is based on the production layout. Any changes made to it are saved in your branch's layout and can later be [applied](forest-cli-commands/deploy.md) to it.

![](../assets/branch-example.png)

{% hint style="warning" %}
This also means that any changes made to the origin of your Branch will instantly reflect on your Branch.

For those familiar with git's _rebase_, this means you will **never have to** **rebase** your Branch on its origin, as it is done automatically.
{% endhint %}

### How do you create a branch?

To create a branch, you'll need to use [Forest Admin CLI](forest-cli-commands/). Make sure you've created your local Development Environment using the [init](forest-cli-commands/init.md) command. Then, to create a Branch named `my-branch` based on your `production` Environment, simply run:

```
forest branch my-branch --origin production
```

{% hint style="info" %}
Using kebab-case is recommended. However, if you prefer to use spaces in your Branch names, don't forget to surround them with quotes, like so `forest branch "my branch" --origin production`.
{% endhint %}

To learn more about the `branch` command, please visit [this page](forest-cli-commands/branch.md).

### Checking your branch information

On your interface, you can check at all times what is your current Branch and how many layout changes were made on it.
These information appear in the top banner of your admin panel. The "branches pushed" information is only relevant for Remote Environments: it shows how many Branches were already pushed onto it.

![](../assets/branch-current-branch-display.png)

Your **current** Branch will be displayed at the top.

{% hint style="info" %}
To switch your _current_ branch to another existing branch, check out the [switch](forest-cli-commands/switch.md) command.
{% endhint %}

Now that you've mastered Branch creation and management, let's dive into the next step of the development workflow: deployment.
