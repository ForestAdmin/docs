---
title: forest login
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Developers

## Description

Authenticate the Forest Admin CLI with your Forest Admin account.

## Content outline

1. **Command Overview**
   ```bash
   forest login
   ```

2. **What It Does**
   - Opens browser for authentication
   - Stores authentication token
   - Enables CLI commands

3. **Authentication Flow**
   - Run `forest login`
   - Browser opens automatically
   - Login to Forest Admin
   - Grant CLI access
   - Return to terminal

4. **Token Storage**
   - Local configuration
   - Token location
   - Token expiration
   - Security considerations

5. **Options**
   - `--token <token>` - Login with token directly
   - `--email <email>` - Pre-fill email

6. **Examples**
   ```bash
   # Interactive login
   forest login

   # Login with token
   forest login --token YOUR_TOKEN
   ```

7. **Troubleshooting**
   - Browser doesn't open
   - Authentication fails
   - Token expired
   - Permission denied

8. **Logout**
   ```bash
   forest logout
   ```

## Related pages

- [CLI Overview](/reference/cli/overview)
- [Init](/reference/cli/init)
