---
title: Custom Datasources
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Advanced developers

## Description

Build custom datasource connectors for proprietary databases, APIs, or specialized data stores.

## Sources to migrate

- Custom datasource documentation
- Plugin/extensibility guides

## Content outline

1. **What are Custom Datasources?**
   - When to build custom datasources
   - vs REST API datasources
   - Architecture overview

2. **Datasource Interface**
   - Required methods (CRUD operations)
   - Optional methods
   - Schema definition
   - Query interface

3. **Implementation Steps**
   - Create datasource class
   - Implement CRUD operations
   - Define schema
   - Register with agent

4. **CRUD Operations**
   - List/Query (with filters, pagination)
   - Read single record
   - Create
   - Update
   - Delete

5. **Query Handling**
   - Filter conditions
   - Sorting
   - Pagination
   - Search

6. **Platform-Specific Guides**
   - Node.js custom datasources
   - Ruby custom datasources (if available)
   - Python custom datasources (if available)

7. **Examples**
   - Elasticsearch datasource
   - GraphQL API datasource
   - Proprietary database
   - File-based data

8. **Best Practices**
   - Error handling
   - Performance optimization
   - Caching strategies
   - Testing

9. **Advanced Topics**
   - Relationships with custom datasources
   - Aggregations
   - Transactions
   - Real-time updates

## Migration notes

- Advanced feature
- Primarily Node.js focused
- Link to plugins for reusable datasources

## Related pages

- [REST API Datasources](/product/integration/data-sources/rest-api/overview)
- [Data Sources Overview](/product/integration/data-sources/overview)
- [Process > Plugins](/product/process/for-tech-teams/plugins/overview)
