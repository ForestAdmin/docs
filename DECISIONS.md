# Architecture Decisions Log

This document captures all key decisions made during the design of the new Forest Admin documentation architecture.

## Decision Process Overview

**Date:** October 24, 2025
**Participants:** Gautier (Forest Admin) + Claude (Architecture Consultant)
**Context:** Consolidating 5 scattered documentation repositories (~909 files) into one unified structure

---

## Question & Answer History

### Q1: Initial Request
**Question:** Retravailler la documentation de Forest Admin (consolidate 5 repos)

**Context:**
- USER GUIDE (UI features): https://github.com/ForestAdmin/documentation-user-guide
- DEVELOPER GUIDE v1: https://github.com/ForestAdmin/documentation
- DEVELOPER GUIDE v2: https://github.com/ForestAdmin/developer-guide/
- CLOUD guide: https://github.com/ForestAdmin/documentation-cloud
- PUBLIC API guide: https://github.com/ForestAdmin/public-api-guide

**Analysis Result:**
- Total: ~909 markdown files scattered across 5 repos
- Massive duplication (Actions, Charts, Fields documented in 4+ places)
- No cross-repo navigation
- Version confusion (v1 vs v2 vs Cloud)
- ~150+ files problematic for any layer-based structure

---

### Q2: Documentation Tool
**Question:** "Tu penses quoi de mintlify? qui semble être devenu le market standard"

**Analysis:**
- Mintlify: $150-300/month, beautiful out-of-the-box, AI-powered search
- VitePress: Free, open-source, fast but requires more setup
- Docusaurus: Free, mature, but slower than VitePress

**Decision:** Consider Mintlify but defer decision
**Rationale:**
- Budget OK ($150-300/month acceptable)
- Appearance > Control > Cost (user priorities)
- Focus on content architecture first, tool later
- Create tool-agnostic markdown structure

---

### Q3: Sunset PHP?
**Question:** "On sunset PHP totalement (cette documentation peut disparaitre)"

**Answer:** YES

**Decision:** Complete PHP deprecation
**Impact:**
- ~152 PHP files (25% of agent-v2) archived
- Moved to Legacy tab with EOL notice
- Migration path to Node.js or Python provided

---

### Q4: How to Handle Deprecated Docs?
**Question:** "Comment faire pour rendre disponibles les documentations deprecated sans qu'elles soient clefs?"

**Decision:** Separate "Legacy" tab (5th top-level tab)

**Implementation:**
- Visible tab but last position (not prominent)
- Contains: Agent v1 + PHP
- Clear deprecation notices
- `noindex` meta tags (no SEO)
- Excluded from internal search
- Migration guides provided

**Alternatives Considered:**
- Hide in footer: Harder to find for users who need it
- Put in Reference tab: Clutters technical docs
- Remove completely: Existing users would struggle

---

### Q5: Avoiding Redundancy
**Question:** "Éviter la redondance (une grande majorité pour Ruby et Node sont pareils, comme pour Cloud, User Guide, etc.)"

**User provided product layer architecture:**
```
4 Layers:
- OPERATIONS (Build, Execute, Collaborate, Manage, Embed)
- PROCESS (For Ops Teams, For Tech Teams)
- CONTROL (Roles, Auth, Security, Audit)
- INTEGRATION (Data Sources)
```

**Decision:** Align documentation with product architecture + DRY principle

