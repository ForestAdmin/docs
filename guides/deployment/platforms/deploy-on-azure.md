This tutorial is designed to assist you with a step-by-step guide to deploy the admin backend to Azure AppService

# Prerequisites

- You have an azure account validated with a credit card
- A valid subscription on Azure to run AppService applications
- You already have a ForestAdmin project with a development environment
- The source code of your ForestAdmin project has its own GitHub repository

# Code preparation

## Application port

Azure will provide the port on which you need to mount the server with the environment variable named `PORT`. By default, ForestAdmin
generate the source code with the variable `APPLICATION_PORT`.

Simply change how the port is being provided by using the following:

```typescript
agent.mountOnStandaloneServer(
  Number(process.env.PORT || process.env.APPLICATION_PORT || 8080),
);
```

## Forestadmin schema

This step-by-step guide implies a build and release phase in a further step. To initiate a proper release, we need to change the ForestAdmin
schema file from `.forestadmin-schema.json` to `forestadmin-schema.json` (simply remove the dot). Otherwise, this mandatory file will not be taken into account during release.

To properly propagate the change, add the following configuration option in the `createAgent` function call:

```typescript
const agent = createAgent<Schema>({
  ...
  schemaPath: './forestadmin-schema.json',
});
```

# Create a new appservice app on Azure

## Create the application

Reach the AppService section of Azure and click and create a new application
![](../../assets/deploy-on-azure-create-app.png)

## Configure the application

On the `basics` section, select:

- Your subscription and your resource group
- Give a name to your application
- Select `Code` for the `Publish` configuration and your NodeJS version
- Select your preferred operating system (ForestAdmin works on both `Linux` and `Windows`)
- Select your Region and your Plan. Please do note that ForestAdmin is small and minimalistic application that requires very few resources to run

![](../../assets/deploy-on-azure-basics.png)

Then, reach the Deployment tab of the configuration. Activate the `Continuous deployment settings`, and configure the integration so the GitHub repository of your project is connected to your app.
The idea is to link your AppService application to a branch of your GitHub repository so anytime something new gets added on that branch, is released is done automatically. Azure will automatically create
GitHub actions that you can customize in the future.

![](../../assets/deploy-on-azure-continuous-deployment.png)

Next click on the Networking tab, and ensure that public access is available, otherwise your application won't be accessible from the public internet.

![](../../assets/deploy-on-azure-networking.png)

Reach the next section, being `Monitor + secure`. You can activate monitoring feature at your convenience. After that, reach the next section of the configuration and click on create at the very bottom left.

{% hint style="success" %}
Your new GitHub action will automatically run after that step, and a first deployment will occur.
{% endhint %}

## Configure your production environment

Now that the app is running, we need to configure it with the proper environment variables from ForestAdmin.

From the ForestAdmin web application, start a `deploy to production` process. You will enter a wizard that will assist you in the creation of your production environment.

First, enter the URL of your application when prompted. You can retrieve it from the `Overview` section of your Azure application, under the label `default domain`.

Secondly, enter the configuration of your production database

Then finally, At the end of the `Deploy to production` process, you will be prompted with some environment variables. Reach the `settings > environment variables` section of your Azure project, and add every of the variables prompted by ForestAdmin.

After restarting your server, your production environment should be up and running !

{% hint style="info" %}
Having problems deploying? Check out [troubleshooting common problems](https://community.forestadmin.com/t/deploying-on-google-cloud-platform-forestadmin-schema-json-file-does-not-exist/4406) in our community.
{% endhint %}
