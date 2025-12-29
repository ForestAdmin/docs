---
title: Legacy agents - overview
---
> **Status:** LEGACY
>
> **Audience:** Legacy users migrating to new agents

## ⚠️ legacy notice

The following agents are **legacy** and no longer maintained:

- **forest-express-sequelize** (Node.js + Sequelize)
- **forest-express-mongoose** (Node.js + Mongoose)

### End of life

- **Support ends:** [Date TBD]
- **Security updates:** Critical issues only
- **New features:** None

### Why legacy?

These agents have been replaced by **@forestadmin/agent** which provides:
- ✅ Better performance
- ✅ TypeScript support
- ✅ Modern architecture
- ✅ Multi-datasource support
- ✅ Active development & support

## Migration required

**You should migrate to @forestadmin/agent as soon as possible.**

### Migration resources

- **[Complete Migration Guide](/guides/migration/from-v1/overview)**
- **[Step-by-step Instructions](/guides/migration/from-v1/steps/datasources)**
- **[Node.js Setup](/product/integration/setup/nodejs)**

### Migration support

- 📖 Documentation: Full migration guide available
- 💬 Community Forum: [link]
- 📧 Support: support@forestadmin.com
- 🎫 Enterprise: Dedicated migration assistance available

## Legacy documentation

Reference documentation for legacy agents is maintained for migration purposes only:

- **[forest-express-sequelize](/reference/agent-api/deprecated/forest-express-sequelize)**
- **[forest-express-mongoose](/reference/agent-api/deprecated/forest-express-mongoose)**

⚠️ **Do not use for new projects!**

## Comparison: Old vs new

| Feature | Legacy Agents | @forestadmin/agent |
|---------|-------------------|-------------------|
| TypeScript | ❌ No | ✅ Yes |
| Multi-datasource | ❌ No | ✅ Yes |
| Performance | ⚠️ OK | ✅ Excellent |
| Architecture | ⚠️ Legacy | ✅ Modern |
| Active Development | ❌ No | ✅ Yes |
| New Features | ❌ No | ✅ Yes |
| Security Updates | ⚠️ Critical only | ✅ Regular |
| Support | ❌ Legacy | ✅ Full support |

## Breaking changes

Key differences when migrating:

1. **Initialization** - New API syntax
2. **Smart Actions** - New response format
3. **Smart Fields** - Better async support
4. **Hooks** - New hook system
5. **Configuration** - Environment variable changes
6. **Relationships** - Improved definition syntax

See [Migration Guide](/guides/migration/from-v1/overview) for complete details.

## Timeline

- **Now:** Limited support (critical security only)
- **[Date]:** End of support
- **Action Required:** Migrate before EOL

## Getting help

### Migration questions
- 📖 [Migration Guide](/guides/migration/from-v1/overview)
- 💬 Community Forum
- 📧 support@forestadmin.com

### Enterprise support
Enterprise customers can request dedicated migration assistance.

---

**⚠️ Important:** This documentation is for **legacy reference only**. For current documentation, see:
- [Node.js Agent API Reference](/reference/agent-api/nodejs)
- [Integration > Setup > Node.js](/product/integration/setup/nodejs)
