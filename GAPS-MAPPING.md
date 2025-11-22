# Gaps Mapping to New Architecture

**Date:** 2025-10-24
**Status:** Classification complete
**Total Gaps:** 50

This document maps all identified gaps to their final location in the new architecture.

---

## Classification by Location

### PRODUCT > BUILD (11 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 1 | Widget & Field Customization | `product/build/fields-and-widgets/` | HIGH |
| 2 | Collection Settings | `product/build/collection-configuration.md` | HIGH |
| 5 | Search Customization (UI) | `product/build/search-configuration.md` | HIGH |
| 23 | Workspace Patterns | `product/build/workspace-patterns/` | MEDIUM |
| 31 | UI Actions Customization | `product/build/action-ui-configuration.md` | MEDIUM |
| 32 | Smart Views | `product/build/custom-views/smart-views/` | MEDIUM |
| 33 | Layout Versioning | `product/build/layout-versioning.md` | MEDIUM |
| 34 | Branding Customization | `product/build/branding.md` | LOW |
| 49 | View Maintenance | `product/build/layout-maintenance.md` | LOW |
| 50 | Admin Panel Branding | `product/build/branding.md` (merge with 34) | LOW |

**Files to create in Build:** 8 new files/folders

---

### PRODUCT > INTEGRATION (8 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 3 | Advanced Database Patterns | `product/integration/data-sources/advanced-patterns/` | HIGH |
| 8 | Data Source Replication | `product/integration/data-sources/custom-datasources/replication/` | HIGH |
| 9 | REST API Data Source | `product/integration/data-sources/rest-api/` | HIGH |
| 21 | Relationships Advanced | `product/integration/data-sources/relationships/advanced/` | MEDIUM |
| 22 | Custom Datasource Translation | `product/integration/data-sources/custom-datasources/translation/` | MEDIUM |
| 37 | Polymorphic Relationships | `product/integration/data-sources/relationships/polymorphic.md` | MEDIUM |
| 40 | Database-Specific Guides | `product/integration/data-sources/database-guides/` | MEDIUM |

**Files to create in Integration:** 7 new files/folders

---

### PRODUCT > CONTROL (6 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 11 | Permission Levels | `product/control/roles-permissions/permission-levels.md` | HIGH |
| 14 | Security Hardening | `product/control/security/hardening-guide.md` | HIGH |
| 15 | SSO & SCIM | `product/control/authentication/sso-providers/` + `scim.md` | HIGH |
| 26 | Scopes Implementation | `product/control/scopes-and-visibility.md` | MEDIUM |
| 41 | IP Whitelisting | `product/control/security/ip-whitelisting.md` | MEDIUM |
| 42 | 2FA | `product/control/authentication/2fa.md` | HIGH |

**Files to create in Control:** 6 new files/folders

---

### PRODUCT > PROCESS > FOR TECH TEAMS (6 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 4 | Lifecycle Hooks | `product/process/for-tech-teams/hooks/` | HIGH |
| 6 | Field Behaviors Advanced | `product/process/for-tech-teams/smart-fields/advanced-behaviors.md` | HIGH |
| 7 | Plugin System | `product/process/for-tech-teams/plugins/` | HIGH |
| 25 | Segments Advanced | `product/process/for-tech-teams/smart-segments/advanced-usage.md` | MEDIUM |
| 36 | Aggregations Advanced | `product/process/for-tech-teams/queries/aggregations.md` | MEDIUM |
| 44 | Smart Actions Patterns | `product/process/for-tech-teams/smart-actions/patterns.md` | MEDIUM |

**Files to create in Process:** 6 new files/folders

**Note sur #7 (Plugins):** Oui, à mettre avec Actions, Fields, etc. - c'est un pattern de factorisation avancé pour réutiliser du code. Logique = Process > For Tech Teams

---

### PRODUCT > EXECUTE (1 gap)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 24 | Export Functionality | `product/execute/export-and-download.md` | MEDIUM |

**Files to create in Execute:** 1 file

---

