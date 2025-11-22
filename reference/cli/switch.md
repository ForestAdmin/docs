---
title: forest switch
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Developers

## Description

Switch between different Forest Admin branches or environments.

## Content outline

1. **Command Overview**
   ```bash
   forest switch <branch-name>
   ```

2. **What It Does**
   - Changes active branch
   - Updates environment variables
   - Switches Forest Admin UI context

3. **Switching Branches**
   ```bash
   # Switch to feature branch
   forest switch feature/new-action

   # Switch back to production
   forest switch main
   ```

4. **Viewing Current Branch**
   ```bash
   forest branch --current
   ```

5. **Options**
   - `--create` - Create and switch in one command
   - `--force` - Force switch (discard changes)

6. **Examples**
   ```bash
   # Switch to existing branch
   forest switch develop

   # Create and switch
   forest switch --create feature/customer-export

   # Switch back to production
   forest switch production
   ```

7. **Best Practices**
   - Commit changes before switching
   - Restart agent after switching
   - Clear browser cache if needed

8. **Troubleshooting**
   - Branch not found
   - Uncommitted changes
   - Environment mismatch

## Related pages

- [Branch](/reference/cli/branch)
- [Push](/reference/cli/push)
- [CLI Overview](/reference/cli/overview)
