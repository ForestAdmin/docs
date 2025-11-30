The `forest deploy` command enables you to apply some layout changes to your [reference](../using-branches.md#what-is-a-branch) environment.

```
$ forest deploy --help

Deploy layout changes of the current branch to the reference one.

USAGE
  forest deploy

OPTIONS
  --force                  Skip confirmations.
  --help                   Display usage information.
```

### Deploying your changes

It is paramount to understand this command before using it:

![](../../assets/deploy-command.png)

As you can see in the above figure, the layout changes (Δ) located in your local branch `my-branch` will be **irreversibly applied** to your Production Environment layouts. Since your Remote Environment have Production as origin, the changes will appear there too.

To deploy, simply run `forest deploy`:

```
$ forest deploy
```

{% hint style="info" %}
To be able to deploy, the origin of the branch should be the reference Environment (generally Production).
{% endhint %}

### Deploy from the UI

{% hint style="warning" %}
This action is only possible for Remote Environments that have an origin that is the reference Environment (so generally Production as origin).
{% endhint %}

![](../../assets/deploy-ui.png)

To deploy your latest layout changes from a Remote Environment, use the **top banner link**: "Deploy to ...".
