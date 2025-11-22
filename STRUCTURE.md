# Documentation Structure Overview

Quick reference for the new Forest Admin documentation architecture.

## Visual Structure

```
new-documentation/
│
├── 📄 README.md                        Main documentation
├── 📄 DECISIONS.md                     Decision log
├── 📄 STRUCTURE.md                     This file
│
├── 🚀 get-started/                     TAB 1: Get Started
│   ├── introduction.md                 ← 4-layer intro (marketing/education)
│   ├── quickstart-cloud.md
│   ├── quickstart-self-hosted.md
│   └── core-concepts.md
│
├── 📦 product/                         TAB 2: Product
│   │
│   ├── integration/                    LAYER 1: Connect data
│   │   ├── overview.md
│   │   ├── data-sources/
│   │   │   ├── sql.md
│   │   │   ├── mongodb.md
│   │   │   ├── rest-api.md
│   │   │   └── custom.md
│   │   ├── architectures/
│   │   │   ├── self-hosted.md         ← PROMINENT
│   │   │   ├── cloud.md
│   │   │   └── comparison.md
│   │   └── setup/
│   │       ├── nodejs.md              ← PRIORITY 1
│   │       ├── ruby.md                ← PRIORITY 2
│   │       └── python.md              ← PRIORITY 3
│   │
│   ├── control/                        LAYER 2: Security & compliance
│   │   ├── overview.md
│   │   ├── roles-permissions.md
│   │   ├── authentication/
│   │   │   ├── overview.md
│   │   │   ├── sso.md
│   │   │   ├── 2fa.md
│   │   │   └── scim.md
│   │   ├── security.md
│   │   └── audit.md
│   │
│   ├── process/                        LAYER 3: Business logic
│   │   ├── overview.md
│   │   ├── for-ops-teams/             [ROADMAP]
│   │   │   ├── overview.md
│   │   │   ├── nocode-actions.md
│   │   │   ├── mcp-connections.md
│   │   │   └── process-builder.md
│   │   └── for-tech-teams/
│   │       ├── overview.md
│   │       ├── smart-actions/
│   │       │   ├── overview.md        ← Shared concepts
│   │       │   ├── cloud.md
│   │       │   ├── nodejs.md
│   │       │   ├── ruby.md
│   │       │   └── python.md
│   │       ├── smart-fields/
│   │       │   └── [similar structure]
│   │       ├── smart-segments/
│   │       │   └── [similar structure]
│   │       └── hooks.md
│   │
│   ├── build/                          OPERATIONS: Build
│   │   ├── overview.md
│   │   ├── workspaces.md              ← High priority
│   │   ├── layout-editor.md
│   │   └── custom-views.md
│   │
│   ├── execute/                        OPERATIONS: Execute
│   │   ├── overview.md
│   │   ├── actions.md
│   │   ├── workflows.md
│   │   └── browse.md
│   │
│   ├── collaborate/                    OPERATIONS: Collaborate
│   │   ├── overview.md
│   │   ├── inbox.md
│   │   ├── approval.md
│   │   ├── notes.md
│   │   └── escalation.md
│   │
│   ├── manage/                         OPERATIONS: Manage
│   │   ├── overview.md
│   │   ├── dashboards.md
│   │   ├── activity.md
│   │   └── team-performance.md
│   │
│   └── embed/                          OPERATIONS: Embed
│       ├── overview.md
│       └── assist.md                   [ROADMAP]
│
├── 📚 reference/                       TAB 3: Reference
│   ├── api/
│   │   ├── introduction.md
│   │   ├── authentication.md
│   │   ├── rate-limits.md
│   │   └── endpoints/
│   │       ├── activity-logs.md
│   │       ├── admin-logs.md
│   │       └── notes.md
│   │
│   ├── cli/
│   │   ├── overview.md
│   │   ├── init.md
│   │   ├── login.md
│   │   ├── branch.md
│   │   ├── switch.md
│   │   ├── push.md
│   │   └── deploy.md
│   │
│   └── schema/
│       └── forestadmin-schema.md
│
├── 📖 guides/                          TAB 4: Guides
│   ├── integrations/
│   │   ├── overview.md
│   │   ├── stripe.md
│   │   ├── slack.md
│   │   ├── metabase.md
│   │   ├── intercom.md
│   │   └── sso-providers/
│   │       ├── azure.md
│   │       ├── okta.md
│   │       ├── google.md
│   │       └── aws.md
│   │
│   ├── deployment/
│   │   ├── overview.md
│   │   ├── development-workflow.md
│   │   ├── environments.md
│   │   ├── aws.md
│   │   ├── heroku.md
│   │   ├── gcp.md
│   │   └── self-hosted.md
│   │
│   ├── migration/
│   │   ├── overview.md
│   │   ├── from-v1.md
│   │   └── steps/
│   │       ├── datasources.md
│   │       ├── smart-actions.md
│   │       ├── smart-fields.md
│   │       └── smart-relationships.md
│   │
│   └── best-practices/
│       ├── performance.md
│       ├── security.md
│       └── troubleshooting.md
│
└── ⚠️  legacy/                         TAB 5: Legacy
    ├── overview.md                     ← Deprecation notice
    ├── agent-v1/
    │   └── README.md
    └── php/
        └── README.md
```

