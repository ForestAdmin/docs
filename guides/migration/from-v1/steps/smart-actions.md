---
title: "Migration Step 3: Smart Actions"
---
> **Status:** TODO - Priority: HIGH
>
> **Audience:** Developers (v1 users)

## Description

Migrate your Smart Actions from Agent v1 to Agent v2.

## Sources to migrate

- `developer-guide-agent-v2/sources/migration/smart-actions.md`

## Content outline

1. **v1 vs v2 Actions**
   - API differences
   - Response format changes
   - Form field syntax
   - Result types

2. **Action Definition**
   **v1:**
   ```javascript
   collection('users', {
     actions: [{
       name: 'Send email',
       // v1 syntax
     }]
   });
   ```

   **v2:**
   ```typescript
   agent.customizeCollection('users', collection =>
     collection.addAction('Send email', {
       // v2 syntax
     })
   );
   ```

3. **Form Fields**
   - v1 to v2 field mapping
   - Field types
   - Validation
   - Dynamic fields

4. **Action Handler**
   - Context API changes
   - Request/response format
   - Async/await patterns
   - Error handling

5. **Result Types**
   - Success messages
   - Error responses
   - File downloads
   - Redirects
   - Webhooks

6. **Code Examples**
   - Before/after comparisons
   - Common action patterns
   - Migration templates

7. **Testing**
   - Testing migrated actions
   - Verification checklist
   - Common issues

8. **Breaking Changes**
   - API deprecations
   - Behavior changes
   - Workarounds

## Migration notes

- HIGH priority for v1 users
- Side-by-side examples
- Automated migration tools (if available)

## Related pages

- [Migration Overview](/guides/migration/from-v1/overview)
- [Next: Smart Fields](/guides/migration/from-v1/steps/smart-fields)
- [Process > Actions > Node.js](/product/process/actions/coding-actions/nodejs)
