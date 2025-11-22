---
title: Workspaces
---
> **Status:** TODO - Priority: HIGH
>
> **Category:** Operations > Build
>
> **Audience:** Ops teams (primary) + Developers (advanced)

## Description

Workspaces are custom applications built for specific workflows - think "KYC app", "Incident management app", or "Fraud monitoring dashboard".

## Sources to migrate

- `documentation-user-guide/workspaces/introduction-to-workspaces.md`
- `documentation-user-guide/workspaces/using-workspaces/`
- `documentation-user-guide/workspaces/building-a-workspace/`
- `documentation-user-guide/workspaces/building-a-workspace/workspace-incident-management-example.md`
- `documentation-user-guide/workspaces/building-a-workspace/workspace-kyc-example.md`
- `documentation-user-guide/workspaces/building-a-workspace/transactions-monitoring-and-fraud-management-example.md`
- `documentation-user-guide/workspaces/building-a-workspace/fleet-management-example.md`
- `documentation-cloud/sources/ui-customization/workspaces.md`
- `documentation-cloud/sources/core-concepts/workspaces.md`

## Content outline

1. **What is a Workspace?**
   - Custom application for a specific workflow
   - Combines data, actions, and UI in one place
   - No code required for basic workspaces
   - Code available for advanced customization

2. **When to Use Workspaces**
   - Specific operational workflow (KYC, incident response, fraud review)
   - Team needs focused interface (not full admin panel)
   - Multiple steps/stages in a process
   - Examples: Customer onboarding, Order fulfillment, Content moderation

3. **Workspace Components**
   - **Data:** Collections displayed in workspace
   - **Layouts:** How data is arranged (tables, cards, lists)
   - **Actions:** Buttons to execute operations
   - **Filters:** Pre-set data filters
   - **Charts:** Visualizations for context

4. **Building a Workspace** (step-by-step)
   - Step 1: Create new workspace
   - Step 2: Choose collections to display
   - Step 3: Design layout (drag-and-drop)
   - Step 4: Add filters and actions
   - Step 5: Share with team

5. **Workspace Examples**

   **KYC Onboarding Workspace**
   - Displays: Pending applications
   - Actions: Approve, Reject, Request documents
   - Filters: By status, by risk level
   - [See full example](#)

   **Incident Management Workspace**
   - Displays: Active incidents
   - Actions: Assign, Escalate, Resolve
   - Charts: Incidents over time
   - [See full example](#)

   **Fraud Monitoring Workspace**
   - Displays: Flagged transactions
   - Actions: Block user, Refund, Mark safe
   - Filters: By risk score
   - [See full example](#)

6. **Advanced: Custom Actions in Workspaces**
   - Workspaces can trigger Smart Actions
   - Actions defined in code (Process layer)
   - Example: "Approve KYC" button → calls Smart Action → updates database + sends email
   - [See Smart Actions](/product/process/for-tech-teams/smart-actions/overview)

7. **Sharing Workspaces**
   - Share URL with pre-selected data
   - Team-specific workspaces
   - Permission control (who can access)

8. **Tips & Best Practices**
   - Start simple, add complexity later
   - Focus on one workflow per workspace
   - Use clear naming (team will use this daily)
   - Test with actual users before rolling out

## Migration notes

- Heavy content from user-guide (good source)
- Examples are GOLD - migrate all workspace examples
- Cloud docs have similar content - check for differences
- Workspaces are PRIMARY Operations tool - emphasize heavily
- Link to Smart Actions but don't duplicate Process layer content

## Related pages

- [Layout Editor](/product/build/layout-editor)
- [Smart Actions](/product/process/for-tech-teams/smart-actions/overview)
- [Actions (Execute)](/product/execute/actions)
