---
title: "Relationships"
description: "Define and manage relationships between collections"
---
> **Status:** TODO - Priority: HIGH
>
> **Audience:** Developers

## Description

Learn how to define and manage relationships between collections within your datasources.

## Sources to migrate

- `developer-guide-agent-v2/sources/agent-customization/relationships/`
- `documentation-agent-v1/reference-documentation/relationships/`

## Content outline

1. **Relationship Types**
   - BelongsTo (many-to-one)
   - HasMany (one-to-many)
   - HasOne (one-to-one)
   - ManyToMany
   - Polymorphic relationships

2. **Defining Relationships**
   - Automatic detection
   - Manual configuration
   - Foreign keys
   - Join tables

3. **Smart Relationships**
   - Code-based relationships
   - Computed associations
   - External data relationships

4. **Cross-datasource Relationships**
   - Creating relationships between different datasources
   - Virtual relationships
   - Performance considerations

5. **Platform-Specific Guides**
   - Node.js implementation
   - Ruby implementation
   - Python implementation

6. **Performance**
   - Eager loading
   - N+1 prevention
   - Relationship caching

## Migration notes

- HIGH priority - fundamental feature
- Cross-reference with Smart Fields
- Framework-specific examples

## Related pages

- [Data Sources Overview](/product/integration/data-sources/overview)
- [Process > Fields > Smart Fields](/product/process/fields/smart-fields/overview)
- [Process > Advanced Patterns > Relationships](/product/process/for-tech-teams/advanced-patterns/relationship-patterns)