### PRODUCT > COLLABORATE (4 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 12 | Approval Workflows | `product/collaborate/approval-workflows/configuration.md` | HIGH |
| 28 | Notes & Comments API | `product/collaborate/notes.md` (expand) | MEDIUM |
| 29 | Inbox & Task Distribution | `product/collaborate/inbox.md` (expand) | MEDIUM |
| 30 | Escalation Features | `product/collaborate/escalation.md` | MEDIUM |

**Files to create in Collaborate:** 4 files

**Note sur #12:** Approval = Collaborate (workflow collaboratif) ou Manage (gestion) ? **→ Collaborate** car c'est utilisé pour valider des actions entre équipes

---

### PRODUCT > MANAGE (3 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 27 | Analytics & Dashboards | `product/manage/dashboards/advanced-charts.md` | MEDIUM |
| 45 | Charts Advanced | `product/manage/dashboards/chart-patterns.md` | MEDIUM |

**Files to create in Manage:** 2 files

---

### REFERENCE > API (1 gap)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 17 | Public API Complete | `reference/api/` (migrate 8 files) | HIGH |

**Files to migrate in API:** 8 files from public-api-guide

---

### REFERENCE > CLI (1 gap)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 16 | CLI Commands Complete | `reference/cli/` (migrate 9 files) | HIGH |

**Files to migrate in CLI:** 9 files from developer-guide-agent-v2

---

### REFERENCE > SCHEMA (1 gap)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 39 | Schema Synchronization | `reference/schema/synchronization.md` | MEDIUM |

**Files to create in Schema:** 1 file

---

### GUIDES > INTEGRATIONS (2 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 10 | Framework-Specific Deployment | `guides/integrations/framework-deployment/` | HIGH |
| 43 | Third-Party Integrations (31) | `guides/integrations/` (expand) | MEDIUM |

**Files to create in Integrations:** ~35 files

**Note:** #43 = **GARDER SOUS LE PIED** (31 intégrations à migrer plus tard)

---

### GUIDES > DEPLOYMENT (1 gap)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 19 | Platform-Specific Deployment | `guides/deployment/platforms/` | HIGH |

**Files to create in Deployment:** ~10 platform guides

---

### GUIDES > MIGRATION (1 gap)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 20 | Migration v1→v2 Complete | `guides/migration/from-v1/` | HIGH |

**Files to migrate in Migration:** ~15 files

---

### GUIDES > BEST PRACTICES (5 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 13 | Performance Optimization | `guides/best-practices/performance.md` (expand) | HIGH |
| 18 | Troubleshooting Complete | `guides/best-practices/troubleshooting.md` (expand) | HIGH |
| 35 | Pagination Customization | `guides/best-practices/pagination.md` | MEDIUM |
| 38 | Multi-Tenant Setup | `guides/best-practices/multi-tenant.md` | MEDIUM |
| 46 | Forest Folder Customization | `guides/best-practices/folder-structure.md` | LOW |

**Files to create/expand in Best Practices:** 5 files

---

### LEGACY (2 gaps)

| # | Gap | File Location | Priority |
|---|-----|---------------|----------|
| 47 | Routes & Endpoints (v1) | `legacy/agent-v1/routes.md` | LOW |
| 48 | Smart Collections (v1) | `legacy/agent-v1/smart-collections.md` | LOW |

**Files to create in Legacy:** 2 files (archive v1 patterns)

---

## Summary by Location

| Location | Gap Count | Priority Distribution | Action Required |
|----------|-----------|----------------------|-----------------|
| **Product > Build** | 11 | HIGH: 3, MED: 5, LOW: 3 | Create 8 files/folders |
| **Product > Integration** | 8 | HIGH: 3, MED: 5 | Create 7 files/folders |
| **Product > Control** | 6 | HIGH: 5, MED: 1 | Create 6 files/folders |
| **Product > Process** | 6 | HIGH: 3, MED: 3 | Create 6 files/folders |
| **Product > Execute** | 1 | MED: 1 | Create 1 file |
| **Product > Collaborate** | 4 | HIGH: 1, MED: 3 | Create 4 files |
| **Product > Manage** | 2 | MED: 2 | Create 2 files |
| **Reference > API** | 1 | HIGH: 1 | Migrate 8 files |
| **Reference > CLI** | 1 | HIGH: 1 | Migrate 9 files |
| **Reference > Schema** | 1 | MED: 1 | Create 1 file |
| **Guides > Integrations** | 2 | HIGH: 1, MED: 1 | Create ~35 files |
| **Guides > Deployment** | 1 | HIGH: 1 | Create ~10 files |
| **Guides > Migration** | 1 | HIGH: 1 | Migrate ~15 files |
| **Guides > Best Practices** | 5 | HIGH: 2, MED: 2, LOW: 1 | Create 5 files |
| **Legacy** | 2 | LOW: 2 | Archive 2 files |
| **TOTAL** | **50** | **HIGH: 23, MED: 22, LOW: 5** | ~150 files to create/migrate |

