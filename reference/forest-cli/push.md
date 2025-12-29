The `forest push` command enables you to apply your local changes to a Remote Environment: for instance, pushing to your staging environment will result in your latest local layout changes being visible on your staging.

```
$ forest push --help
Push layout changes of your current branch to the branch origin.

USAGE
  $ forest push

OPTIONS
  --projectId=projectId          The id of the project to work on.
  --force                        Skip push changes confirmation.
  --help                         Display usage information.
```

### Pushing to a remote environment

It is paramount to understand this command before using it:

![](../../assets/push-command.png)

Pushing a branch to a Remote Environment means applying your latest layout changes to your origin Environment of your branch. In the figure above, your layout changes (Δ) will be moved from `my-branch` to `Staging`.

{% hint style="warning" %}
Pushing your changes from your local branch will automatically **delete** it.
{% endhint %}

To push layout changes to your origin:

```
$ forest push
```

You will be prompted for confirmation before pushing to a remote:

```
$ forest push
[? Push branch my-current-branch onto Remote2 (Y|n): Y
```

{% hint style="info" %}
To skip that confirmation, use the `--force` option.
{% endhint %}

### Push from the UI

{% hint style="warning" %}
This action is only possible for Remote Environments that have an origin that is not the reference Environment (so generally not Production as origin).
{% endhint %}

![](../../assets/push-ui.png)

To push your latest layout changes from a Remote Environment, use the **top banner link**: "Push to ...".

### Difference between `push` and `deploy` commands

{% hint style="danger" %}
Do not be confused by `forest push` and `forest deploy` commands.
{% endhint %}

- `forest push` applies the latest layout changes of a branch or environment to a non-reference environment (i.e. Remote Environments once your admin panel live in production).
- `forest deploy` applies your latest layout changes **definitively** to your reference environment (i.e. Production once your admin panel live in production).

{% hint style="info" %}
You cannot `push` to the Production Environment, because anything added on Production should be definitive. Therefore you can only `deploy` to Production.
{% endhint %}