## Navigation Mapping

### Tab 1: Get Started
**For:** All new users
**Goal:** Quick onboarding

```
Get Started
├─ Introduction (4 layers explanation)
├─ Quickstart Cloud
├─ Quickstart Self-hosted
└─ Core Concepts
```

---

### Tab 2: Product
**For:** All users (learning & using Forest Admin)
**Goal:** Complete product documentation

```
Product

├─ Integration (Layer 1)
│  ├─ Overview
│  ├─ Data Sources
│  ├─ Architectures
│  └─ Setup

├─ Control (Layer 2)
│  ├─ Overview
│  ├─ Roles & Permissions
│  ├─ Authentication
│  ├─ Security
│  └─ Audit

├─ Process (Layer 3)
│  ├─ Overview
│  ├─ For Ops Teams (No-code)
│  └─ For Tech Teams (Code)

├─ Build (Operations)
│  ├─ Overview
│  ├─ Workspaces
│  ├─ Layout Editor
│  └─ Custom Views

├─ Execute (Operations)
│  ├─ Overview
│  ├─ Actions
│  ├─ Workflows
│  └─ Browse

├─ Collaborate (Operations)
│  ├─ Overview
│  ├─ Inbox
│  ├─ Approval
│  ├─ Notes
│  └─ Escalation

├─ Manage (Operations)
│  ├─ Overview
│  ├─ Dashboards
│  ├─ Activity
│  └─ Team Performance

└─ Embed (Operations)
   ├─ Overview
   └─ Assist
```

---

### Tab 3: Reference
**For:** Developers needing technical specs
**Goal:** Quick API/CLI reference

```
Reference

├─ API Reference
│  ├─ Introduction
│  ├─ Authentication
│  ├─ Rate Limits
│  └─ Endpoints

├─ CLI Commands
│  ├─ Overview
│  ├─ forest init
│  ├─ forest login
│  ├─ forest branch
│  ├─ forest push
│  └─ forest deploy

└─ Schema Reference
   └─ .forestadmin-schema.json
```

---

### Tab 4: Guides
**For:** Users with specific cross-cutting tasks
**Goal:** Solve situational problems

```
Guides

├─ Integrations
│  ├─ Overview
│  ├─ Stripe
│  ├─ Slack
│  ├─ Metabase
│  └─ SSO Providers

├─ Deployment
│  ├─ Overview
│  ├─ Development Workflow
│  ├─ Environments
│  ├─ AWS
│  ├─ Heroku
│  └─ GCP

├─ Migration
│  ├─ Overview
│  ├─ From v1 to v2
│  └─ Steps

└─ Best Practices
   ├─ Performance
   ├─ Security
   └─ Troubleshooting
```

---

### Tab 5: Legacy
**For:** Users on deprecated versions
**Goal:** Reference only, encourage migration

```
Legacy

├─ Overview (deprecation notice)
├─ Agent v1 (Deprecated)
└─ PHP Agent (End-of-Life)
```

---

## File Count Summary

**Created:**
- ✅ 1 README.md (comprehensive)
- ✅ 1 DECISIONS.md (decision log)
- ✅ 1 STRUCTURE.md (this file)
- ✅ ~50 folder structure
- ✅ ~15 placeholder .md files

**Total:** ~70 files/folders created

**Next:** Migrate ~909 source files → ~600-700 unique files in this structure

---

## Priority Content to Migrate First

### Tier 1 (Week 1) - Essential
- ✅ Get Started pages (NEW content)
- ⏳ Integration > Setup (Node.js, Ruby, Python)
- ⏳ Reference > API (8 files from public-api-guide)
- ⏳ Operations > Build > Workspaces
- ⏳ Process > Smart Actions (overview + Node.js)

### Tier 2 (Week 2-3) - Core Features
- ⏳ All Operations categories (complete)
- ⏳ Integration (complete)
- ⏳ Control (complete)
- ⏳ Process > Smart Fields, Segments (all frameworks)

### Tier 3 (Week 3-4) - Advanced
- ⏳ Guides > Integrations
- ⏳ Guides > Migration
- ⏳ Guides > Deployment
- ⏳ Reference > CLI

### Tier 4 (Week 4+) - Archive
- ⏳ Legacy > Agent v1
- ⏳ Legacy > PHP

---

## Key Decisions Recap

1. **5 tabs:** Get Started, Product, Reference, Guides, Legacy
2. **4 layers visible:** Integration, Control, Process, Operations (5 categories)
3. **Self-hosted > Cloud** (prominence)
4. **Node.js > Ruby > Python** (priority)
5. **PHP = deprecated** (moved to Legacy)
6. **DRY principle:** Shared concepts + framework-specific implementations
7. **Tool-agnostic:** Plain .md files (Mintlify decision deferred)
8. **Cross-references OK:** Don't force pure layer separation

---

**Created:** 2025-10-24
**Status:** Ready for content migration
**Next:** Begin Tier 1 migration