---

## Priority Breakdown by Location

### HIGH Priority (23 gaps) - Week 1-2

**Product (12):**
- Build: Widget catalog, Collection settings, Search UI
- Integration: DB patterns, Replication, REST API
- Control: Permissions, Security, SSO/SCIM, 2FA
- Process: Hooks, Field behaviors, Plugins
- Collaborate: Approval workflows

**Reference (2):**
- API complete (8 files)
- CLI complete (9 files)

**Guides (9):**
- Framework deployment
- Platform deployment
- Migration v1→v2
- Performance optimization
- Troubleshooting

---

### MEDIUM Priority (22 gaps) - Week 3-4

**Product (16):**
- Build: Workspace patterns, UI actions, Smart views, Layout versioning
- Integration: Relationships advanced, Custom datasources, Polymorphic, DB guides
- Control: Scopes, IP whitelist
- Process: Segments advanced, Aggregations, Actions patterns
- Execute: Export
- Collaborate: Notes, Inbox, Escalation
- Manage: Charts, Analytics

**Reference (1):**
- Schema sync

**Guides (5):**
- Third-party integrations (31) - **SOUS LE PIED**
- Pagination
- Multi-tenant

---

### LOW Priority (5 gaps) - Month 2+

**Product (3):**
- Build: Branding, View maintenance

**Guides (1):**
- Forest folder

**Legacy (2):**
- v1 routes, smart collections

---

## Migration Effort Estimate

### Immediate (Week 1-2) - HIGH Priority
- **~60 files** to create/migrate
- Focus: Core technical features, security, reference docs

### Short-term (Week 3-4) - MEDIUM Priority
- **~70 files** to create/migrate
- Focus: Advanced features, integrations (without the 31), deployment

### Long-term (Month 2+) - LOW + Integrations
- **~40 files** to create/migrate
- Plus: **31 third-party integration guides** (SOUS LE PIED)

**Total estimated:** ~170 files + 31 integrations = ~200 files

---

## Files to Create by Section

### Product > Build (8 new files/folders)
```
product/build/
├── fields-and-widgets/
│   ├── edit-widgets-catalog.md
│   ├── display-widgets-catalog.md
│   └── widget-options.md
├── collection-configuration.md
├── search-configuration.md
├── workspace-patterns/
│   ├── incident-management.md
│   ├── kyc-workflow.md
│   ├── fraud-detection.md
│   └── fleet-management.md
├── action-ui-configuration.md
├── custom-views/
│   └── smart-views/
│       ├── map-view.md
│       ├── calendar-view.md
│       ├── gallery-view.md
│       └── custom-validation.md
├── layout-versioning.md
├── layout-maintenance.md
└── branding.md
```

### Product > Integration (7 new files/folders)
```
product/integration/data-sources/
├── advanced-patterns/
│   ├── multi-schema.md
│   ├── sql-views.md
│   ├── read-replica.md
│   └── cross-database.md
├── custom-datasources/
│   ├── replication/
│   │   ├── persistent-cache.md
│   │   ├── update-strategies.md
│   │   └── scheduled-updates.md
│   └── translation/
│       ├── structure.md
│       ├── capabilities.md
│       └── implementation.md
├── rest-api/
│   ├── setup.md
│   ├── authentication.md
│   └── pagination.md
├── relationships/
│   ├── advanced-patterns.md
│   └── polymorphic.md
└── database-guides/
    ├── postgresql.md
    ├── mongodb.md
    ├── mysql.md
    └── sql-server.md
```

