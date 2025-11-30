---
title: Environments & Branches
description: Overview of environments and branches for Forest Admin development workflow
---

This section covers the development workflow for Forest Admin, including how to manage environments, branches, and deploy your changes.

## Overview

Your Forest Admin panel is composed of two parts:
- **Backend (Agent)**: Part of your codebase, versioned with Git
- **Frontend (UI)**: Managed on Forest Admin servers, versioned with Branches

This separation requires specific tools and workflows to manage both effectively.

## Development Workflow


  * [Development Workflow](/product/process/for-tech-teams/developer-workflow/development-workflow.md) - Understand the complete development workflow with Forest Admin
  * [Environments](/product/process/for-tech-teams/developer-workflow/environments.md) - Create and manage Development, Production, and Remote environments
  * [Using Branches](/product/process/for-tech-teams/developer-workflow/using-branches.md) - Version control for your Forest Admin layouts
  * [Deploying Changes](/product/process/for-tech-teams/developer-workflow/deploying-your-changes.md) - Deploy your layout changes to Remote and Production


## Key Concepts

### Environments

Forest Admin supports multiple environments:
- **Development**: Your local environment for testing
- **Production**: Your live environment with real data
- **Remote**: Staging, QA, or other testing environments

Each environment has its own:
- Agent URL
- Database connection
- Environment secret (`FOREST_ENV_SECRET`)
- Layout configuration

### Branches

Branches allow you to version your UI layout changes:
- Create a branch from any environment
- Make layout changes without affecting the origin
- Test changes before deploying
- Push to Remote environments or deploy to Production

### Layout

A **layout** is the complete UI configuration including:
- Collection visibility and ordering
- Field configurations
- Smart Actions
- Smart Views
- Charts and dashboards
- Permissions and roles

## CLI Commands


  * [forest branch](/product/reference/cli/branch.md) - Create a new branch
  * [forest deploy](/product/reference/cli/deploy.md) - Deploy to production
  * [forest push](/product/reference/cli/push.md) - Push to remote environment


## Related Topics


  * [Schema Updates](/product/process/for-tech-teams/developer-workflow/schema-updates.md) - Learn how to update your data schema
  * [CLI Reference](/product/reference/cli/overview.md) - Complete CLI command reference


