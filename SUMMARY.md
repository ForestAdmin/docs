# Summary
* [Introduction](README.md)
  * [How Forest Admin Works](get-started/how-it-works.md)


## Get started

* First Steps
  * [Introduction to Forest Admin](get-started/intro-to-forest-admin.md)
  * Quickstart
    * [Quickstart](get-started/quickstart.md)
    * [Quickstart: Cloud](get-started/quickstart-cloud.md)
    * [Quickstart: Self-Hosted](get-started/quickstart-self-hosted.md)
    * [Quickstart: On-Premise](get-started/quickstart-on-premise.md)
  * [Features Overview](get-started/features-overview.md)

* Connect
  * [Overview](product/integration/overview.md)
  * Architectures
    * [Self-Hosted Architecture](product/integration/architectures/self-hosted.md)
    * [Cloud Architecture](product/integration/architectures/cloud.md)
    * [On-Premise Architecture](product/integration/architectures/on-premise.md)
  * Data Sources
    * [Add datasources to your project](product/integration/data-sources/overview.md)
    * [Creating Your Agent](product/integration/setup/create-agent.md)
    * Provided Datasources
      * [SQL Datasource](product/integration/data-sources/provided/sql.md)
      * [Sequelize Datasource](product/integration/data-sources/provided/sequelize.md)
      * [Mongoose Datasource](product/integration/data-sources/provided/mongoose.md)
      * [MongoDB Datasource](product/integration/data-sources/provided/mongodb.md)
      * [ActiveRecord Datasource](product/integration/data-sources/provided/active-record.md)
      * [Mongoid Datasource](product/integration/data-sources/provided/mongoid.md)
    * Community Datasources
      * [Hubspot](product/integration/data-sources/community-datasources/hubspot.md)
      * [Elasticsearch](product/integration/data-sources/community-datasources/elasticsearch.md)
      * [CosmosDB](product/integration/data-sources/community-datasources/cosmosdb.md)
    * Custom Datasources
      * [Custom Datasources Overview](product/integration/data-sources/custom/overview.md)
      * Translation Strategy
        * [Overview](product/integration/data-sources/custom/translation/README.md)
        * [Structure](product/integration/data-sources/custom/translation/structure.md)
        * [Capabilities](product/integration/data-sources/custom/translation/capabilities.md)
        * [Read-Only Implementation](product/integration/data-sources/custom/translation/read-only.md)
        * [Read-Write Implementation](product/integration/data-sources/custom/translation/read-write.md)
        * [Relationships](product/integration/data-sources/custom/translation/relationships.md)
      * Replication Strategy
        * [Overview](product/integration/data-sources/custom/replication/overview.md)
        * [Schema Definition](product/integration/data-sources/custom/replication/schema.md)
        * [Persistent Cache](product/integration/data-sources/custom/replication/persistent-cache.md)
        * [Write Operations](product/integration/data-sources/custom/replication/write.md)
        * Update Strategies
          * [Overview](product/integration/data-sources/custom/replication/updates/README.md)
          * [Change Polling](product/integration/data-sources/custom/replication/updates/change-polling.md)
          * [Push Updates](product/integration/data-sources/custom/replication/updates/push.md)
          * [Scheduled Rebuild](product/integration/data-sources/custom/replication/updates/scheduled-rebuild.md)
  * Integrations
    * [MCP Servers](product/integration/integrations/mcp-servers.md)
  * Relationships
    * [Relationships Overview](product/integration/relationships/overview.md)
    * [Many-to-One Relationships](product/integration/relationships/many-to-one.md)
    * [One-to-Many Relationships](product/integration/relationships/one-to-many.md)
    * [Computed Foreign Keys](product/integration/relationships/computed-foreign-keys.md)
    * [Polymorphic Relationships](product/integration/relationships/polymorphic.md)
  * Advanced Concepts
    * [Environment Variables](product/integration/environment-variables.md)
    * [Data Types](product/integration/data-types.md)
    * [Relationships](product/integration/relationships-schema.md)
    * [TypeScript Autocompletion](product/integration/typescript-autocompletion.md)
    * [Autocompletion & Typings](product/integration/setup/autocompletion.md)
    * [Troubleshooting Setup](product/integration/setup/troubleshooting.md)
    * [.forestadmin-schema.json Reference](reference/schema/forestadmin-schema.md)
    * [Security & Privacy](reference/technical/security.md)
    * [Data Model](reference/technical/typing.md)
    * [Relationships Model](reference/technical/relationships.md)


* Control
  * [Control](product/control/overview.md)
  * [Roles & Permissions](product/control/roles-permissions.md)
  * [Teams](product/control/teams.md)
  * [Scopes](product/control/scopes-and-visibility.md)
  * [Audit & Activity Logs](product/control/audit.md)
  * Authentication
    * [Authenticate with Forest Admin](product/control/authentication/overview.md)
    * [SSO with Google](product/control/authentication/sso-google.md)
    * [SSO with Azure](product/control/authentication/sso-azure.md)
    * [SSO with Okta](product/control/authentication/sso-okta.md)
    * [SSO with AWS](product/control/authentication/sso-aws.md)
    * [SSO with OneLogin](product/control/authentication/sso-onelogin.md)
    * SCIM Provisioning
      * [SCIM Integration with Okta](product/control/authentication/scim-okta.md)
      * [SCIM Integration with OneLogin](product/control/authentication/scim-onelogin.md)
      * [Manual SCIM Integration with Okta](product/control/authentication/scim-okta-manual.md)
  * Security
    * [Security & Privacy Architecture](product/control/security/architecture.md)
    * [IP Whitelisting](product/control/security/ip-whitelisting.md)
    * [Auto Logout](product/control/security/auto-logout.md)
    * [Two-Factor Authentication Enforcement](product/control/security/2fa-enforcement.md)

