---
title: Legacy agents overview
description: Overview of deprecated Forest Admin agents and why you should migrate
---

> **⚠️ DEPRECATION NOTICE**
>
> All Agent v1 packages are deprecated and reached End-of-Support in December 2024.
>
> **We strongly recommend migrating to Agent v2 immediately.**

## Deprecated agents

The following agents are no longer supported and will not receive security updates or bug fixes:

### JavaScript agents
- **forest-express-sequelize** v9 - Node.js agent with Sequelize ORM
- **forest-express-mongoose** v9 - Node.js agent with Mongoose ODM

### Ruby agent
- **forest_liana** (forest-rails) - Ruby on Rails agent v1

### Python agent
- **django-forestadmin** v1 - Python Django agent (End-of-Life soon)

## Why you must migrate now

### Security & maintenance
- ❌ **No security patches** - Your application is vulnerable to security issues
- ❌ **No bug fixes** - Issues will not be resolved
- ❌ **No support** - Our team cannot help with v1 issues
- ❌ **Technical debt** - The gap between v1 and v2 grows every day

### Agent v2 benefits

#### Better performance
- **3x faster query execution** - Optimized query engine
- **Reduced memory footprint** - More efficient resource usage
- **Lazy loading** - Load data only when needed
- **Query optimization** - Automatic query batching and caching

#### Modern features
- ✅ **Native TypeScript support** (Node.js) - Full type safety and autocomplete
- ✅ **Better plugin system** - Easier customization and extension
- ✅ **Cross-datasource relationships** - Join data from multiple sources
- ✅ **Improved error handling** - Better debugging experience
- ✅ **Cloud deployment support** - Deploy with Forest Admin Cloud
- ✅ **Better hooks system** - More control over data operations

#### Developer experience
- **Cleaner API** - More intuitive and consistent
- **Better documentation** - Comprehensive guides and examples
- **Active development** - Regular updates and new features
- **Modern tooling** - Works with latest frameworks and tools
- **Better testing** - Easier to test your customizations

#### Business impact
- **Faster time to market** - Build features faster
- **Lower maintenance costs** - Less time fixing bugs
- **Better user experience** - Faster and more reliable
- **Future-proof** - Ready for upcoming features

## Migration effort

| Agent | Estimated Time | Complexity | Breaking Changes |
|-------|---------------|------------|------------------|
| forest-express-sequelize | 2-4 hours | Low | Minimal |
| forest-express-mongoose | 2-4 hours | Low | Minimal |
| forest-rails | 4-8 hours | Medium | Some API changes |
| django-forestadmin | 4-8 hours | Medium | Some API changes |

**Note:** These estimates are for typical projects. Complex customizations may require more time.

## What changes in v2?

### Core concepts (same)
✅ Collections, fields, and relationships work the same way
✅ Smart Actions, Smart Fields, Smart Segments stay conceptual similar
✅ Permissions and roles configuration unchanged in UI
✅ Your Forest Admin UI configuration is preserved

### API changes (different)
- **Package names** - New package structure (`@forestadmin/agent`, `@forestadmin/datasource-*`)
- **Initialization** - New agent setup with datasource composition
- **Customization API** - New methods for Smart Actions, Fields, etc.
- **Hooks** - New hook system with better typing

### Migration path

1. **Install new agent** - Add v2 packages alongside v1
2. **Copy datasource connection** - Reuse your database connection
3. **Migrate customizations** - Update Smart Actions, Fields, Segments one by one
4. **Test in parallel** - Run v1 and v2 side-by-side
5. **Switch to v2** - Update your environment variables
6. **Remove v1** - Uninstall v1 packages

**The good news:** You can migrate incrementally without downtime!

## Migration support

We're here to help you migrate:

### Resources
- 📚 **[Detailed Migration Guides](/guides/migration/from-v1/overview)** - Step-by-step instructions
- 🎥 **Video tutorials** - Watch how to migrate
- 💬 **Community support** - Ask questions in our community
- 📧 **Email support** - Contact support@forestadmin.com

### We help you
- ✅ Review your migration plan
- ✅ Answer technical questions
- ✅ Debug migration issues
- ✅ Provide code examples

## Start migration today

Don't wait! The longer you stay on v1, the harder migration becomes.



  * [Migration Guide](/guides/migration/from-v1/overview.md) - Complete step-by-step migration guide
  * [What's New in v2](/reference/agent-api/overview.md) - Discover all v2 features
  * [Get Help](mailto:support@forestadmin.com.md) - Contact our team for migration support
  * [Community](https://community.forestadmin.com) - Ask questions to the community



## Faqs

### Can i run v1 and v2 in parallel?
Yes! This is the recommended approach. You can run both agents on different ports during migration.

### Will my UI configuration be preserved?
Yes! All your layout, permissions, and settings are preserved.

### How long does migration take?
Most teams complete migration in 1-2 days for simple projects, up to 1 week for complex ones.

### What if i have many smart actions?
Migrate them one by one. The new API is similar but more powerful.

### Do i need to change my database?
No! Your database structure stays the same.

### Is there a migration tool?
We provide guides and code examples. Some parts can be automated, others require manual updates.

---

**⚠️ Critical:** Security vulnerabilities in v1 will NOT be patched. Migrate to v2 to keep your application secure.