### Product > Control (6 new files/folders)
```
product/control/
├── roles-permissions/
│   ├── permission-levels.md
│   └── team-management.md
├── authentication/
│   ├── sso-providers/
│   │   ├── azure.md
│   │   ├── okta.md
│   │   ├── google.md
│   │   ├── aws.md
│   │   └── onelogin.md
│   ├── scim.md
│   └── 2fa.md
├── security/
│   ├── hardening-guide.md
│   └── ip-whitelisting.md
└── scopes-and-visibility.md
```

### Product > Process > For Tech Teams (6 new files/folders)
```
product/process/for-tech-teams/
├── hooks/
│   ├── lifecycle-events.md
│   ├── hook-patterns.md
│   └── collection-interception.md
├── plugins/
│   ├── plugin-architecture.md
│   ├── custom-plugins.md
│   └── provided-plugins/
│       ├── aws-s3.md
│       ├── export.md
│       └── flattener.md
├── smart-fields/
│   └── advanced-behaviors.md
├── smart-segments/
│   └── advanced-usage.md
├── smart-actions/
│   └── patterns.md
└── queries/
    └── aggregations.md
```

### Product > Collaborate (4 files)
```
product/collaborate/
├── approval-workflows/
│   └── configuration.md
├── notes.md (expand)
├── inbox.md (expand)
└── escalation.md
```

### Product > Manage (2 files)
```
product/manage/dashboards/
├── advanced-charts.md
└── chart-patterns.md
```

### Product > Execute (1 file)
```
product/execute/
└── export-and-download.md
```

### Reference (19 files to migrate)
```
reference/
├── api/ (8 files from public-api-guide)
│   ├── introduction.md
│   ├── authentication.md
│   ├── rate-limits.md
│   └── endpoints/
│       ├── activity-logs.md
│       ├── admin-logs.md
│       └── notes.md
├── cli/ (9 files from agent-v2)
│   ├── overview.md
│   ├── init.md
│   ├── login.md
│   ├── branch.md
│   ├── switch.md
│   ├── push.md
│   ├── deploy.md
│   ├── environments-create.md
│   └── environments-reset.md
└── schema/
    └── synchronization.md
```

### Guides (80+ files)
```
guides/
├── integrations/
│   ├── framework-deployment/
│   │   ├── express.md
│   │   ├── nestjs.md
│   │   ├── rails.md
│   │   ├── django.md
│   │   └── flask.md
│   └── [31 third-party integrations - SOUS LE PIED]
├── deployment/
│   └── platforms/
│       ├── aws.md
│       ├── heroku.md
│       ├── gcp.md
│       ├── azure.md
│       └── ubuntu.md
├── migration/
│   └── from-v1/ (~15 files)
└── best-practices/
    ├── performance.md (expand)
    ├── troubleshooting.md (expand)
    ├── pagination.md
    ├── multi-tenant.md
    └── folder-structure.md
```

### Legacy (2 files)
```
legacy/agent-v1/
├── routes.md
└── smart-collections.md
```

---

## Decision Log - Key Classifications

### Plugin System (#7) → Process > For Tech Teams ✅
**Rationale:** Plugins = advanced pattern de factorisation de code (comme Actions, Fields). C'est du comportement technique avancé pour développeurs. Logique = Process > For Tech Teams avec les autres patterns de customisation.

### Approval Workflows (#12) → Collaborate ✅ (vs Manage)
**Rationale:** Approval = workflow collaboratif entre équipes pour valider des actions. Primaire = Collaborate (collaboration entre users). Secondaire = Manage (tracking/analytics des approvals). Placement = Collaborate.

### Third-Party Integrations (#43) → **GARDER SOUS LE PIED** ⏸️
**Rationale:** 31 integration guides from v1. Important mais pas prioritaire immédiat. À traiter après les gaps HIGH/MEDIUM.

---

**Status:** ✅ Mapping complete
**Next:** Créer les fichiers dans l'ordre de priorité (HIGH → MEDIUM → LOW)
