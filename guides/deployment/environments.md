---
title: Environments & Configuration Management
---
> **Status:** TODO - Priority: HIGH
>
> **Audience:** Developers, DevOps

## Description

Manage multiple environments (development, staging, production) for your Forest Admin setup.

## Sources to migrate

- `developer-guide-agent-v2/sources/deployment/using-branches.md`
- `documentation-user-guide/project-settings/environments-tab.md`
- Environment management documentation

## Content outline

1. **Environments Overview**
   - What are environments
   - Development, Staging, Production
   - Environment isolation
   - Use cases

2. **Creating Environments**
   - UI-based creation
   - CLI creation
   - Environment settings
   - Naming conventions

3. **Environment Configuration**
   - Environment variables
   - Database connections (per environment)
   - API keys
   - Secrets management

4. **Environment Variables**
   - FOREST_ENV_SECRET (unique per environment)
   - FOREST_AUTH_SECRET
   - DATABASE_URL
   - Custom variables

5. **Branches & Environments**
   - Branch per environment
   - Origin branches
   - Branch inheritance
   - Merging changes

6. **Workflow**
   - Development → Staging → Production
   - Testing in staging
   - Promoting changes
   - Rollback strategies

7. **Layout Sync**
   - Layout per environment
   - Syncing layouts
   - Layout versioning
   - Conflict resolution

8. **Best Practices**
   - Mirror production in staging
   - Environment parity
   - Configuration management
   - Secrets security
   - Testing procedures

9. **Troubleshooting**
   - Wrong environment
   - Configuration mismatch
   - Sync issues

## Migration notes

- HIGH priority - essential for teams
- Clear workflow diagrams
- Step-by-step guides

## Related pages

- [Development Workflow](/guides/deployment/development-workflow)
- [Reference > CLI > Branch](/reference/cli/branch)
- [Process > Developer Workflow > Environments](/product/process/for-tech-teams/developer-workflow/environments-and-branches)
