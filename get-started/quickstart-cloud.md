---
title: "Quickstart: Cloud"
description: "Get your admin panel running in 5 minutes with Forest Admin Cloud"
---


Forest Admin Cloud is the fastest way to get started. We host and manage everything for you—just connect your database.

## What you'll do

<Steps>
  <Step title="Create your account">
    Sign up and create a new project
  </Step>
  <Step title="Connect your database">
    Provide your database credentials
  </Step>
  <Step title="Access your admin panel">
    Start managing your data immediately
  </Step>
</Steps>

## Prerequisites

Before you begin, make sure you have:

- **Database credentials** (PostgreSQL, MySQL, MongoDB, etc.)
- **Database accessible from the internet** or willingness to set up tunneling

{% hint style="warning" %}

If your database restricts IPs, you'll need to whitelist **35.180.175.97**

{% endhint %}

## Step 1: Create your account

1. Visit [app.forestadmin.com/signup](https://app.forestadmin.com/signup)
2. Sign up for a free account
3. Create a new project and choose a name

## Step 2: Connect your database

### Choose your database type

Select your database from the list:
- PostgreSQL
- MySQL
- MongoDB
- SQL Server
- And more...

### Enter connection details

Use this tool to build your connection URI:

<ConnectionUriBuilder dbType="postgresql" />

Or provide your credentials directly in Forest Admin's interface.

{% hint style="success" %}

**Using a local database?** You can set up SSH tunneling using tools like Ngrok, Bastion, or Localtunnel. Forest Admin will provide an SSH public key to add to your server's `authorized_keys` file.

{% endhint %}

### Test connection

Click **"Test connection"** to verify your credentials, then click **"Next"** to sync your schema.

## Step 3: Access your admin panel

🎉 **Congratulations!** Your admin panel is ready.

Forest Admin automatically:
- ✅ Discovered your database schema
- ✅ Created collections for each table
- ✅ Set up CRUD operations
- ✅ Configured search and filters
- ✅ Generated a default layout

You can now:
- Browse and edit your data
- Search and filter records
- Create, update, and delete entries
- Export data
- Invite team members

## What's next?



  * [Customize your UI](/product/build/layout-editor.md) - Use the layout editor to customize your admin panel
  * [Add business logic](/product/process/actions/overview.md) - Create Smart Actions for custom workflows
  * [Set permissions](/product/control/roles-permissions.md) - Configure roles and access control



## Troubleshooting

<AccordionGroup>
  <Accordion title="Connection failed - Database not accessible" icon="circle-xmark">
    **Solution:** Your database needs to be accessible from the internet.

    Options:
    - Whitelist Forest Admin IP: **35.180.175.97**
    - Use Cloud SQL/RDS with public access
    - Set up SSH tunneling (see Step 2)
  </Accordion>

  <Accordion title="No tables showing up" icon="table">
    **Solution:** Make sure your database user has read permissions on the tables you want to access.

    Check your database permissions and refresh the schema in Forest Admin settings.
  </Accordion>

  <Accordion title="I want to use a local database" icon="laptop">
    **Start with your production database first.**

    With Cloud deployment, you connect your production database initially. Once set up, you can create a development environment that connects to your local database.

    Learn more: [Managing Environments](/product/manage/environments)
  </Accordion>
</AccordionGroup>

## Need help?

- **Documentation**: [Cloud Architecture](/product/integration/architectures/cloud)
- **Community**: [community.forestadmin.com](https://community.forestadmin.com)
- **Support**: [support@forestadmin.com](mailto:support@forestadmin.com)
