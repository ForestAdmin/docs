# Additions à faire au SUMMARY.md

## À ajouter dans "Get Started" (après ligne 17)
  * Framework-Specific Quickstarts
    * [Quickstart: Node.js](get-started/quickstart-nodejs.md)
    * [Quickstart: Ruby on Rails](get-started/quickstart-ruby.md)
  * [How Forest Admin Works](get-started/how-it-works.md)

## À ajouter dans "Data Sources" (remplacer les lignes 27-40)
  * Data Sources
    * [Add datasources to your project](product/integration/data-sources/overview.md)
    * Provided Datasources
      * [SQL Datasource](product/integration/data-sources/provided/sql.md)
      * [Sequelize Datasource](product/integration/data-sources/provided/sequelize.md)
      * [Mongoose Datasource](product/integration/data-sources/provided/mongoose.md)
      * [MongoDB Datasource](product/integration/data-sources/provided/mongodb.md)
      * [ActiveRecord Datasource](product/integration/data-sources/provided/active-record.md)
      * [Mongoid Datasource](product/integration/data-sources/provided/mongoid.md)
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

## À ajouter après "Architectures" (après ligne 24)
  * Setup & Installation
    * [Installation Guide](product/integration/setup/installation.md)
    * [Creating Your Agent](product/integration/setup/create-agent.md)
    * [Autocompletion & Typings](product/integration/setup/autocompletion.md)
    * [Troubleshooting Setup](product/integration/setup/troubleshooting.md)

## À ajouter après "Integrations" (après ligne 42)
  * Relationships
    * [Relationships Overview](product/integration/relationships/overview.md)
    * [Many-to-One Relationships](product/integration/relationships/many-to-one.md)
    * [One-to-Many Relationships](product/integration/relationships/one-to-many.md)
    * [Computed Foreign Keys](product/integration/relationships/computed-foreign-keys.md)
    * [Polymorphic Relationships](product/integration/relationships/polymorphic.md)

## À remplacer dans "Actions / Code-based actions" (lignes 89-93)
      * [Code-based actions](product/process/actions/custom-actions/overview.md)
      * Action Forms
        * [Static Forms](product/process/actions/custom-actions/forms/static-forms.md)
        * [Dynamic Forms](product/process/actions/custom-actions/forms/dynamic-forms.md)
        * [Form Widgets Catalog](product/process/actions/custom-actions/forms/widgets.md)
        * [Form Layout](product/process/actions/custom-actions/forms/layout.md)
      * [Result types](product/process/actions/custom-actions/result-types.md)
      * [Scope & Context](product/process/actions/custom-actions/scope-context.md)
      * [Related data invalidation](product/process/actions/custom-actions/related-data-invalidation.md)

## À ajouter après "Fields" (après ligne 102)
  * Smart Fields
    * [Smart Fields Overview](product/process/smart-fields/overview.md)
    * [Computed Fields](product/process/smart-fields/computed.md)
    * [Write Override](product/process/smart-fields/write.md)
    * [Binary Fields](product/process/smart-fields/binary.md)
    * [Filter Override](product/process/smart-fields/filter.md)
    * [Sort Override](product/process/smart-fields/sort.md)
    * [Field Validation](product/process/smart-fields/validation.md)
    * [Manage Fields](product/process/smart-fields/manage-fields.md)

## À remplacer "Segments" (ligne 103)
  * Segments
    * [Segments Overview](product/process/segments/overview.md)
    * [Smart Segments](product/process/segments/smart-segments.md)

## À remplacer "Hooks" section (lignes 106-110)
    * Hooks
      * [Hooks Overview](product/process/hooks/overview.md)
      * [Collection Hooks](product/process/hooks/collection-hooks.md)
      * [Collection Override](product/process/hooks/collection-override.md)

## À remplacer "Plugins" section (lignes 111-115)
    * Plugins
      * [Plugins Overview](product/process/plugins/overview.md)
      * [Custom Plugins](product/process/plugins/custom-plugins.md)
      * Provided Plugins
        * [AWS S3 Plugin](product/process/plugins/provided/aws-s3.md)
        * [Advanced Export Plugin](product/process/plugins/provided/export-advanced.md)
        * [Flattener Plugin](product/process/plugins/provided/flattener.md)

## À ajouter dans "Build" après "Smart Views" (après ligne 135)
  * Custom Views
    * [Custom Views Overview](product/build/custom-views/overview.md)
    * Examples
      * [Calendar View](product/build/custom-views/examples/calendar.md)
      * [Gallery View](product/build/custom-views/examples/gallery.md)
      * [Map View](product/build/custom-views/examples/map.md)
      * [Moderation View](product/build/custom-views/examples/moderation.md)
      * [Shipping View](product/build/custom-views/examples/shipping.md)
      * [Tinder-like Validation View](product/build/custom-views/examples/tinder-validation.md)

## À ajouter dans "Execute" (après ligne 145)
  * [Custom Search](product/execute/search/custom-search.md)
  * [Pagination Configuration](product/execute/browse/pagination.md)

## À ajouter dans "Manage / Charts" (après ligne 159)
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

## À ajouter dans "Migration / From v1" (après ligne 223)
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

## À ajouter dans "Deployment" (après ligne 215)
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

## À ajouter dans "Reference / CLI" (après ligne 195)
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

## À ajouter dans "Reference" (après ligne 198)
* Technical Reference
  * [Schema Reference](reference/technical/schema.md)
  * [Security & Privacy](reference/technical/security.md)
  * [Data Model](reference/technical/typing.md)
  * [Relationships Model](reference/technical/relationships.md)

## À ajouter à la racine (ligne 2, nouveau fichier)
* [Developer Guide Overview](developer-guide-overview.md)
