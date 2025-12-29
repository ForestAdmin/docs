The `forest init` command provides an all-in-one initialization command that will:

- allow you to log in if you haven't already done so with [forest login](login.md)
- set up your Development Environment on a given project.

```
$ forest init --help
Set up your development environment in your current folder.

USAGE
  $ forest init

OPTIONS
  -p, --projectId=projectId  The id of the project you want to init.
```

{% hint style="danger" %}
`forest init` is not meant to help you create a new project. If you don't have an existing project yet, please refer to [this guide](../../getting-started/quick-start.md).
{% endhint %}

{% hint style="info" %}
`forest init` should be run from your project's codebase root directory.
{% endhint %}

#### Login

All Forest Admin CLI commands require to be **authenticated**. You can do this on the fly using `forest init` or use `forest login` first. Refer to the [forest login](login.md) page if you need further details.

#### Project selection

The Development Environment you will initialize is unique per project and per developer. It is thus mandatory that we identify the right project to create your environment accordingly:

If you only have 1 project attached to your Forest Admin account, this step will be automatically skipped.

If you have more than 1 project, you'll be asked to choose one. **It must match the project from your current directory.**

#### Endpoint selection

If you're using `forest init`, it means your Agent should already be running **locally**. If you used the default values used during installation, your endpoint should be `http://localhost:3310`. <!-- markdown-link-check-disable-line -->

Since Forest Admin needs to know your project's local endpoint to create your Development Environment, we've left the same default values. You can just hit "Enter" to use those; otherwise, simply fill in the _host_ and _port_ to fit your configuration.

```
[? Enter your local agent host: (localhost) localhost
[? Enter your local agent port: (3310)
```

{% hint style="success" %}
At this point, your **Development Environment** will be **created**! 🎉
{% endhint %}

{% hint style="info" %}
Note that your new Development Environment is identified by the `FOREST_ENV_SECRET` in your `.env` file. If you do not have such file, it will be generated automatically during the command execution.
{% endhint %}

#### (optional) connecting your database

To work properly in your local environment, your Forest Admin Project needs to be connected to the corresponding database (which could be local or remote, it's your choice).

As a result, we've added an extra step to the `forest init` command to help you set up your database. If you accept the help, you'll be taken through a few prompts and your `DATABASE_URL`{{#nodejs}} (and potentially [other environment variables](../../getting-started/install/create-your-agent.md)){{/nodejs}} will be generated and added to your `.env` file.
