---
title: "Introduction to Forest Admin"
description: "Build powerful admin panels on top of your existing data—no frontend code required"
---

Forest Admin is an **admin panel builder** that connects to your existing databases and APIs. Instead of spending months building internal tools, your team gets a fully-functional admin interface in minutes.

**From data to admin panel:** Connect your database → Get CRUD operations, role-based access, and workflows → Customize with business logic and branding.

## What Forest Admin does



  * [For Operations Teams](#) - Manage customers, orders, and operations without waiting on engineering. Handle support, approvals, and workflows in one place.
  * [For Engineering Teams](#) - Skip building internal tools. Connect your data sources and customize with code only when needed.



**Core capabilities:**
- **Data operations** — Browse, search, create, edit, delete records
- **Custom business logic** — Add actions, computed fields, workflows
- **Access control** — Roles, permissions, SSO, audit logs
- **UI customization** — Layouts, forms, dashboards, branding
- **Team collaboration** — Notes, approval workflows, notifications

## How it works

Forest Admin uses an **agent-based architecture**. An agent runs in your infrastructure (or ours) and connects to your data sources. Your team accesses the Forest Admin UI to manage data.

```
Your Database ←→ Forest Admin Agent ←→ Forest Admin UI
                 (Node.js or Ruby)        (Web Interface)
```

**The agent handles:**
- Database connections and queries
- Business logic (actions, computed fields)
- Permission enforcement

**The UI provides:**
- Data browsing and editing
- Forms and layouts
- Dashboards and analytics
- User management

{% hint style="info" %}

**Your data never leaves your control.** The agent reads/writes directly to your database. We don't store your data.

{% endhint %}


## Common use cases

**Customer Support** — View profiles, process refunds, manage subscriptions, handle tickets

**Operations** — Track orders, manage inventory, monitor KPIs, generate reports

**Content Moderation** — Review user content, manage catalogs, schedule publications

**Workflows & Approvals** — KYC/AML compliance, fraud detection, approval processes

## Next steps



  * [Quick Start](/get-started/quickstart.md) - Get up and running in 5 minutes
  * [Features Overview](/get-started/features-overview.md) - Explore what you can build
  * [Connect Your Data](/product/integration/overview.md) - Learn how to connect data sources



---

**Need help?** Join our [Community Forum](https://community.forestadmin.com) or email [support@forestadmin.com](mailto:support@forestadmin.com)
