---
title: Build Tools
---
> **Status:** TODO - Priority: HIGH
>
> **Category:** Operations > Build
>
> **Audience:** All users (ops teams and developers)

## Description

Build custom interfaces for your operations teams - no code required for basic customization.

## Sources to migrate

- `documentation-user-guide/getting-started/master-your-ui/README.md`
- `documentation-user-guide/getting-started/master-your-ui/using-the-layout-editor-mode/`
- `documentation-cloud/sources/ui-customization/workspaces.md`

## Content outline

1. **What are Build Tools?**
   - Visual tools to create custom interfaces
   - No code required for basic use
   - Code available for advanced customization
   - Three main tools: Workspaces, Layout Editor, Custom Views

2. **Build Tools Overview**

   **Workspaces**
   - Purpose: Create task-specific apps (KYC, incident management, fraud monitoring)
   - Who uses it: Ops teams (no-code) + developers (advanced)
   - [Learn more](/product/build/workspaces)

   **Layout Editor**
   - Purpose: Customize tables, details, forms
   - Who uses it: Ops teams (drag-and-drop)
   - [Learn more](/product/build/layout-editor)

   **Custom Views**
   - Purpose: Build completely custom UI components
   - Who uses it: Developers (code required)
   - [Learn more](/product/build/custom-views)

3. **When to Use Each Tool**
   - **Workspaces:** Building a complete workflow/app
   - **Layout Editor:** Adjusting default views (tables, forms)
   - **Custom Views:** Need specialized visualization (maps, calendars, galleries)

4. **Build Workflow**
   1. Start with Layout Editor (easy customization)
   2. Create Workspaces for specific workflows
   3. Add Custom Views when you need specialized UI
   4. Connect to Smart Actions for business logic

5. **Build + Process Integration**
   - Build tools create the UI
   - Process layer (Smart Actions) adds the logic
   - Example: Workspace displays data → Smart Action processes refund
   - [See Process layer](/product/process/overview)

6. **What's Next?**
   - [Create your first Workspace](/product/build/workspaces)
   - [Customize with Layout Editor](/product/build/layout-editor)
   - [Build Custom Views](/product/build/custom-views)

## Migration notes

- Content from user-guide (UI focus) goes here
- Link to Process layer for business logic (don't duplicate)
- Build = UI, Process = Logic (clear separation)
- Workspaces span both layers but primary home is here

## Related pages

- [Workspaces](/product/build/workspaces)
- [Layout Editor](/product/build/layout-editor)
- [Custom Views](/product/build/custom-views)
- [Process Layer](/product/process/overview)
