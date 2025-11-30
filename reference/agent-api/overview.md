---
title: Agent API Overview
description: Overview of Forest Admin agent APIs across different languages
---

# Agent API Overview

Forest Admin provides agent SDKs in multiple languages to connect your backend to the Forest Admin platform. These agents expose your data models as admin interfaces with customization capabilities.

## Available Agents

### Node.js Agent (Current)

The latest Node.js agent with full TypeScript support and modern architecture.

**Package:** `@forestadmin/agent`

**Features:**
- Full TypeScript support with type inference
- Multi-datasource support
- Plugin ecosystem
- Advanced customization API
- Cloud and self-hosted deployment

* [Node.js Agent Documentation](/reference/agent-api/nodejs.md) - Complete API reference for the Node.js agent

### Ruby Agent (Current)

The Ruby agent for Rails applications.

**Package:** `forest_admin_agent`

**Features:**
- ActiveRecord and Mongoid support
- Rails integration
- Custom actions and fields
- Approval workflows

* [Ruby Agent Documentation](/reference/agent-api/ruby.md) - Complete API reference for the Ruby agent

## Agent Architecture

All Forest Admin agents follow a similar architecture:

```
Application Database
       ↓
  Agent SDK (Your Backend)
       ↓
  Forest Admin API
       ↓
  Forest Admin UI
```

### Core Concepts

1. **Data Sources**: Connect to your databases (SQL, MongoDB, etc.)
2. **Collections**: Map your models to admin interfaces
3. **Customizations**: Add Smart Actions, Fields, and Segments
4. **Hooks**: Intercept and modify operations
5. **Authentication**: Secure access with Forest Admin auth

## Choosing an Agent

| Agent | Best For | Deployment |
|-------|----------|------------|
| **Node.js** | Modern applications, TypeScript projects | Self-hosted or Cloud |
| **Ruby** | Rails applications | Self-hosted |

## Migration from v1

If you're using a legacy v1 agent, see our migration guides:


  * [Migration Guide](/guides/migration/from-v1/overview.md) - Migrate from Agent v1 to v2
  * [Legacy Documentation](/legacy/agents-overview.md) - v1 agent documentation (deprecated)


## Getting Started

<Steps>
  <Step title="Choose your agent">
    Select the agent that matches your stack (Node.js or Ruby)
  </Step>

  <Step title="Install the package">
    Add the agent SDK to your project
  </Step>

  <Step title="Configure data sources">
    Connect your databases
  </Step>

  <Step title="Add customizations">
    Create Smart Actions, Fields, and Segments
  </Step>

  <Step title="Deploy">
    Deploy your agent (self-hosted or Cloud)
  </Step>
</Steps>

## Next Steps


  * [Node.js API](/reference/agent-api/nodejs.md) - Node.js API reference
  * [Ruby API](/reference/agent-api/ruby.md) - Ruby API reference
  * [Quickstart](/get-started/quickstart.md) - Get started guide


## Support

Need help with your agent implementation?

- 📚 [Documentation Portal](/get-started/intro-to-forest-admin)
- 💬 [Community Forum](https://community.forestadmin.com)
- 📧 [Support Email](mailto:support@forestadmin.com)