* Project Settings
  * [General Settings](product/project-settings/general.md)
  * [User Management](product/project-settings/users.md)
  * [Interface & Branding](product/project-settings/interface.md)
  * [Billing & Invoices](product/project-settings/billing.md)

## Product

* Overview
  * [Forest Admin Product](product/overview.md)

* Process
  * Actions
    * [Actions](product/process/actions/overview.md)
    * [No-code actions](product/process/actions/nocode-actions.md)
    * Code-based actions
      * [Code-based actions](product/process/actions/custom-actions/overview.md)
      * Action Forms
        * [Static Forms](product/process/actions/custom-actions/forms/static-forms.md)
        * [Dynamic Forms](product/process/actions/custom-actions/forms/dynamic-forms.md)
        * [Form Widgets Catalog](product/process/actions/custom-actions/forms/widgets.md)
        * [Form Layout](product/process/actions/custom-actions/forms/layout.md)
      * [Result types](product/process/actions/custom-actions/result-types.md)
      * [Scope & Context](product/process/actions/custom-actions/scope-context.md)
      * [Related data invalidation](product/process/actions/custom-actions/related-data-invalidation.md)
  * Fields
    * [Overview](product/process/fields/overview.md)
    * [Add fields](product/process/fields/add-fields.md)
    * [Move, rename and remove fields](product/process/fields/move-rename-remove.md)
    * [Override binary field mode](product/process/fields/binary.md)
    * [Override writing behavior](product/process/fields/write.md)
    * [Override filtering behavior](product/process/fields/filter.md)
    * [Override sorting behavior](product/process/fields/sort.md)
    * [Validation](product/process/fields/validation.md)
  * Smart Fields
    * [Smart Fields Overview](product/process/smart-fields/overview.md)
    * [Computed Fields](product/process/smart-fields/computed.md)
    * [Write Override](product/process/smart-fields/write.md)
    * [Binary Fields](product/process/smart-fields/binary.md)
    * [Filter Override](product/process/smart-fields/filter.md)
    * [Sort Override](product/process/smart-fields/sort.md)
    * [Field Validation](product/process/smart-fields/validation.md)
    * [Manage Fields](product/process/smart-fields/manage-fields.md)
  * Segments
    * [Segments Overview](product/process/segments/overview.md)
    * [Smart Segments](product/process/segments/smart-segments.md)
  * [Workflows](product/process/workflows/overview.md)
  * Advanced Concepts
    * Hooks
      * [Hooks Overview](product/process/hooks/overview.md)
      * [Collection Hooks](product/process/hooks/collection-hooks.md)
      * [Collection Override](product/process/hooks/collection-override.md)
    * Plugins
      * [Plugins Overview](product/process/plugins/overview.md)
      * [Custom Plugins](product/process/plugins/custom-plugins.md)
      * Provided Plugins
        * [AWS S3 Plugin](product/process/plugins/provided/aws-s3.md)
        * [Advanced Export Plugin](product/process/plugins/provided/export-advanced.md)
        * [Flattener Plugin](product/process/plugins/provided/flattener.md)
    * Developer Workflow
      * [Development Workflow](product/process/for-tech-teams/developer-workflow/development-workflow.md)
      * [Environments & Branches](product/process/for-tech-teams/developer-workflow/environments-and-branches.md)
      * [Using Branches](product/process/for-tech-teams/developer-workflow/using-branches.md)
      * [Deploying Your Changes](product/process/for-tech-teams/developer-workflow/deploying-your-changes.md)
      * [Schema Updates](product/process/for-tech-teams/developer-workflow/schema-updates.md)
      * [TypeScript Setup & Autocompletion](product/process/for-tech-teams/developer-workflow/typescript-setup.md)

* Build
  * [Layout Editor](product/build/layout-editor.md)
  * [Collection Configuration](product/build/collection-configuration.md)
  * [Layout Maintenance](product/build/layout-maintenance.md)
  * [Workspaces](product/build/workspaces.md)
  * [Dashboards](product/manage/dashboards.md)
  * Fields & Widgets
    * [Fields & Widgets - Overview](product/build/fields-and-widgets/overview.md)
    * [Display Widgets Catalog](product/build/fields-and-widgets/display-widgets.md)
    * [Edit Widgets Catalog](product/build/fields-and-widgets/edit-widgets.md)
  * Smart Views
    * [Smart Views](product/build/custom-views/smart-views.md)
  * Custom Views
    * [Custom Views Overview](product/build/custom-views/overview.md)
    * Examples
      * [Calendar View](product/build/custom-views/examples/calendar.md)
      * [Gallery View](product/build/custom-views/examples/gallery.md)
      * [Map View](product/build/custom-views/examples/map.md)
      * [Moderation View](product/build/custom-views/examples/moderation.md)
      * [Shipping View](product/build/custom-views/examples/shipping.md)
      * [Tinder-like Validation View](product/build/custom-views/examples/tinder-validation.md)
  * Advanced Concepts
    * [Layout Versioning](product/build/layout-versioning.md)
    * [Search Configuration](product/build/search-configuration.md)
    * [Pagination Configuration](product/build/pagination.md)

* Execute
  * [Browsing & Searching Data](product/execute/browse.md)
  * [Executing Actions](product/execute/actions.md)
  * [Executing Workflows](product/execute/workflows.md)
  * [Export Data](product/execute/export.md)
  * [Custom Search](product/execute/search/custom-search.md)
  * [Pagination Configuration](product/execute/browse/pagination.md)

* Collaborate
  * [Approval Workflows](product/collaborate/approval-workflows.md)
  * [Inbox & Notifications](product/collaborate/inbox.md)
  * [Notes & Comments](product/collaborate/notes.md)
  * [Escalation Workflows](product/collaborate/escalation.md)

