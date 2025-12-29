---
title: "User management"
description: "Add, manage, and remove users in your Forest Admin project"
---

The Users tab allows you to invite new users, manage which teams they're part of, and control access to your Forest Admin project.

## Accessing user management

1. Click your **project logo** in the top-left corner
2. Select **Project Settings**
3. Navigate to the **Users** tab

## Inviting new users

To add a user to your Forest Admin project:

1. In the "Invite users" section, provide:
   - **Email address** - The user's work email
   - **Team** - The team they will join
   - **Role** - Their role for granular permissions
   - **Permission level** - User, Manager, Editor, Developer, or Admin
2. Click **Invite**

{% hint style="info" %}
Your pending invitations are listed below the user list so you can track who hasn't accepted yet.
{% endhint %}

## Managing user teams

Access a user's details page to manage which teams they're part of. You can:

- Add them to a new team
- Modify their teams
- Remove them from a team

{% hint style="warning" %}
A user must be assigned to at least one team at all times. Make sure to add them to a new team before removing them from their current one.
{% endhint %}

Remember to save changes after making adjustments.

## User tags

Tags organize users into groups for the [Scopes](/product/control/scopes-and-visibility) feature, controlling data visibility based on user attributes.

Each tag consists of a `key` and `value` pair:
- **Key** - Must be unique per user (e.g., `region`, `department`, `office_id`)
- **Value** - The specific value for that user (e.g., `US-West`, `Sales`, `42`)

### Example use cases

- **Regional access** - Tag users with `region: EU` or `region: US` to show only relevant data
- **Department filtering** - Tag with `department: Sales` to restrict to sales records
- **Multi-tenant applications** - Tag with `client_id: 123` for client-specific data access

## Removing users

To delete a user account:

1. Open their details page from the Users tab
2. Scroll to the **Danger zone** section
3. Click **Remove user**
4. Confirm by typing `CONFIRM REMOVE`

{% hint style="warning" %}
Removing a user is permanent and will immediately revoke their access to Forest Admin.
{% endhint %}

## Related pages

- [Roles & Permissions](/product/control/roles-permissions) - Configure granular permissions with roles
- [Teams](/product/control/teams) - Create different layouts for different groups
- [Scopes](/product/control/scopes-and-visibility) - Control data visibility with user tags
