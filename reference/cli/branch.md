---
title: forest branch
---
> **Status:** TODO - Priority: HIGH
>
> **Audience:** Developers

## Description

Create and manage branches for isolated development environments.

## Sources to migrate

- `developer-guide-agent-v2/sources/deployment/using-branches.md`
- CLI documentation

## Content outline

1. **Command Overview**
   ```bash
   forest branch <branch-name>
   forest branch list
   forest branch delete <branch-name>
   ```

2. **Creating Branches**
   - Branch naming conventions
   - Base branch selection
   - Automatic environment creation

3. **Branch Management**
   - List all branches
   - Switch between branches
   - Delete branches
   - Branch status

4. **Branch Workflow**
   - Feature development
   - Layout changes isolation
   - Testing changes
   - Merging to production

5. **Options & Flags**
   - `--origin` - Specify base branch
   - `--delete` - Delete branch
   - `--list` - List branches

6. **Examples**
   ```bash
   # Create new branch
   forest branch feature/new-action

   # List all branches
   forest branch list

   # Delete branch
   forest branch delete feature/new-action
   ```

## Related pages

- [forest switch](/reference/cli/switch)
- [forest push](/reference/cli/push)
- [forest deploy](/reference/cli/deploy)
- [Guides > Development Workflow](/guides/deployment/development-workflow)