* Manage
  * [Activity Tracking](product/manage/activity.md)
  * [Team Performance](product/manage/team-performance.md)
  * [Dispatcher](product/manage/dispatcher.md)
  * Charts
    * [Charts - Overview](product/manage/charts/overview.md)
    * [Chart Patterns & Examples](product/manage/charts/chart-patterns.md)
    * Smart Charts
      * [Smart Charts Overview](product/manage/charts/smart-charts.md)
      * [Value Charts](product/manage/charts/examples/value.md)
      * [Time-based Charts](product/manage/charts/examples/time.md)
      * [Distribution Charts](product/manage/charts/examples/distribution.md)
      * [Leaderboard Charts](product/manage/charts/examples/leaderboard.md)
      * [Objective Charts](product/manage/charts/examples/objective.md)
      * [Percentage Charts](product/manage/charts/examples/percentage.md)
    * Custom Charts (Frontend)
      * [Custom Charts Overview](product/manage/charts/custom/overview.md)
      * [Bar Charts](product/manage/charts/custom/bar-chart.md)
      * [Cohort Charts](product/manage/charts/custom/cohort-chart.md)
      * [Density Maps](product/manage/charts/custom/density-map.md)
      * [Table Charts](product/manage/charts/custom/table-chart.md)

* Embed
  * [Assist - AI-Powered Assistant](product/embed/assist.md)
  * [MCP Server](product/embed/mcp-server.md)

## Reference

* Overview
  * [Reference](reference/overview.md)

* Agent API
  * [Agent API Overview](reference/agent-api/overview.md)
  * [Agent API Reference](reference/agent-api/nodejs.md)
  * [Ruby Agent API Reference](reference/agent-api/ruby.md)
  * Deprecated
    * [Legacy Agents - Overview](reference/agent-api/deprecated/overview.md)
    * [forest-express-sequelize Reference (LEGACY)](reference/agent-api/deprecated/forest-express-sequelize.md)
    * [forest-express-mongoose Reference (LEGACY)](reference/agent-api/deprecated/forest-express-mongoose.md)

* Public API
  * [Forest Admin Public API](reference/api/introduction.md)
  * [API Authentication](reference/api/authentication.md)
  * [API Rate Limits](reference/api/rate-limits.md)
  * Endpoints
    * [Activity Logs API](reference/api/endpoints/activity-logs.md)
    * [Admin Logs API](reference/api/endpoints/admin-logs.md)
    * [Notes API](reference/api/endpoints/notes.md)

* CLI
  * [Forest Admin CLI - Overview](reference/cli/overview.md)
  * [forest init](reference/cli/init.md)
  * [forest login](reference/cli/login.md)
  * [forest branch](reference/cli/branch.md)
  * [forest push](reference/cli/push.md)
  * [forest deploy](reference/cli/deploy.md)
  * [forest switch](reference/cli/switch.md)

* Forest CLI Commands
  * [CLI Overview](reference/forest-cli/README.md)
  * [forest init](reference/forest-cli/init.md)
  * [forest login](reference/forest-cli/login.md)
  * [forest branch](reference/forest-cli/branch.md)
  * [forest push](reference/forest-cli/push.md)
  * [forest deploy](reference/forest-cli/deploy.md)
  * [forest switch](reference/forest-cli/switch.md)
  * [forest environments:create](reference/forest-cli/environments-create.md)
  * [forest environments:reset](reference/forest-cli/environments-reset.md)
  * [forest set-origin](reference/forest-cli/set-origin.md)

* Schema
  * [.forestadmin-schema.json Reference](reference/schema/forestadmin-schema.md)
    * [Schema Reference](reference/technical/schema.md)
    * [Security & Privacy](reference/technical/security.md)
    * [Data Model](reference/technical/typing.md)
    * [Relationships Model](reference/technical/relationships.md)


## Guides

* Overview
  * [Guides](guides/overview.md)

* Learning Paths
  * [Learning Paths](guides/learning-paths/overview.md)
  * [Customize Your Agent](guides/learning-paths/developer.md)
  * [Customize Your UI](guides/learning-paths/operations.md)
  * [Advanced](guides/learning-paths/business.md)

* Deployment
  * [Deployment Guide](guides/deployment/overview.md)
  * [Production Deployment](guides/deployment/production.md)
  * [Development Workflow](guides/deployment/development-workflow.md)
    * [Deployment Workflow](guides/deployment/workflow.md)
    * [Managing Environments](guides/deployment/environments.md)
    * [Using Branches](guides/deployment/branches.md)
    * [Deploying Changes](guides/deployment/deploy-changes.md)
    * Platform-Specific Guides
      * [Deploy to AWS](guides/deployment/platforms/deploy-on-aws.md)
      * [Deploy to Azure](guides/deployment/platforms/deploy-on-azure.md)
      * [Deploy to GCP](guides/deployment/platforms/deploy-on-gcp.md)
      * [Deploy to Heroku](guides/deployment/platforms/deploy-on-heroku.md)
      * [Deploy to Ubuntu](guides/deployment/platforms/deploy-on-ubuntu.md)
  * [Environments & Configuration Management](guides/deployment/environments.md)
  * Platforms
    * [Deploy to AWS](guides/deployment/platforms/aws.md)
    * [Deploy to Heroku](guides/deployment/platforms/heroku.md)

