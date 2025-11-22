---
title: Environments
description: Create and manage Development, Production, and Remote environments
---

Once your project successfully installed, a local **Development** environment is created for you, with a temporary `pre-deploy-to-production` branch (more on _branches_ later).

Your first objective should be to deploy to **Production**.

## Deploying to Production

Forest Admin is meant to help you manage your operations: this can only happen if your team operates on your Production data. To do so, you need to **create your Production Environment**.

Click "Deploy to production" on the top banner or in the "Environments" tab of your Project settings.

![Deploy to Production](https://docs.forestadmin.com/assets/environment-deploy-to-production.png)

### Deploy your Agent

In the first step, you need to input your Agent's URL. This is the URL of the server onto which you have deployed (or will soon deploy) your Agent's codebase:

{% hint style="info" %}

If you need help deploying your Agent's codebase, here are step-by-step guides showing how it can be done on various platforms. See the [Deployment guides](/guides/deployment/production).

{% endhint %}

![Deploy Step 1](https://docs.forestadmin.com/assets/environment-deploy-step-1.png)

{% hint style="warning" %}

Note that for security reasons, your agent must use the **HTTPS** protocol.

{% endhint %}

{% hint style="info" %}

The URL must not end with a trailing `/`.

{% endhint %}

### Connect to your database

In the next step, you need to fill out your Production database credentials:

![Deploy Step 2](https://docs.forestadmin.com/assets/environment-deploy-step-2.png)

{% hint style="info" %}

Your **database credentials** never leave your browser and are solely used to generate environment variables on the next step, so they are **never exposed**.

{% endhint %}

### Set your environment variables

The final step requires that you add environment variables to your server. Follow the on-screen instructions:

![Deploy Step 3](https://docs.forestadmin.com/assets/environment-deploy-step-3.png)

Once your server is successfully detected and running with the indicated environment variables, a "Finish" button will appear. Click on it to finalize the creation of your Production Environment.

## Creating a Remote Environment

Now that your admin panel is live in production, you might want to add an extra step for testing purposes. Forest Admin allows you to create Remote Environments (for test, qa, staging, pre-production,... purposes).

To create a new Remote Environment, go to your Project settings **(1)**:

![Project Settings](https://docs.forestadmin.com/assets/environment-project-settings.png)

Then from the "Environments" Tab, click on "Add a new environment" **(2)**.

![Add New Environment](https://docs.forestadmin.com/assets/environment-add-new.png)

{% hint style="info" %}

You can choose to deploy to a Remote Environment **before** going to production (see below), it's up to you.

{% endhint %}

![Deploy To Environment](https://docs.forestadmin.com/assets/environment-deploy-to.png)

### Choose your Environment name

You'll first be asked to input the name of the Remote Environment you wish to create:

![Remote Step 1](https://docs.forestadmin.com/assets/environment-remote-step-1.png)

### Enter your Agent's URL for that Environment

Deploy your Agent server in this specific environment, then input its URL:

![Remote Step 2](https://docs.forestadmin.com/assets/environment-remote-step-2.png)

### Connect to your database

You need a separate database for this new environment: if you're creating a _Staging_, then it must be your _staging_ data, so your _staging_ database.

![Remote Step 3](https://docs.forestadmin.com/assets/environment-remote-step-3.png)

{% hint style="info" %}

Your **database credentials** never leave your browser and are solely used to generate environment variables on the next step, so they are **never exposed**.

{% endhint %}

### Set your environment variables

The final step requires that you add environment variables to your server. Follow the on-screen instructions:

![Remote Step 4](https://docs.forestadmin.com/assets/environment-remote-step-4.png)

Once your server is successfully detected and running with the indicated environment variables, a **Finish** button will appear. Click on it to finalize the creation of your new Remote Environment.

## Change Environment origin

You can change the origins of your Environments to create complex workflows, for instance, `dev > staging > preprod > production`.
All the layouts of an environment will be generated based on its parent's layout.

To do so, click on the Environment you wish to change the origin of, and from its details page, select the desired origin in the "Set Origin" section.

![Set Environment Origin](https://docs.forestadmin.com/assets/environment-set-origin.png)

{% hint style="warning" %}

All child Environments will be refreshed based on the new hierarchy.

{% endhint %}

## Set an Environment as Production

A standard project usually has a production and at least a staging Environment, but you may be using other Remote Environments. At some point, you may feel the need to set another Environment as your Production Environment (also known as "reference").

{% hint style="info" %}

To set as production an Environment it should have the actual reference as origin.

{% endhint %}

To do so, click on the Environment you wish to set as production, and from its details page, click "Set as production".

![Set as Production](https://docs.forestadmin.com/assets/environment-set-as-production.png)

{% hint style="warning" %}

The actual reference will take the new production as the origin. All children layouts will be refreshed. Any layout change that is not applicable will be ignored.

{% endhint %}

## Delete an Environment

You may also delete an Environment. **Be very careful** as there is no going back!

## Related pages



  * [Using Branches](/product/process/for-tech-teams/developer-workflow/using-branches.md) - Learn about layout version control
  * [Deploying Changes](/product/process/for-tech-teams/developer-workflow/deploying-your-changes.md) - Deploy your changes to environments


