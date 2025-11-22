# Forest Admin Documentation

[![Mintlify](https://img.shields.io/badge/Built%20with-Mintlify-00D4AA)](https://mintlify.com)

The official documentation for Forest Admin - the admin panel framework that adapts to your data.

## 🚀 Quick Start

### Local Development

```bash
# Install Mintlify CLI
npm install -g mintlify

# Run local server
mintlify dev

# Open http://localhost:3000
```

### Project Structure

```
forestadmin-documentation/
├── mint.json                 # Mintlify configuration
├── get-started/             # Getting started guides
├── product/                 # Product documentation
│   ├── integration/        # Layer 1: Connect data
│   ├── control/            # Layer 2: Access & security
│   ├── process/            # Layer 3: Business logic
│   ├── build/              # Operations: Build UI
│   ├── execute/            # Operations: Execute tasks
│   ├── collaborate/        # Operations: Team collaboration
│   ├── manage/             # Operations: Analytics
│   └── embed/              # Operations: Embedding
├── reference/              # Technical reference
│   ├── api/               # Public API reference
│   ├── cli/               # CLI commands
│   ├── schema/            # Schema reference
│   └── agent-api/         # Agent API reference
├── guides/                 # How-to guides
│   ├── deployment/        # Deployment guides
│   ├── migration/         # Migration guides
│   ├── best-practices/    # Best practices
│   └── integrations/      # Third-party integrations
└── legacy/                 # Deprecated documentation
```

## 📚 Documentation Architecture

Forest Admin documentation follows a **4-layer architecture** + **5 operations categories**:

### The 4 Layers

1. **Integration** - Connect your data sources
2. **Control** - Manage access, roles, and security
3. **Process** - Define business logic (actions, fields, segments, hooks)
4. **Operations** - Execute daily tasks

### The 5 Operations

- **Build** - Configure your admin interface
- **Execute** - Perform operations on data
- **Collaborate** - Team workflows and communication
- **Manage** - Analytics and monitoring
- **Embed** - Integrate Forest Admin into your apps

## 🎯 Content Organization

### Guide vs Reference

- **Guides** (product/, guides/) - Learn concepts, use cases, patterns
- **Reference** (reference/) - Technical API docs, exact syntax

### Framework Priority

1. **Node.js** (@forestadmin/agent) - PRIORITY 1
2. **Ruby** (forest_liana) - PRIORITY 2
3. **Python** (forestadmin-agent-python) - PRIORITY 3
4. **Cloud** - ROADMAP features

## 📝 Contributing

### Adding New Content

1. Find the appropriate placeholder file
2. Follow the content outline provided
3. Maintain cross-references
4. Include code examples

### File Structure

Each page includes:
- Status and priority
- Target audience
- Description
- Sources to migrate (from old docs)
- Content outline
- Migration notes
- Related pages

### Style Guide

- Use GitHub-flavored markdown
- Code blocks with language tags
- Clear headings hierarchy
- Internal links with relative paths
- External links with full URLs

## 🔄 Migration Status

**Current Status:** 157 placeholder files created

**Source Repositories:**
- documentation-user-guide (65 files)
- documentation-agent-v1 (174 files)
- developer-guide-agent-v2 (607 files)
- documentation-cloud (55 files)
- public-api-guide (8 files)

**Migration Progress:**
- [ ] Phase 1: HIGH priority content (Week 1)
- [ ] Phase 2: MEDIUM priority content (Week 2-3)
- [ ] Phase 3: LOW priority content (Week 4+)
- [ ] Phase 4: Third-party integrations (31 guides)

See [PLACEHOLDER-SUMMARY.md](./PLACEHOLDER-SUMMARY) for detailed status.

## 🏗️ Architecture Decisions

See [DECISIONS.md](./DECISIONS) for all architectural decisions and rationale.

Key decisions:
- Feature-based Process structure (Option B)
- Framework priority (Node.js > Ruby > Python)
- DRY principle (shared concepts + specific implementations)
- Guide vs Reference separation
- Agent API in Reference section

## 📦 Deployment

### Mintlify Cloud

Connected to this repository. Automatically deploys on push to main.

### Custom Domain

[To be configured]

## 🔗 Links

- **Live Documentation:** [To be configured]
- **Forest Admin Website:** https://www.forestadmin.com
- **Community Forum:** [link]
- **GitHub Issues:** [link]

## 📄 License

[To be determined]

---

**Built with ❤️ by the Forest Admin team**
