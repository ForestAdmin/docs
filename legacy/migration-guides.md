---
title: Migration Guides
description: Step-by-step guides to migrate from Agent v1 to Agent v2
---

> **Migration is easier than you think!**
>
> Most teams complete migration in 1-2 days. You can run v1 and v2 in parallel with zero downtime.

## Choose Your Migration Path

Select your current agent to see the specific migration guide:



  * [forest-express-sequelize](/guides/migration/from-v1/steps/datasources.md) - Migrate from Node.js Sequelize agent
  * [forest-express-mongoose](/guides/migration/from-v1/steps/datasources.md) - Migrate from Node.js Mongoose agent
  * [forest-rails](/guides/migration/from-v1/overview.md) - Migrate from Ruby on Rails agent
  * [django-forestadmin](/guides/migration/from-v1/overview.md) - Migrate from Python Django agent



## Migration Overview

### General Process

All migrations follow a similar process:

1. **Install Agent v2** alongside v1 (no conflict)
2. **Setup datasources** with new syntax
3. **Migrate customizations** one by one:
   - Smart Actions
   - Smart Fields
   - Smart Segments
   - Hooks
4. **Test in parallel** (run both agents)
5. **Switch traffic** to v2
6. **Remove v1** packages

### Zero-Downtime Migration

You can run v1 and v2 simultaneously:

```bash
# Terminal 1 - Run v1 on port 3000
npm run start:v1

# Terminal 2 - Run v2 on port 3001
npm run start:v2
```

Then switch your Forest Admin environment to point to the v2 agent when ready.

## Migration by Component

### Datasources

* [Migrate Datasources](/guides/migration/from-v1/steps/datasources.md) - Learn how to migrate your database connections to the new datasource system

### Smart Actions

* [Migrate Smart Actions](/guides/migration/from-v1/steps/smart-actions.md) - Convert your Smart Actions to the new Agent v2 API

### Smart Fields

Coming soon - Guide for migrating Smart Fields

### Smart Segments

Coming soon - Guide for migrating Smart Segments

### Hooks

Coming soon - Guide for migrating Hooks

## Common Migration Patterns

### Pattern 1: Simple CRUD Application

**Complexity:** Low
**Time:** 2-4 hours

Applications with:
- Basic CRUD operations
- Few or no Smart Actions
- No Smart Fields/Segments

**Steps:**
1. Install v2 packages
2. Copy datasource configuration
3. Test basic functionality
4. Deploy

### Pattern 2: Custom Business Logic

**Complexity:** Medium
**Time:** 1-2 days

Applications with:
- Multiple Smart Actions
- Some Smart Fields
- Basic hooks

**Steps:**
1. Install v2 packages
2. Migrate datasources
3. Migrate Smart Actions one by one
4. Migrate Smart Fields
5. Test thoroughly
6. Deploy

### Pattern 3: Complex Integrations

**Complexity:** High
**Time:** 3-5 days

Applications with:
- Many Smart Actions with complex logic
- Multiple Smart Fields and Segments
- Multiple hooks
- External API integrations

**Steps:**
1. Plan migration strategy
2. Set up v2 in parallel with v1
3. Migrate datasources
4. Migrate customizations incrementally
5. Test each component
6. Gradual rollout

## Migration Checklist

Use this checklist to track your migration progress:

### Pre-Migration
- [ ] Read the migration guide for your agent
- [ ] Review your current customizations (Smart Actions, Fields, Segments)
- [ ] Backup your project
- [ ] Set up a test environment

### Installation
- [ ] Install Agent v2 packages
- [ ] Keep v1 packages (for parallel running)
- [ ] Update package.json scripts

### Datasources
- [ ] Migrate database connection
- [ ] Test database connectivity
- [ ] Verify schema generation

### Customizations
- [ ] List all Smart Actions
- [ ] List all Smart Fields
- [ ] List all Smart Segments
- [ ] List all Hooks
- [ ] Migrate each customization
- [ ] Test each feature

### Testing
- [ ] Test in development
- [ ] Test in staging
- [ ] Verify all Smart Actions work
- [ ] Verify all Smart Fields work
- [ ] Verify permissions
- [ ] Load testing

### Deployment
- [ ] Deploy v2 agent
- [ ] Run v1 and v2 in parallel
- [ ] Switch environment to v2
- [ ] Monitor for issues
- [ ] Remove v1 code

## Getting Help

### Resources

- **[Complete Migration Guide](/guides/migration/from-v1/overview)** - Detailed step-by-step instructions
- **[Agent v2 API Reference](/reference/agent-api/overview)** - New API documentation
- **[Examples Repository](https://github.com/ForestAdmin)** - Code examples

### Support Channels



  * [Email Support](mailto:support@forestadmin.com.md) - Get help from our team
  * [Community](https://community.forestadmin.com) - Ask the community
  * [Documentation](/guides/migration/from-v1/overview.md) - Read the full guide



## Migration Timeline

We recommend the following timeline:

| Week | Tasks |
|------|-------|
| **Week 1** | Read guides, plan migration, set up v2 in parallel |
| **Week 2** | Migrate datasources and core customizations |
| **Week 3** | Test thoroughly, fix issues |
| **Week 4** | Deploy to staging, final testing |
| **Week 5** | Deploy to production, monitor |

## FAQs

### Do I need to migrate everything at once?
No! You can migrate incrementally. Run v1 and v2 in parallel and switch when ready.

### Will my UI break during migration?
No. Your Forest Admin UI configuration is preserved. The agent change is transparent to end-users.

### Can I revert if something goes wrong?
Yes! Keep v1 running until you're confident with v2. You can switch back instantly.

### What about my data?
Your database is not affected. Only the agent code changes.

### Are there breaking changes?
Yes, the API is different, but concepts are the same. Most changes are syntax updates.

---

**Ready to migrate?** Start with the [complete migration guide](/guides/migration/from-v1/overview).
