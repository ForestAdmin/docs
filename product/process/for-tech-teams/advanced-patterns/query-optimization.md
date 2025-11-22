---
title: Query Optimization Patterns
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Developers

## Description

Learn how to optimize database queries and improve Forest Admin performance.

## Sources to migrate

- Look for performance/optimization content across repos
- Best practices documentation

## Content outline

1. **Query Analysis**
   - Identifying slow queries
   - Query profiling
   - Database explain plans
   - Performance monitoring

2. **Optimization Techniques**
   - Index optimization
   - Query rewriting
   - Eager loading
   - Batch loading

3. **N+1 Query Prevention**
   - Detecting N+1 queries
   - Using includes/joins
   - DataLoader pattern
   - Batching strategies

4. **Caching Strategies**
   - Field caching
   - Query result caching
   - Smart cache invalidation
   - Redis integration

5. **Pagination & Limits**
   - Cursor-based pagination
   - Offset limits
   - Streaming large datasets

6. **Framework-Specific Optimization**
   - Node.js patterns
   - ActiveRecord optimization (Ruby)
   - Django ORM optimization (Python)

## Migration notes

- Cross-cutting content
- Important for production deployments
- Framework-specific examples

## Related pages

- [Integration > Data Sources](/product/integration/data-sources/overview)
- [Segments > Smart Segments](/product/process/segments/smart-segments/overview)
- [Aggregations](/product/process/for-tech-teams/aggregations/overview)
