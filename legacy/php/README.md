---
title: PHP Agent Documentation (End of Life)
---

# PHP Agent Documentation (End of Life)

> **Status:** END OF LIFE
>
> **Audience:** Legacy PHP users

## End of Life Notice

**The Forest Admin PHP agent has reached End of Life and is no longer supported.**

- ❌ No security updates
- ❌ No bug fixes
- ❌ No new features
- ❌ No technical support

## Recommended Alternatives

We recommend transitioning to one of our actively supported agents:

1. **Node.js Agent** (Recommended)
   - [Get Started with Node.js](/product/integration/setup/nodejs)
   - Modern, actively maintained
   - Full feature support
   - TypeScript support

2. **Ruby Agent**
   - [Get Started with Ruby](/product/integration/setup/ruby)
   - Active development
   - Rails integration

3. **Python Agent**
   - [Get Started with Python](/product/integration/setup/python)
   - Django/Flask support

## Migration Options

### Option 1: Migrate to Node.js (Recommended)

If you have a mixed stack or can add a Node.js service:
- Deploy Node.js agent as standalone service
- Connect to your existing database
- No changes to your PHP application needed

### Option 2: Maintain PHP Application, Use Node.js for Admin

- Keep your PHP application as-is
- Deploy separate Node.js service for Forest Admin
- Both connect to same database
- Forest Admin traffic goes through Node.js agent

### Option 3: Fully Migrate to Supported Platform

Consider migrating your entire admin/backoffice to a supported stack.

## Legacy PHP Documentation

The PHP agent documentation is archived for historical reference only:

### Source Repository Location

- **Repository:** `documentation-agent-v1/` (PHP-specific sections)
- **Status:** Archived, read-only
- **Last Updated:** [Date]

### What Was Supported

The PHP agent provided:
- Laravel integration
- Symfony integration
- Doctrine ORM support
- Eloquent ORM support
- Basic CRUD operations
- Smart Features (limited)

### Why PHP Agent Was Discontinued

- Limited community adoption
- Maintenance burden
- PHP ecosystem challenges
- Focus on actively used platforms

## Getting Help

For migration questions:
- 📧 Contact: support@forestadmin.com
- 💬 Community Forum: [link]
- 📞 Sales (for enterprise migrations): [link]

## Technical Support

While we cannot provide support for the PHP agent itself, we can help you:
- Plan your migration
- Choose the right alternative
- Connect your existing database to a new agent
- Architecture consultation (enterprise plans)

---

**⚠️ Critical:** Do not use the PHP agent for new projects. For current documentation, see [Get Started](/get-started/introduction).