**Redundancy Strategy:**
1. Shared concept pages (overview) - explain ONCE
2. Framework-specific pages (nodejs.md, ruby.md, python) - only when different
3. Use code tabs when concepts are identical
4. Link between related content (don't duplicate)

**Example:** Smart Actions
```
smart-actions/
├── overview.md      ← SHARED: What, why, when
├── cloud.md         ← Cloud-specific
├── nodejs.md        ← Node.js-specific
├── ruby.md          ← Ruby-specific
└── python.md        ← Python-specific
```

---

### Q6: Does 4-Layer Structure Work?
**Question:** "Tu peux regarder dans les documentations actuelles et voir où cela ne marcherait pas avec cette structure?"

**Analysis:** Agent analyzed all 909 files and found ~150+ problematic files

**Major Problems Identified:**
1. **Getting Started / Onboarding** (~20 files) - Crosses all layers sequentially
2. **Migration guides** (~20 files) - Meta-architectural, spans all layers
3. **Workspaces** (~10 files) - Both UI (Operations) AND process template (Process)
4. **Smart features** (~40 files) - Mix UI definition + business logic
5. **Deployment** (~20 files) - Integration + Operations
6. **CLI commands** (~10 files) - Used across all layers
7. **Third-party integrations** (~30 files) - Connection + business logic
8. **Roles & Permissions** (~15 files) - Affects all layers
9. **Troubleshooting** (~10 files) - Cross-cutting
10. **Framework/language dimension** (~100+ files) - Orthogonal to layers

**Conclusion:** ~30-40% of content doesn't fit pure 4-layer model

---

### Q7: How to Handle Problematic Content?
**Question:** Implicit - need solution for content that doesn't fit layers

**Decision:** Hybrid navigation with 5 tabs
- Tab 1: **Get Started** (onboarding journey, cross-layer)
- Tab 2: **Product** (4 layers implementation)
- Tab 3: **Reference** (API, CLI, Schema - technical specs)
- Tab 4: **Guides** (cross-cutting: integrations, deployment, migration, best practices)
- Tab 5: **Legacy** (deprecated content)

**Philosophy:** "It's okay if content mentions other layers. Don't force pure separation when concepts naturally overlap."

**Content Placement Examples:**
- **Workspaces** → Operations > Build (primary use = UI), links to Process for customization
- **Smart Actions** → Process > For Tech Teams (primary use = coding), links to Operations for UI usage
- **Migrations** → Guides tab (meta-architectural, temporal)
- **Deployment** → Guides tab (infrastructure, orthogonal to product)
- **Integrations** → Guides tab (combines connection + logic)

---

### Q8: Keep Original 4-Layer Navigation?
**Question:** "J'ai l'impression qu'on peut garder la navigation d'origine. Ce n'est pas grave si tes exemples vont d'un autre layer à un autre."

**User insight:** "Workspaces, ce n'est que de la UI, mais ça s'appuie sur des actions qui peuvent être UI or code. Mais c'est pas grave, c'est pas faux de dire que Workspaces est dans la UI"

**Decision:** YES - Keep 4-layer structure, don't over-engineer purity

**Rationale:**
- Workspaces = UI feature → Operations ✅ (even if it uses actions defined in Process)
- Smart Actions = Business logic → Process ✅ (even if it has UI aspects)
- Reality > purity
- Cross-references are fine
- Users follow mental models, not architectural perfection

---

### Q9: Usage Statistics & Priorities
**Questions:**
1. Cloud vs Self-hosted ratio?
2. Framework usage (Node.js vs Ruby vs Python)?
3. MCP & Process Builder status?
4. When to launch?
5. Budget OK?

**Answers:**
1. **Self-hosted is largely more prominent** than Cloud
2. **Node.js >> Ruby >> Python** in popularity
3. **Roadmap (soon hopefully)** - not yet released
4. **POC this week**, continue reflecting
5. **Yes, $150-300/month OK**

**Decision Impact:**
- Self-hosted prioritized over Cloud in navigation
- Setup guides order: Node.js → Ruby → Python
- Include placeholders for MCP/Process Builder (marked as ROADMAP)
- Create structure this week, migrate content progressively

---

### Q10: Operations Structure - Nested or Flat?
**Question:** "Est-ce qu'on met 'Operations -> puis les catégories' ou directement les catégories?"

**Options:**
- **Option A:** Explicit `operations/` folder with 5 subdirectories
- **Option B:** Flat - 5 categories directly (build/, execute/, collaborate/, manage/, embed/)

**Discussion:**
- Option A: Respects 4-layer concept, but deeper navigation
- Option B: Flatter navigation, but loses "Operations" label

**Decision:** Hybrid (Option A for structure)
**Implementation:**
- Folders: `product/integration/`, `product/control/`, `product/process/`
- Operations categories: `product/build/`, `product/execute/`, `product/collaborate/`, `product/manage/`, `product/embed/`
- Navigation sidebar: Integration, Control, Process = explicit groups; Build, Execute, Collaborate, Manage, Embed = flat groups
- Preserves layer concept for education/marketing
- Provides direct access for usability

---

### Q11: Mintlify Tab Structure
**Question:** "Mintlify a deux types de menus (onglets en haut, et catégories sur la gauche), comment tu verrais ça?"

**Decision:** 5 tabs based on user journey + audience

**Tabs (final):**
```
┌─────────────┬─────────┬───────────┬────────┬────────┐
│ Get Started │ Product │ Reference │ Guides │ Legacy │
└─────────────┴─────────┴───────────┴────────┴────────┘
```

**Tab Order Refinement:**
- Originally proposed: Get Started / Product / Guides / Reference / Legacy
- User requested: Reference before Guides
- Final: Get Started / Product / **Reference** / Guides / Legacy

**Rationale:**
- Get Started = onboarding (all users)
- Product = 4 layers (all users, main content)
- Reference = API/CLI (developers, frequently accessed)
- Guides = situational tasks (deployment, migration, integrations)
- Legacy = deprecated (existing users only)

**Alternatives Considered:**
- Tabs by audience (Users vs Devs): Overlap too much
- Tabs by journey (Setup → Configure → Operate → Extend): Less clear
- Standard chosen: Similar to Stripe, Supabase pattern

---

### Q12: Legacy Tab Placement
**Question:** "Mettons un onglet en plus" for Legacy

**Options:**
- A) Visible 5th tab (same level as others)
- B) Hidden in footer
- C) Nested in Reference tab

