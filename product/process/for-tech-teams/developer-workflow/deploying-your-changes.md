---
title: Deploying your changes
description: Deploy your layout changes to Remote and Production environments
---

You're building a great feature that requires tweaking your layout (UI), you've used the Layout Editor and it looks just like you had imagined? Well now is the time to stage it to an upper Environment.

Imagine this is your current situation:

![Current Situation](https://docs.forestadmin.com/assets/dev-workflow-current-situation.png)

This would be the case if you chose "Staging" as the origin of your branch when you first created it. You have then made **layout changes (Δ)** over it.

## Applying your changes to your production environment: `deploy`

`deploy` means applying your branch's changes to your reference environment definitively. To achieve this, you'll be using Forest Admin CLI's [deploy](/product/reference/cli/deploy) command:

```bash
forest deploy
```

The result is the following:

![End Situation](https://docs.forestadmin.com/assets/dev-workflow-end-situation.png)

{% hint style="info" %}
As all your environments' layouts depend on your **reference** environment, the `deploy` command will apply the layout changes to all your project environments.
In the example above, your layout changes are also applied to the Remote Environment, as it is based on Production.
{% endhint %}

{% hint style="warning" %}
Deploy with care as such action cannot be reverted.
{% endhint %}

{% hint style="warning" %}
Don't forget to **deploy your backend changes** (if any) before the `deploy` command, as showcased on [this flowchart](/product/process/for-tech-teams/developer-workflow/development-workflow).
{% endhint %}

## Testing your changes on a remote environment: `push`

Let's say that before the deployment to Production, you want to test and validate your charges on a staging environment.

`push` means moving your Branch's changes to a Remote Environment set as the origin of your Branch. To achieve this, you'll be using Forest Admin CLI's [push](/product/reference/cli/push) command:

```bash
forest push
```

{% hint style="warning" %}
Pushing your changes from your local Branch will automatically **delete** it.
{% endhint %}

Note that you'll be pushing your **current** Branch. To select another Branch, use [switch](/product/reference/cli/switch).
If the origin of your Branch is not the Remote you want (ie: staging) change it with [set-origin](/product/reference/cli/set-origin)

{% hint style="info" %}
As your company grows, so does your development flow. You may need a more complex hierarchy between environments where you have more than one layer of test environments before production (test, staging, qa, preprod,...). This is possible using the Forest Admin CLI and the right Environment settings, see [Environments](/product/process/for-tech-teams/developer-workflow/environments#change-environment-origin).
{% endhint %}

![Push](https://docs.forestadmin.com/assets/dev-workflow-push.png)

### Deploying the layout of a remote environment

Once you have tested your new layout on "Remote 1", you can't deploy your Branch layout to "Production", as your Branch has been deleted by the action of pushing it to "Remote 1".

To deploy it from this Environment, simply **click on "Deploy to production"** in the top banner of your Environment layout.

![Deploy Remote](https://docs.forestadmin.com/assets/dev-workflow-deploy-remote.png)

### Making changes directly from a remote environment

Imagine you have pushed a Branch layout changes onto a Remote Environment, but notice a slight change is still required in the layout. You can apply the final touches using the Layout Editor from the Remote Environment. It will be applied on top of your Branch layout changes; any changes you make on that Remote Environment will also be deployed when you decide to run `forest deploy`.

You can also create a new branch with your Remote Environment as the origin and do the process explained above.

## Related pages


  * [Using Branches](/product/process/for-tech-teams/developer-workflow/using-branches.md) - Learn about branches
  * [Environments](/product/process/for-tech-teams/developer-workflow/environments.md) - Manage your environments
  * [CLI Reference](/product/reference/cli/overview.md) - CLI commands reference