* Migration
  * [Migration Guide](guides/migration/overview.md)
  * From v1
    * [Migrating from Agent v1 to v2](guides/migration/from-v1/overview.md)
    * [What's New in v2](guides/migration/from-v1/whats-new.md)
    * [Prerequisites](guides/migration/from-v1/prerequisites.md)
    * [Recommendations](guides/migration/from-v1/recommendations.md)
    * Migration Steps
      * [Steps Overview](guides/migration/from-v1/steps/overview.md)
      * [Compare Agents](guides/migration/from-v1/steps/compare.md)
      * [Migrate Datasources](guides/migration/from-v1/steps/datasource.md)
      * [Replace Old Agent](guides/migration/from-v1/steps/replace.md)
      * [Run in Parallel](guides/migration/from-v1/steps/run-parallel.md)
      * Customizations
        * [Smart Actions Migration](guides/migration/from-v1/steps/customizations/smart-actions.md)
        * [Smart Fields Migration](guides/migration/from-v1/steps/customizations/smart-fields.md)
        * [Smart Relationships Migration](guides/migration/from-v1/steps/customizations/smart-relationships.md)
        * [Smart Segments Migration](guides/migration/from-v1/steps/customizations/smart-segments.md)
        * [Smart Charts Migration](guides/migration/from-v1/steps/customizations/smart-charts.md)
        * [API Charts Migration](guides/migration/from-v1/steps/customizations/api-charts.md)
        * [Route Overrides Migration](guides/migration/from-v1/steps/customizations/route-overrides.md)
        * [Live Queries Migration](guides/migration/from-v1/steps/customizations/live-queries.md)
    * Steps
      * [Migration Step 2: Datasources](guides/migration/from-v1/steps/datasources.md)
      * [Migration Step 3: Smart Actions](guides/migration/from-v1/steps/smart-actions.md)

* Best Practices
  * [Performance Best Practices](guides/best-practices/performance.md)
  * [Security Best Practices](guides/best-practices/security.md)
  * [Troubleshooting Guide](guides/best-practices/troubleshooting.md)

* Troubleshooting
  * [Troubleshooting Onboarding & Authentication](guides/troubleshooting-onboarding-authentication.md)

* Integrations
  * [Third-Party Integrations](guides/integrations/overview.md)
  * [Stripe Integration](guides/integrations/stripe.md)

* Contributing
  * [Forest Admin experimental: community driven content](guides/forestadmin-experimental.md)

## Legacy

* Overview
  * [Legacy Agents Overview](legacy/agents-overview.md)
  * [Migration Guides](legacy/migration-guides.md)

* JavaScript Agents
  * [Forest Admin](legacy/javascript-agents/README.md)
  * [Quick start](legacy/javascript-agents/getting-started/setup-guide.md)
  * [Development workflow](legacy/javascript-agents/getting-started/development-workflow.md)
  * How it works
    * [How it works](legacy/javascript-agents/reference-guide/how-it-works/overview.md)
    * [Environments](legacy/javascript-agents/reference-guide/how-it-works/environments.md)
    * Developing on Forest Admin
      * [Developing on Forest Admin](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/overview.md)
      * [Using branches](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/using-branches.md)
      * [Deploying your changes](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/deploying-your-changes.md)
      * Forest CLI commands
        * [Forest CLI commands](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/overview.md)
        * [init](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/init.md)
        * [login](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/login.md)
        * [branch](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/branch.md)
        * [switch](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/switch.md)
        * [set-origin](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/set-origin.md)
        * [push](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/push.md)
        * [environments:reset](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/environments-reset.md)
        * [environments:create](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/environments-create.md)
        * [deploy](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/deploy.md)
      * [Express packages](legacy/javascript-agents/reference-guide/how-it-works/developing-on-forest-admin/express-packages.md)
  * Models
    * [Models](legacy/javascript-agents/reference-guide/models/overview.md)
    * [Enrich your models](legacy/javascript-agents/reference-guide/models/enrich-your-models.md)
    * Relationships
      * [Relationships](legacy/javascript-agents/reference-guide/models/relationships/overview.md)
      * Create a Smart relationship
        * [Create a Smart relationship](legacy/javascript-agents/reference-guide/models/relationships/create-a-smart-relationship/overview.md)
        * [GetIdsFromRequest](legacy/javascript-agents/reference-guide/models/relationships/create-a-smart-relationship/getidsfromrequest.md)
      * Smart Relationship Examples
        * [Smart hasMany relationship in mongoDB](legacy/javascript-agents/reference-guide/models/relationships/smart-relationship-examples/smart-hasmany-relationship-in-mongodb.md)
  * Actions
    * [Actions](legacy/javascript-agents/reference-guide/actions/overview.md)
    * Create and manage Smart Actions
      * [Create and manage Smart Actions](legacy/javascript-agents/reference-guide/actions/create-and-manage-smart-actions/overview.md)
      * [Use a Smart Action Form](legacy/javascript-agents/reference-guide/actions/create-and-manage-smart-actions/use-a-smart-action-form.md)
      * [Smart Action Intents](legacy/javascript-agents/reference-guide/actions/create-and-manage-smart-actions/use-action-intent.md)
    * Smart Action Examples
      * [Calculate the distance between two string addresses](legacy/javascript-agents/reference-guide/actions/smart-action-examples/calculate-the-distance-between-two-string-addresses.md)
      * [Impersonate a user](legacy/javascript-agents/reference-guide/actions/smart-action-examples/impersonate-a-user.md)
      * [Create a record with a multiselect through a many-to-many relationship](legacy/javascript-agents/reference-guide/actions/smart-action-examples/create-a-record-with-a-multiselect-through-a-many-to-many-relationship.md)
      * [Handle enums with alias labels in a smart action](legacy/javascript-agents/reference-guide/actions/smart-action-examples/handle-enums-with-alias-labels-in-a-smart-action.md)
      * [Dropdown with list of values in smart action form](legacy/javascript-agents/reference-guide/actions/smart-action-examples/dropdown-with-list-of-values-in-smart-action-form.md)
      * [Custom dynamic dropdown in a form using smart collections](legacy/javascript-agents/reference-guide/actions/smart-action-examples/custom-dynamic-dropdown-in-a-form-using-smart-collections.md)
      * [Refresh hasMany relationship in smart action](legacy/javascript-agents/reference-guide/actions/smart-action-examples/refresh-hasmany-relationship-in-smart-action.md)
      * [Smart segment to restrict access to an action on a record details view](legacy/javascript-agents/reference-guide/actions/smart-action-examples/smart-segment-to-restrict-access-to-an-action-on-a-record-details-view.md)
      * [BelongsToMany edition through smart collection](legacy/javascript-agents/reference-guide/actions/smart-action-examples/belongstomany-edition-through-smart-collection.md)
      * [Upload files to amazon s3](legacy/javascript-agents/reference-guide/actions/smart-action-examples/upload-files-to-amazon-s3.md)
      * [Upload several files with the File Picker](legacy/javascript-agents/reference-guide/actions/smart-action-examples/upload-several-files-with-the-file-picker.md)
      * [Retrieve smart field info in a smart action](legacy/javascript-agents/reference-guide/actions/smart-action-examples/retrieve-smart-field-info-in-a-smart-action.md)
      * [Smart action to create several records from the input of a single smart action form](legacy/javascript-agents/reference-guide/actions/smart-action-examples/smart-action-to-create-several-records-from-the-input-of-a-single-smart-action-form.md)
      * [Add many existing records at the same time (hasMany-belongsTo relationship)](legacy/javascript-agents/reference-guide/actions/smart-action-examples/add-many-existing-records-at-the-same-time-hasmany-belongsto-relationship.md)
      * [Call a n8n webhook](legacy/javascript-agents/reference-guide/actions/smart-action-examples/call-a-webhook-with-record-ids.md)
      * [Anonymize users in bulk](legacy/javascript-agents/reference-guide/actions/smart-action-examples/update-users-in-bulk.md)
  * Smart Fields
    * [Smart Fields](legacy/javascript-agents/reference-guide/smart-fields/overview.md)
    * Smart Field Examples
      * [Add an HTML credit card as a smart field in a summary view](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/add-an-html-credit-card-as-a-smart-field-in-a-summary-view.md)
      * [Display field with complex info in html format (rich text editor)](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/display-field-with-complex-info-in-html-format-rich-text-editor.md)
      * [Generate signed urls to display S3 files in a smart field](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/generate-signed-urls-to-display-s3-files-in-a-smart-field.md)
      * [Print a status object in a single line field](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/print-a-status-object-in-a-single-line-field.md)
      * [Sort by smart field](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/sort-by-smart-field.md)
      * [Sort by smart field that includes value from a belongsTo relationship](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/sort-by-smart-field-that-includes-value-from-a-belongsto-relationship.md)
      * [Add fields destined to the create form](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/add-fields-destined-to-the-create-form.md)
      * [Add validation to a smart field edition](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/add-validation-to-a-smart-field-edition.md)
      * [Display smart field as progress bar using rich text editor](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/display-smart-field-as-progress-bar-using-rich-text-editor.md)
      * [Update point geometry field using a smart field and algolia api](legacy/javascript-agents/reference-guide/smart-fields/smart-field-examples/update-point-geometry-field-using-a-smart-field-and-algolia-api.md)
  * Smart Collections
    * [Smart Collections](legacy/javascript-agents/reference-guide/smart-collections/overview.md)
    * Examples
      * [Create a Smart Collection with Amazon S3](legacy/javascript-agents/reference-guide/smart-collections/examples/amazon-s3-integration-example.md)
      * [Smart relationship between model and stripe cards](legacy/javascript-agents/reference-guide/smart-collections/examples/smart-relationship-between-model-and-stripe-cards.md)
      * [Create records from a Smart collection](legacy/javascript-agents/reference-guide/smart-collections/examples/create-records-from-a-smart-collection.md)
      * [Searchable smart collection with records fetched from hubspot API](legacy/javascript-agents/reference-guide/smart-collections/examples/searchable-smart-collection-with-records-fetched-from-hubspot-api.md)
    * [Serializing your records](legacy/javascript-agents/reference-guide/smart-collections/serializing-your-records.md)
  * Routes
    * [Routes](legacy/javascript-agents/reference-guide/routes/overview.md)
    * [Default routes](legacy/javascript-agents/reference-guide/routes/default-routes.md)
    * [Extend a route](legacy/javascript-agents/reference-guide/routes/extend-a-route.md)
    * [Override a route](legacy/javascript-agents/reference-guide/routes/override-a-route.md)
  * Integrations
    * [Stripe](legacy/javascript-agents/reference-guide/integrations/stripe.md)
    * [Mixpanel](legacy/javascript-agents/reference-guide/integrations/mixpanel.md)
    * [Intercom](legacy/javascript-agents/reference-guide/integrations/intercom.md)
    * Elasticsearch
      * [Interact with your Elasticsearch data](legacy/javascript-agents/reference-guide/integrations/elasticsearch/interact-with-your-elasticsearch-data.md)
      * [Elasticsearch service/utils](legacy/javascript-agents/reference-guide/integrations/elasticsearch/elasticsearch-service-utils.md)
      * [Another example](legacy/javascript-agents/reference-guide/integrations/elasticsearch/another-example.md)
    * Zendesk
      * [Zendesk](legacy/javascript-agents/reference-guide/integrations/zendesk/overview.md)
      * [Authentication, Filtering & Sorting](legacy/javascript-agents/reference-guide/integrations/zendesk/authentication-filtering-and-sorting.md)
      * [Display Zendesk tickets](legacy/javascript-agents/reference-guide/integrations/zendesk/display-zendesk-tickets.md)
      * [Display Zendesk users](legacy/javascript-agents/reference-guide/integrations/zendesk/display-zendesk-users.md)
      * [View tickets related to a user](legacy/javascript-agents/reference-guide/integrations/zendesk/view-tickets-related-to-a-user.md)
      * [Bonus: Direct link to Zendesk + change priority of a ticket](legacy/javascript-agents/reference-guide/integrations/zendesk/bonus-direct-link-to-zendesk--and--change-priority-of-a-ticket.md)
    * Dwolla
      * [Display Dwolla customers](legacy/javascript-agents/reference-guide/integrations/dwolla/display-dwolla-customers.md)
      * [Display Dwolla funding sources](legacy/javascript-agents/reference-guide/integrations/dwolla/display-dwolla-funding-sources.md)
      * [Display Dwolla transfers](legacy/javascript-agents/reference-guide/integrations/dwolla/display-dwolla-transfers.md)
      * [Link users and Dwolla customers](legacy/javascript-agents/reference-guide/integrations/dwolla/link-users-and-dwolla-customers.md)
      * [Dwolla Service](legacy/javascript-agents/reference-guide/integrations/dwolla/dwolla-service.md)
    * [Razorpay](legacy/javascript-agents/reference-guide/integrations/razorpay.md)
    * Hubspot
      * [Create a Hubspot company](legacy/javascript-agents/reference-guide/integrations/hubspot/create-a-hubspot-company.md)
      * [Display Hubspot companies](legacy/javascript-agents/reference-guide/integrations/hubspot/display-hubspot-companies.md)
    * Twilio
      * [Send an SMS with Twilio and Zapier](legacy/javascript-agents/reference-guide/integrations/twilio/send-an-sms-with-twilio-and-zapier.md)
    * [Azure Table Storage](legacy/javascript-agents/reference-guide/integrations/azure-table-storage.md)
    * Slack
      * [Send Smart Action notifications to Slack](legacy/javascript-agents/reference-guide/integrations/slack/send-smart-action-notifications-to-slack.md)
    * Algolia
      * [Geocode an address with Algolia](legacy/javascript-agents/reference-guide/integrations/algolia/geocode-an-address-with-algolia.md)
  * Smart Views
    * [Smart Views](legacy/javascript-agents/reference-guide/smart-views/overview.md)
    * [Create a Map view](legacy/javascript-agents/reference-guide/smart-views/create-a-map-view.md)
    * [Create a Calendar view](legacy/javascript-agents/reference-guide/smart-views/create-a-calendar-view.md)
    * [Create a Shipping view](legacy/javascript-agents/reference-guide/smart-views/create-a-shipping-view.md)
    * [Create a Gallery view](legacy/javascript-agents/reference-guide/smart-views/create-a-gallery-view.md)
    * [Create a custom tinder-like validation view](legacy/javascript-agents/reference-guide/smart-views/create-a-custom-tinder-like-validation-view.md)
    * [Create a dynamic calendar view for an event-booking use case](legacy/javascript-agents/reference-guide/smart-views/create-a-dynamic-calendar-view-for-an-event-booking-use-case.md)
    * [Create a custom moderation view](legacy/javascript-agents/reference-guide/smart-views/create-a-custom-moderation-view.md)
  * [Smart Segments](legacy/javascript-agents/reference-guide/smart-segments.md)
  * Scopes
    * [Scopes](legacy/javascript-agents/reference-guide/scopes/overview.md)
    * [Create a scope more than one level away based on a Smart field](legacy/javascript-agents/reference-guide/scopes/create-a-scope-more-than-one-level-away-based-on-a-smart-field.md)
    * [Scope on a smart field extracting a json's column attribute](legacy/javascript-agents/reference-guide/scopes/scope-on-a-smart-field-extracting-a-jsons-column-attribute.md)
  * [Performance](legacy/javascript-agents/reference-guide/performance.md)
  * Charts
    * [Charts](legacy/javascript-agents/reference-guide/charts/overview.md)
    * [Create an API-based Chart](legacy/javascript-agents/reference-guide/charts/create-an-api-based-chart.md)
    * [Create a Smart Chart](legacy/javascript-agents/reference-guide/charts/create-a-smart-chart.md)
    * [Create Charts with AWS Redshift](legacy/javascript-agents/reference-guide/charts/create-charts-with-aws-redshift.md)
  * Setup
    * [Install](legacy/javascript-agents/how-tos/setup/install.md)
    * [Connecting Forest Admin to Your Database (Forest Cloud)](legacy/javascript-agents/how-tos/setup/connecting-forest-admin-to-your-database-forest-cloud.md)
    * [Forest Admin IP white-listing (Forest Cloud)](legacy/javascript-agents/how-tos/setup/forest-admin-ip-white-listing-forest-cloud.md)
    * [Why HTTPS is necessary even locally](legacy/javascript-agents/how-tos/setup/why-https-is-necessary-even-locally.md)
    * [Troubleshooting](legacy/javascript-agents/how-tos/setup/troubleshooting.md)
    * [Prevent permission errors at installation](legacy/javascript-agents/how-tos/setup/prevent-permission-errors-at-installation.md)
    * [Deploy Your Admin Backend With Aws](legacy/javascript-agents/how-tos/setup/deploy-your-admin-backend-with-aws.md)
    * [Deploy your admin backend on Heroku](legacy/javascript-agents/how-tos/setup/deploy-to-production-on-heroku.md)
    * [Deploy your admin backend to Ubuntu server](legacy/javascript-agents/how-tos/setup/deploy-to-production-to-ubuntu-server.md)
    * [Deploy your admin backend to Google Cloud Platform](legacy/javascript-agents/how-tos/setup/deploy-your-admin-backend-with-google-cloud-platform.md)
    * [Install Forest Admin on a remote machine](legacy/javascript-agents/how-tos/setup/install-forest-admin-on-a-remote-machine.md)
    * [Use Forest Admin with a read-only database](legacy/javascript-agents/how-tos/setup/use-forest-admin-with-a-read-only-database.md)
    * [Configuring CORS headers](legacy/javascript-agents/how-tos/setup/configuring-cors-headers.md)
    * [Running Forest Admin on multiple servers](legacy/javascript-agents/how-tos/setup/running-forest-admin-on-multiple-servers.md)
  * Upgrade
    * Upgrade notes
      * [Upgrade to v9](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v9.md)
      * [Upgrade to v8](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v8.md)
      * [Upgrade to v7](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v7.md)
      * [Upgrade to v6](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v6.md)
      * [Upgrade to v5](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v5.md)
      * [Upgrade to v4](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v4.md)
      * [Upgrade to v3](legacy/javascript-agents/how-tos/maintain/upgrade-notes-sql-mongodb/upgrade-to-v3.md)
    * [Update your models' definition](legacy/javascript-agents/how-tos/maintain/update-your-models-definition.md)
    * [Monitor your Forest's status](legacy/javascript-agents/how-tos/maintain/monitor-your-forests-status.md)
    * [Manage your Forest Admin environments programmatically](legacy/javascript-agents/how-tos/maintain/manage-your-forest-admin-programmatically.md)
    * [Changing your domain name](legacy/javascript-agents/how-tos/maintain/changing-your-domain-name.md)
    * [Migrate to the new role system](legacy/javascript-agents/how-tos/maintain/migrate-to-the-new-role-system.md)
    * [Push your new version to production](legacy/javascript-agents/how-tos/maintain/push-your-new-version-to-production.md)
  * Databases
    * [Use a demo SQL database](legacy/javascript-agents/how-tos/databases/use-a-demo-database.md)
    * [Populate a postgreSQL database on Heroku](legacy/javascript-agents/how-tos/databases/populate-a-postgresql-database-on-heroku.md)
    * [Connect to a read replica database](legacy/javascript-agents/how-tos/databases/connect-to-a-read-replica-database.md)
    * [Plug multiple schemas](legacy/javascript-agents/how-tos/databases/plug-multiple-schemas.md)
    * [Add new databases](legacy/javascript-agents/how-tos/databases/add-new-databases.md)
    * [Manage SQL views](legacy/javascript-agents/how-tos/databases/manage-sql-views.md)
  * Settings
    * [Customize your /forest folder](legacy/javascript-agents/how-tos/settings/customize-your-forest-folder.md)
    * [Disable automatic Forest Admin schema update](legacy/javascript-agents/how-tos/settings/disable-automatic-forest-admin-schema-update.md)
    * [Include/exclude models](legacy/javascript-agents/how-tos/settings/include-exclude-models.md)
    * [Display extensive logs](legacy/javascript-agents/how-tos/settings/display-extensive-logs.md)
  * [Releases Support](legacy/javascript-agents/how-tos/releases-support/README.md)

* Ruby Agent
  * [Forest Admin](legacy/ruby-agent/README.md)
  * [Quick start](legacy/ruby-agent/getting-started/setup-guide.md)
  * [Development workflow](legacy/ruby-agent/getting-started/development-workflow.md)
  * How it works
    * [How it works](legacy/ruby-agent/reference-guide/how-it-works/overview.md)
    * [Environments](legacy/ruby-agent/reference-guide/how-it-works/environments.md)
    * Developing on Forest Admin
      * [Developing on Forest Admin](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/overview.md)
      * [Using branches](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/using-branches.md)
      * [Deploying your changes](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/deploying-your-changes.md)
      * Forest CLI commands
        * [Forest CLI commands](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/overview.md)
        * [init](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/init.md)
        * [login](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/login.md)
        * [branch](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/branch.md)
        * [switch](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/switch.md)
        * [set-origin](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/set-origin.md)
        * [push](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/push.md)
        * [environments:reset](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/environments-reset.md)
        * [environments:create](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/environments-create.md)
        * [deploy](legacy/ruby-agent/reference-guide/how-it-works/developing-on-forest-admin/forest-cli-commands/deploy.md)
  * Models
    * Relationships
      * Create a Smart relationship
        * [Create a Smart relationship](legacy/ruby-agent/reference-guide/models/relationships/create-a-smart-relationship/overview.md)
  * Actions
    * [Actions](legacy/ruby-agent/reference-guide/actions/overview.md)
    * Create and manage Smart Actions
      * [Create and manage Smart Actions](legacy/ruby-agent/reference-guide/actions/create-and-manage-smart-actions/overview.md)
      * [Use a Smart Action Form](legacy/ruby-agent/reference-guide/actions/create-and-manage-smart-actions/use-a-smart-action-form.md)
      * [Smart Action Intents](legacy/ruby-agent/reference-guide/actions/create-and-manage-smart-actions/use-action-intent.md)
    * Smart Action Examples
      * [Smart action to create several records from the input of a single smart action form](legacy/ruby-agent/reference-guide/actions/smart-action-examples/smart-action-to-create-several-records-from-the-input-of-a-single-smart-action-form.md)
  * Smart Fields
    * [Smart Fields](legacy/ruby-agent/reference-guide/smart-fields/overview.md)
  * Smart Collections
    * [Smart Collections](legacy/ruby-agent/reference-guide/smart-collections/overview.md)
  * Routes
    * [Override a route](legacy/ruby-agent/reference-guide/routes/override-a-route.md)
  * Smart Views
    * [Smart Views](legacy/ruby-agent/reference-guide/smart-views/overview.md)
    * [Create a Map view](legacy/ruby-agent/reference-guide/smart-views/create-a-map-view.md)
    * [Create a Calendar view](legacy/ruby-agent/reference-guide/smart-views/create-a-calendar-view.md)
    * [Create a Shipping view](legacy/ruby-agent/reference-guide/smart-views/create-a-shipping-view.md)
    * [Create a Gallery view](legacy/ruby-agent/reference-guide/smart-views/create-a-gallery-view.md)
    * [Create a custom tinder-like validation view](legacy/ruby-agent/reference-guide/smart-views/create-a-custom-tinder-like-validation-view.md)
    * [Create a dynamic calendar view for an event-booking use case](legacy/ruby-agent/reference-guide/smart-views/create-a-dynamic-calendar-view-for-an-event-booking-use-case.md)
    * [Create a custom moderation view](legacy/ruby-agent/reference-guide/smart-views/create-a-custom-moderation-view.md)
  * [Smart Segments](legacy/ruby-agent/reference-guide/smart-segments.md)
  * Scopes
    * [Create a scope more than one level away based on a Smart field](legacy/ruby-agent/reference-guide/scopes/create-a-scope-more-than-one-level-away-based-on-a-smart-field.md)
  * [Performance](legacy/ruby-agent/reference-guide/performance.md)
  * Charts
    * [Charts](legacy/ruby-agent/reference-guide/charts/overview.md)
    * [Create an API-based Chart](legacy/ruby-agent/reference-guide/charts/create-an-api-based-chart.md)
  * Setup
    * [Install](legacy/ruby-agent/how-tos/setup/install.md)
    * [Connecting Forest Admin to Your Database (Forest Cloud)](legacy/ruby-agent/how-tos/setup/connecting-forest-admin-to-your-database-forest-cloud.md)
    * [Forest Admin IP white-listing (Forest Cloud)](legacy/ruby-agent/how-tos/setup/forest-admin-ip-white-listing-forest-cloud.md)
    * [Why HTTPS is necessary even locally](legacy/ruby-agent/how-tos/setup/why-https-is-necessary-even-locally.md)
    * [Troubleshooting](legacy/ruby-agent/how-tos/setup/troubleshooting.md)
    * [Prevent permission errors at installation](legacy/ruby-agent/how-tos/setup/prevent-permission-errors-at-installation.md)
    * [Deploy Your Admin Backend With Aws](legacy/ruby-agent/how-tos/setup/deploy-your-admin-backend-with-aws.md)
    * [Deploy your admin backend on Heroku](legacy/ruby-agent/how-tos/setup/deploy-to-production-on-heroku.md)
    * [Deploy your admin backend to Ubuntu server](legacy/ruby-agent/how-tos/setup/deploy-to-production-to-ubuntu-server.md)
    * [Deploy your admin backend to Google Cloud Platform](legacy/ruby-agent/how-tos/setup/deploy-your-admin-backend-with-google-cloud-platform.md)
    * [Install Forest Admin on a remote machine](legacy/ruby-agent/how-tos/setup/install-forest-admin-on-a-remote-machine.md)
    * [Use Forest Admin with a read-only database](legacy/ruby-agent/how-tos/setup/use-forest-admin-with-a-read-only-database.md)
    * [Configuring CORS headers](legacy/ruby-agent/how-tos/setup/configuring-cors-headers.md)
    * [Running Forest Admin on multiple servers](legacy/ruby-agent/how-tos/setup/running-forest-admin-on-multiple-servers.md)
  * Upgrade
    * Upgrade notes
      * [Upgrade to v9](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/upgrade-to-v9.md)
      * [Upgrade to v8](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/upgrade-to-v8.md)
      * [Upgrade to v7](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/upgrade-to-v7.md)
      * [Upgrade to v6](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/upgrade-to-v6.md)
      * [Upgrade to v5](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/untitled.md)
      * [Upgrade to v4](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/upgrade-to-v4.md)
      * [Upgrade to v3](legacy/ruby-agent/how-tos/maintain/upgrade-notes-rails/upgrade-to-v3.md)
    * [Update your models' definition](legacy/ruby-agent/how-tos/maintain/update-your-models-definition.md)
    * [Monitor your Forest's status](legacy/ruby-agent/how-tos/maintain/monitor-your-forests-status.md)
    * [Manage your Forest Admin environments programmatically](legacy/ruby-agent/how-tos/maintain/manage-your-forest-admin-programmatically.md)
    * [Changing your domain name](legacy/ruby-agent/how-tos/maintain/changing-your-domain-name.md)
    * [Migrate to the new role system](legacy/ruby-agent/how-tos/maintain/migrate-to-the-new-role-system.md)
    * [Push your new version to production](legacy/ruby-agent/how-tos/maintain/push-your-new-version-to-production.md)
  * Databases
    * [Use a demo SQL database](legacy/ruby-agent/how-tos/databases/use-a-demo-database.md)
    * [Connect to a read replica database](legacy/ruby-agent/how-tos/databases/connect-to-a-read-replica-database.md)
    * [Plug multiple schemas](legacy/ruby-agent/how-tos/databases/plug-multiple-schemas.md)
    * [Add new databases](legacy/ruby-agent/how-tos/databases/add-new-databases.md)
    * [Manage SQL views](legacy/ruby-agent/how-tos/databases/manage-sql-views.md)
  * Settings
    * [Customize your /forest folder](legacy/ruby-agent/how-tos/settings/customize-your-forest-folder.md)
    * [Disable automatic Forest Admin schema update](legacy/ruby-agent/how-tos/settings/disable-automatic-forest-admin-schema-update.md)
    * [Include/exclude models](legacy/ruby-agent/how-tos/settings/include-exclude-models.md)
    * [Display extensive logs](legacy/ruby-agent/how-tos/settings/display-extensive-logs.md)
  * [Releases Support](legacy/ruby-agent/how-tos/releases-support/README.md)

* Python Agent
  * [Python Agent v1 (Deprecated)](legacy/python-agent-v1.md)