**Decision:** Option A - Visible 5th tab

**Rationale:**
- ~250 deprecated files still needed by existing users
- Clear separation prevents confusion
- Easy to remove entire tab in future when usage drops
- Visible but not prominent (last position)

---

### Q13: Mintlify Configuration
**Question:** Implicit - how to configure Mintlify?

**Decision:** Defer Mintlify-specific config

**Actions Taken:**
- Create tool-agnostic structure (pure markdown)
- No `mint.json` for now
- Focus on content architecture
- Can add Mintlify config later when tool decision is finalized

**Rationale:**
- User said "Oublions mintlify pour le moment"
- Structure works with any documentation platform
- .md files are universal

---

### Q14: File Format
**Question:** "Format des fichiers: .mdx ou .md?"

**Decision:** `.md` (plain markdown)

**Rationale:**
- More universal (works with any tool)
- No tool-specific syntax yet
- Can convert to .mdx later if needed
- Simpler for now

---

### Q15: Placeholder Detail Level
**Question:** Implicit - how detailed should placeholders be?

**Decision:** Structured placeholders with migration guidance

**Template includes:**
- Title and description
- Status tag (TODO / ROADMAP / DEPRECATED)
- Audience
- Layer/category
- Sources to migrate (which old repos)
- Content outline
- Migration notes
- Related pages

**Rationale:**
- Provides clear roadmap for content migration
- Documents source material
- Makes migration actionable for team
- Preserves context and decisions

---

## Final Architecture Summary

### Structure
```
new-documentation/
├── get-started/          (Tab 1: Onboarding)
├── product/              (Tab 2: 4 Layers)
│   ├── integration/
│   ├── control/
│   ├── process/
│   ├── build/
│   ├── execute/
│   ├── collaborate/
│   ├── manage/
│   └── embed/
├── reference/            (Tab 3: API, CLI, Schema)
├── guides/               (Tab 4: Cross-cutting)
│   ├── integrations/
│   ├── deployment/
│   ├── migration/
│   └── best-practices/
└── legacy/               (Tab 5: Deprecated)
```

### Key Principles

1. **Content over tooling** - Architecture works with any platform
2. **DRY but practical** - Share concepts, specify implementations
3. **Reality over purity** - Place content where users expect it, even if it touches multiple layers
4. **Progressive disclosure** - Get Started → Product → Guides → Reference
5. **Clear deprecation** - Legacy content visible but separated
6. **Framework priority** - Node.js > Ruby > Python
7. **Architecture priority** - Self-hosted > Cloud
8. **Education + usability** - 4 layers for positioning, practical navigation for use

### Success Metrics

**Consolidation:**
- From 5 repos → 1 unified structure
- From ~909 files → ~600-700 unique files (30-40% reduction via deduplication)

**Usability:**
- Clear navigation (5 tabs)
- Fast access to common tasks
- Cross-references between related content
- Search across all content (except legacy)

**Maintainability:**
- Single source of truth for concepts
- Framework-specific only when needed
- Clear content ownership per section
- Migration path documented

---

## Open Questions

1. **MCP features** - When will MCP Server and Process Builder be released?
2. **Legacy timeline** - When can we fully remove v1 and PHP docs?
3. **Content migration owner** - Who owns content migration per section?
4. **Tool final decision** - Mintlify, VitePress, Docusaurus, or other?
5. **Launch date** - Target date for new docs launch?

---

## Next Steps

1. ✅ Structure created
2. ✅ Placeholders with migration guidance
3. ✅ Documentation complete (README, DECISIONS)
4. ⏳ Review structure with team
5. ⏳ Begin Tier 1 content migration
6. ⏳ Choose documentation platform
7. ⏳ Setup deployment pipeline
8. ⏳ Launch new docs
9. ⏳ Deprecate old repos

---

**Last Updated:** 2025-10-24
**Status:** Architecture complete, ready for team review and content migration
