---
title: Forest Admin Documentation - New Architecture
---

# Forest Admin Documentation - New Architecture

This directory contains the new, unified documentation structure for Forest Admin, consolidating content from 5 previously separate repositories.

## 📋 Table of Contents

1. [Overview](#overview)
2. [Architecture Philosophy](#architecture-philosophy)
3. [Structure](#structure)
4. [Key Decisions](#key-decisions)
5. [Migration Strategy](#migration-strategy)
6. [Source Repositories](#source-repositories)
7. [Priorities](#priorities)

---

## Overview

This new documentation architecture is designed to:

- **Unify** 5 scattered documentation repositories (~909 files) into one coherent structure
- **Educate** users about Forest Admin's 4-layer architecture (marketing/product positioning)
- **Optimize** for usability with clear navigation paths
- **Eliminate** redundancy while preserving framework-specific content where needed
- **Sunset** deprecated content (Agent v1, PHP) gracefully

### Current State (Before)

| Repository | Files | Audience | Status |
|------------|-------|----------|--------|
| documentation-user-guide | 65 | Non-tech/Ops | Active |
| documentation-agent-v1 | 174 | Developers | Deprecated |
| documentation-cloud | 55 | Cloud users | Active |
| developer-guide-agent-v2 | 607 | Developers | Active |
| public-api-guide | 8 | Developers | Active |
| **TOTAL** | **909** | Mixed | Mixed |

### New State (After)

One unified documentation with:
- Clear 4-layer product architecture
- 5-tab navigation (Get Started, Product, Reference, Guides, Legacy)
- Framework-specific content preserved (Node.js, Ruby, Python)
- PHP sunset, Agent v1 archived
- **Estimated ~600-700 unique files** after deduplication

---

## Architecture Philosophy

### The 4-Layer Model

Forest Admin is built on **4 foundational layers**:

```
┌─────────────────────────────────────────────────────────────┐
│  OPERATIONS - Give teams tools for day-to-day ops           │
│  ├─ Build: Workspaces, Layout Editor, Custom Views         │
│  ├─ Execute: Actions, Workflows, Browse                     │
│  ├─ Collaborate: Inbox, Approval, Notes, Escalation        │
│  ├─ Manage: Dashboards, Activity, Team Performance         │
│  └─ Embed: Assist, MCP Server                              │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│  PROCESS - Codify business logic                            │
│  ├─ For Ops Teams: No-code actions, Process builder, MCP   │
│  └─ For Tech Teams: Smart actions, fields, segments        │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│  CONTROL - Keep ops secure and compliant                    │
│  ├─ Roles & Permissions                                     │
│  ├─ Authentication (SSO, 2FA, SCIM)                         │
│  ├─ Security                                                │
│  └─ Audit                                                   │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│  INTEGRATION - Connect any data, wherever it lives          │
│  ├─ Data Sources (SQL, MongoDB, REST, Custom)              │
│  ├─ Architectures (Cloud, Self-hosted, Hybrid)             │
│  └─ Setup (Node.js, Python, Ruby)                          │
└─────────────────────────────────────────────────────────────┘
```

### Why This Structure?

**For Marketing & Education:**
- Clear product positioning around 4 layers
- Easy to pitch: "Forest Admin connects your data, secures it, codifies processes, and empowers operations"
- Differentiates Forest Admin from competitors

**For Users:**
- Natural progression: Connect data → Secure → Define processes → Execute daily ops
- Clear separation of concerns
- Easy to find what you need

**For Content Organization:**
- Reduces duplication (concepts explained once per layer)
- Allows framework-specific content where needed
- Handles cross-cutting concerns (integrations, deployment) separately

---

## Structure

### Top-Level Navigation (5 Tabs)

```
┌─────────────┬─────────┬───────────┬────────┬────────┐
│ Get Started │ Product │ Reference │ Guides │ Legacy │
└─────────────┴─────────┴───────────┴────────┴────────┘
```

#### Tab 1: Get Started
**Audience:** All new users (technical and non-technical)

**Content:**
- Introduction (presents the 4 layers)
- Quickstart Cloud
- Quickstart Self-hosted (Node.js, Ruby, Python)
- Core Concepts

**Goal:** Onboard users quickly with the minimum knowledge needed

---

#### Tab 2: Product
**Audience:** All users learning about and using Forest Admin

**Content:** The 4 layers + 5 Operations categories

**Sidebar structure:**
```
Integration (Layer 1)
├─ Overview
├─ Data Sources
│  ├─ SQL
│  ├─ MongoDB
│  ├─ REST API
│  └─ Custom
├─ Architectures
│  ├─ Self-hosted (PROMINENT)
│  ├─ Cloud
│  └─ Comparison
└─ Setup
   ├─ Node.js (PRIORITY 1)
   ├─ Ruby (PRIORITY 2)
   └─ Python (PRIORITY 3)

Control (Layer 2)
├─ Overview
├─ Roles & Permissions
├─ Authentication
│  ├─ SSO
│  ├─ 2FA
│  └─ SCIM
├─ Security
└─ Audit

Process (Layer 3)
├─ Overview
├─ For Ops Teams (No-code) [ROADMAP]
│  ├─ No-code Actions
│  ├─ MCP Connections
│  └─ Process Builder
└─ For Tech Teams (Code)
   ├─ Smart Actions
   │  ├─ Overview (shared concepts)
   │  ├─ Cloud
   │  ├─ Node.js
   │  ├─ Ruby
   │  └─ Python
   ├─ Smart Fields
   │  ├─ Overview
   │  ├─ Cloud
   │  ├─ Node.js
   │  ├─ Ruby
   │  └─ Python
   ├─ Smart Segments
   │  └─ [similar structure]
   └─ Hooks

Build (Operations Category 1)
├─ Overview
├─ Workspaces
├─ Layout Editor
└─ Custom Views

Execute (Operations Category 2)
├─ Overview
├─ Actions
├─ Workflows
└─ Browse

Collaborate (Operations Category 3)
├─ Overview
├─ Inbox
├─ Approval
├─ Notes
└─ Escalation

Manage (Operations Category 4)
├─ Overview
├─ Dashboards
├─ Activity
└─ Team Performance

Embed (Operations Category 5)
├─ Overview
└─ Assist (MCP)
```

---

#### Tab 3: Reference
**Audience:** Developers needing technical specifications

**Content:**
```
API Reference
├─ Introduction
├─ Authentication
├─ Rate Limits
└─ Endpoints
   ├─ Activity Logs
   ├─ Admin Logs
   └─ Notes

CLI Commands
├─ Overview
├─ forest init
├─ forest login
├─ forest branch
├─ forest switch
├─ forest push
└─ forest deploy

Schema Reference
└─ .forestadmin-schema.json
```

---

#### Tab 4: Guides
**Audience:** Users with specific cross-cutting tasks

**Content:**
```
Integrations (Third-party services)
├─ Overview
├─ Stripe
├─ Slack
├─ Metabase
├─ Intercom
└─ SSO Providers
   ├─ Azure AD
   ├─ Okta
   ├─ Google
   └─ AWS IAM

Deployment (Infrastructure & DevOps)
├─ Overview
├─ Development Workflow
├─ Environments
├─ AWS
├─ Heroku
├─ GCP
└─ Self-hosted

Migration (Version upgrades)
├─ Overview
├─ From v1 to v2
└─ Steps
   ├─ Datasources
   ├─ Smart Actions
   ├─ Smart Fields
   └─ Smart Relationships

Best Practices
├─ Performance
├─ Security
└─ Troubleshooting
```

---

#### Tab 5: Legacy
**Audience:** Users on deprecated versions (for reference only)

**Content:**
```
Legacy Documentation
├─ Overview (deprecation notice)
├─ Agent v1 (Deprecated)
│  └─ Links to archived v1 docs
└─ PHP Agent (End-of-Life)
   └─ EOL notice + migration path
```

**Note:** This content is:
- Not indexed by search engines (`noindex`)
- Excluded from internal search
- Visible but clearly marked as deprecated
- Contains migration paths to current versions

---

## Key Decisions

### 1. Architecture: Hybrid Navigation (Option A)

**Decision:** Use 4-layer model for conceptual clarity, but flatten Operations into 5 direct categories

**Rationale:**
- Integration, Control, Process are explicit layers (educational value)
- Operations categories (Build, Execute, Collaborate, Manage, Embed) are direct for usability
- Avoids deep nesting (Operations > Build > Workspaces = 3 levels)
- Best of both worlds: clear architecture + practical navigation

**Alternatives considered:**
- Pure 4-layer (Operations as folder) - too many levels
- Pure flat (9 top-level groups) - loses conceptual clarity

---

### 2. Tabs Order: Get Started → Product → Reference → Guides → Legacy

**Decision:** Reference before Guides

**Rationale:**
- Developers frequently need API/CLI reference
- Guides are more situational (deployment, migration)
- Standard pattern: Stripe, Supabase follow similar order

---

### 3. Legacy as Separate Tab

**Decision:** Legacy is a visible 5th tab (not hidden in footer or Reference)

**Rationale:**
- ~250 deprecated files (Agent v1 + PHP) still referenced by existing users
- Clear separation prevents confusion
- Easy to deprecate/remove entire tab in future
- Visible but not prominent (last position)

**Alternatives considered:**
- Hide in footer - harder to find for users who need it
- Put in Reference - clutters technical documentation

---

### 4. PHP Sunset Strategy

**Decision:** Complete deprecation - content moved to Legacy tab with EOL notice

**Rationale:**
- PHP agent is fully deprecated (confirmed by user)
- No new features or bug fixes
- Moving to Legacy makes status crystal clear
- Provides migration path to Node.js/Python

**Impact:**
- ~152 PHP files (25% of agent-v2) archived
- Agent v2 content reduced by 25%

---

### 5. Framework Priority: Node.js > Ruby > Python

**Decision:** Prioritize Node.js content, followed by Ruby, then Python

**Rationale:**
- Node.js is most popular (user confirmation)
- Ruby second
- Python third
- Affects documentation order, example priority, and migration sequencing

**Implementation:**
- Setup pages listed in priority order
- Code examples show Node.js first
- Migration focuses on Node.js completion before other frameworks

---

### 6. Self-hosted vs Cloud Prominence

**Decision:** Self-hosted is more prominent than Cloud

**Rationale:**
- Self-hosted is majority deployment (user confirmation)
- Affects architecture documentation order
- Setup guides emphasize self-hosted path

---

### 7. Handling Cross-Cutting Concerns

**Problem:** Content like Workspaces, Smart Actions, Migrations, Deployment span multiple layers

**Decision:** Place content in primary layer, link to related content in other layers

**Examples:**
- **Workspaces:** Lives in Operations > Build (primary use is UI)
  - Links to Process > Smart Actions for code customization
  - Rationale: Users think "I want to build a workspace" (Operations), not "I want to code process logic"

- **Smart Actions:** Lives in Process > For Tech Teams (primary use is coding business logic)
  - Links to Operations > Execute for how to trigger actions in UI
  - Rationale: Focus is on coding, not UI usage

- **Migrations:** Separate section in Guides tab
  - Rationale: Migration content is meta-architectural, spans all layers, and is temporal (only needed during upgrade)

- **Deployment:** Separate section in Guides tab
  - Rationale: Infrastructure concerns are orthogonal to product layers

- **Integrations:** Separate section in Guides tab
  - Rationale: Third-party integrations combine Connection (Integration) + Business Logic (Process) in one guide

**Philosophy:** "It's okay if content mentions other layers. Don't force pure separation when concepts naturally overlap."

---

### 8. Redundancy Strategy

**Decision:** DRY (Don't Repeat Yourself) with shared concepts + framework-specific implementations

**Pattern:**
```
smart-actions/
├── overview.md          # SHARED: What, why, when, concepts
├── cloud.md             # Cloud-specific implementation
├── nodejs.md            # Node.js-specific implementation
├── ruby.md              # Ruby-specific implementation
└── python.md            # Python-specific implementation
```

**When concepts are identical across frameworks:** Use code tabs in a single page instead of separate pages

**Rationale:**
- Eliminates massive duplication (was a problem in old structure)
- Single source of truth for concepts
- Framework-specific pages only when behavior differs
- Reduces from 607 files (agent-v2) to ~400-500 unique files

---

### 9. No-code Features (Roadmap)

**Decision:** Include placeholders for Process > For Ops Teams (No-code actions, MCP connections, Process builder)

**Rationale:**
- Features are on product roadmap (coming soon)
- Shows complete vision of Process layer
- Placeholder pages marked with [ROADMAP] tag
- Better to have structure ready than retrofit later

---

### 10. Documentation Tool: TBD (Mintlify considered, not decided)

**Decision:** Create tool-agnostic markdown structure first

**Rationale:**
- Focus on content architecture, not tooling
- Pure `.md` files work with any documentation platform
- Mintlify considered (~$150-300/month acceptable budget) but decision deferred
- Can add `mint.json` or equivalent config later

**Requirements for future tool:**
- Support for tabs (5 top-level)
- Support for nested sidebar navigation
- Search functionality (with ability to exclude Legacy content)
- Code syntax highlighting with tabs (Node.js/Ruby/Python)
- Fast and professional appearance

---

## Migration Strategy

### Source Repositories Mapping

| Old Repository | Files | Maps To | Priority | Notes |
|----------------|-------|---------|----------|-------|
| **documentation-user-guide** | 65 | Operations (Build, Execute, Collaborate, Manage) + some Control | HIGH | Active, clean migration |
| **developer-guide-agent-v2** | 607 | Integration (Setup), Process (For Tech Teams), some Operations | HIGH | Most important, deduplicate across languages |
| **documentation-cloud** | 55 | Same structure as agent-v2 but Cloud-specific | MEDIUM | Significant overlap with agent-v2 |
| **public-api-guide** | 8 | Reference > API | HIGH | Quick, clean migration |
| **documentation-agent-v1** | 174 | Legacy > Agent v1 | LOW | Archive with deprecation notices |
| **PHP content** (from agent-v2) | ~152 | Legacy > PHP | LOW | Archive with EOL notice |

### Migration Phases

#### Phase 0: Foundation (COMPLETED)
- ✅ Create folder structure
- ✅ Create placeholder files
- ✅ Document decisions and architecture

#### Phase 1: High-Priority Content (Week 1)
**Goal:** Get essential documentation live quickly

1. **Get Started** (new content)
   - Introduction explaining 4 layers
   - Quickstart guides (Cloud + Self-hosted)
   - Core Concepts

2. **Reference > API** (8 files from public-api-guide)
   - Easiest migration, standalone content
   - High value for developers

3. **Operations categories** (65 files from user-guide)
   - Build: Workspaces, Layout Editor, Custom Views
   - Execute: Actions, Workflows, Browse
   - Collaborate: Inbox, Approval, Notes
   - Manage: Dashboards, Activity

#### Phase 2: Core Technical Content (Week 2-3)
**Goal:** Enable developers to build and customize

1. **Integration** (from agent-v2 + agent-v1)
   - Data Sources
   - Setup guides (Node.js → Ruby → Python priority)
   - Architecture comparison (Cloud vs Self-hosted)

2. **Control** (from user-guide + cloud + agent-v2)
   - Roles & Permissions
   - Authentication (SSO, SCIM, 2FA)
   - Security & Audit

3. **Process > For Tech Teams** (from agent-v2 + cloud)
   - Smart Actions (deduplicate, create shared overview)
   - Smart Fields (deduplicate)
   - Smart Segments (deduplicate)
   - Hooks

#### Phase 3: Cross-Cutting Guides (Week 3-4)
**Goal:** Support advanced use cases

1. **Guides > Integrations** (from agent-v1)
   - Stripe, Slack, Metabase, Intercom
   - SSO providers (Azure, Okta, Google, AWS)

2. **Guides > Deployment** (from agent-v2 + agent-v1)
   - Development workflow
   - AWS, Heroku, GCP deployments
   - Environment management

3. **Guides > Migration** (from agent-v2)
   - v1 to v2 migration guide
   - Step-by-step transformation docs

4. **Reference > CLI** (from agent-v2 + agent-v1)
   - All Forest CLI commands

#### Phase 4: Legacy & Cleanup (Week 4+)
**Goal:** Archive old content gracefully

1. **Legacy > Agent v1** (174 files)
   - Add deprecation notices
   - Link to v2 equivalents
   - Archive full v1 docs with search exclusion

2. **Legacy > PHP** (152 files)
   - EOL notice
   - Migration path to Node.js or Python
   - Minimal reference docs

3. **Final cleanup**
   - Fix broken links
   - Add redirects from old docs
   - SEO optimization
   - Search indexing

### Deduplication Strategy

**Problem:** Significant overlap between repositories:
- Actions documented in user-guide, agent-v1, agent-v2, cloud (4 places!)
- Charts in user-guide, agent-v1, agent-v2, cloud (4 places!)
- Fields, Collections, Segments similarly duplicated

**Solution:**
1. **Identify canonical source** (usually agent-v2 for code, user-guide for UI)
2. **Create shared overview** explaining concepts once
3. **Split by context:**
   - UI usage → Operations categories
   - Code customization → Process > For Tech Teams
   - Framework-specific → Separate pages (cloud.md, nodejs.md, etc.)
4. **Link between related pages** (e.g., Workspaces links to Smart Actions)

**Expected reduction:** ~909 files → ~600-700 unique files (30-40% reduction)

---

## Source Repositories

### Current Documentation Sources

All content being consolidated comes from these 5 repositories:

1. **documentation-user-guide**
   - URL: https://github.com/ForestAdmin/documentation-user-guide
   - Files: 65 markdown files
   - Focus: UI features for operations teams
   - Status: Active
   - Key content: Workspaces, Layout Editor, Actions UI, Dashboards, Collaboration tools

2. **developer-guide-agent-v2**
   - URL: https://github.com/ForestAdmin/developer-guide
   - Files: 607 markdown files (generates 4 variants: Node.js, PHP, Python, Ruby)
   - Focus: Agent v2 developer documentation
   - Status: Active (PHP deprecated)
   - Key content: Agent setup, Smart Actions/Fields/Segments, Data sources, Deployment

3. **documentation-cloud**
   - URL: https://github.com/ForestAdmin/documentation-cloud
   - Files: 55 markdown files
   - Focus: Cloud-specific documentation
   - Status: Active
   - Key content: Similar to agent-v2 but Cloud architecture, UI customization, Code customization

4. **public-api-guide**
   - URL: https://github.com/ForestAdmin/public-api-guide
   - Files: 8 markdown files
   - Focus: Public API documentation
   - Status: Active
   - Key content: API authentication, endpoints (activity logs, admin logs, notes), rate limits

5. **documentation-agent-v1** (LEGACY)
   - URL: https://github.com/ForestAdmin/documentation
   - Files: 174 markdown files
   - Focus: Agent v1 (forest-express-sequelize, forest-express-mongoose, forest_liana, django-forestadmin, laravel-forestadmin)
   - Status: Deprecated (end-of-support announced)
   - Key content: v1 Smart Actions/Fields/Views, Integrations, Setup guides, Routes, Scopes

### Repository Characteristics

**Common patterns across repos:**
- All use GitBook for current documentation
- All use markdown format
- Heavy use of code examples
- Mix of conceptual and how-to content
- Significant duplication of core concepts

**Problematic patterns identified:**
- Concepts documented in 3-4 places with slight variations
- No clear single source of truth
- Cross-repo links don't work
- Search doesn't span all repos
- Version confusion (v1 vs v2 vs Cloud)

---

## Priorities

### Framework Priority (confirmed by user)
1. **Node.js** - Highest priority, most users
2. **Ruby** - Second priority
3. **Python** - Third priority
4. **PHP** - DEPRECATED, moved to Legacy

### Architecture Priority (confirmed by user)
1. **Self-hosted** - Majority deployment, most prominent
2. **Cloud** - Important but smaller user base

### Content Priority

**Tier 1 (Must-have, ~100 files):**
- Get Started (all pages)
- Integration > Setup (Node.js, Ruby, Python)
- Reference > API (all pages)
- Operations > Build (Workspaces, Layout Editor)
- Operations > Execute (Actions)
- Process > Smart Actions (overview + Node.js)

**Tier 2 (Core features, ~200 files):**
- All Operations categories (complete)
- Integration (complete)
- Control (complete)
- Process > Smart Fields, Smart Segments (all frameworks)
- Guides > Deployment (AWS, Heroku)

**Tier 3 (Advanced, ~300 files):**
- Guides > Integrations (all)
- Guides > Migration (complete)
- Reference > CLI (all commands)
- Best Practices (all)

**Tier 4 (Archive, ~250 files):**
- Legacy > Agent v1
- Legacy > PHP

### Migration Timeline

**Week 1:** Tier 1 content (foundation, essential docs)
**Week 2-3:** Tier 2 content (core features, enable full usage)
**Week 3-4:** Tier 3 content (advanced use cases, complete picture)
**Week 4+:** Tier 4 content (archive legacy, cleanup)

---

## Next Steps

1. **Review this structure** - Validate architecture and decisions
2. **Begin content migration** - Start with Tier 1 priority
3. **Choose documentation tool** - Evaluate Mintlify, VitePress, or Docusaurus
4. **Create migration scripts** - Automate file movement where possible
5. **Setup deployment** - CI/CD for documentation site
6. **Add redirects** - From old docs to new structure (SEO)
7. **Launch & communicate** - Announce new docs to users

---

## Contributing

### File Naming Conventions

- Use lowercase with hyphens: `smart-actions.md`, not `SmartActions.md` or `smart_actions.md`
- Overview pages: `overview.md` at the start of each section
- Framework-specific pages: `nodejs.md`, `ruby.md`, `python.md`, `cloud.md`

### Placeholder Format

Each placeholder file should contain:
1. Title and description
2. Status tag (TODO / ROADMAP / DEPRECATED)
3. Source repositories to migrate from
4. Content outline
5. Migration notes
6. Related pages

See example placeholders in this repository.

### Markdown Style

- Use standard markdown (`.md` not `.mdx` until tool is chosen)
- Code blocks with language specification: ` ```javascript `
- Headers: Use `#` for h1, `##` for h2, etc.
- Links: Relative paths within documentation

---

## Questions & Decisions Log

For complete history of decisions made during architecture design, see [DECISIONS.md](/DECISIONS)

---

**Last Updated:** 2025-10-24
**Status:** Architecture complete, ready for content migration
**Contact:** [Your team contact info]
