---
title: "SCIM integration with Okta"
description: "Automate user management by integrating Okta's SCIM provisioning with Forest Admin"
---

{% hint style="info" %}
This feature may not be available on all plans. You need administrator access to the Forest Admin project. Check the [pricing page](https://www.forestadmin.com/pricing) for more information, or [contact us](https://www.forestadmin.com/contact) to enable it.
{% endhint %}

## Supported features

The Okta SCIM integration enables:
- User provisioning from Okta to Forest Admin
- Updating user roles, permission levels, and tags
- Deleting users when removed from the Forest Admin app in Okta
- SCIM Groups for team assignment
- Read-only `userName` (email format) and name fields post-creation

## Setup process

### Step 1: Add Forest Admin app

Navigate to Okta Applications tab, browse the app catalog, and select Forest Admin. Assign a descriptive label.

### Step 2: Authenticate Okta

Enable the User provisioning feature in Forest Admin project settings. This generates an API token to paste into Okta's Integration tab.

### Step 3: Configure mapping rules

Create rules for mandatory fields: `teams`, `role`, `permissionLevel`, and optional `tags`. Values must match existing Forest Admin configurations. Ensure mapping direction flows from Okta to Forest Admin.

**Required Parameters:**
- `permissionLevel`: Must be `admin`, `editor`, `user`, or `developer`
- `role`: Must match existing Forest Admin roles
- `teams`: Team names for user assignment
- `tags`: Optional key/value pairs for user tagging

### Step 4: Manage groups

In Okta's Directory section, create groups matching Forest Admin teams. Use the "Push groups" tab to link Okta groups with Forest Admin teams. Optionally disable group renaming to prevent Okta from overwriting team names.

## Custom attributes

Add custom user attributes via Directory > Profile Editor for enhanced mapping flexibility.

## Troubleshooting

- Verify `permissionLevel` values: admin, editor, user, or developer
- Confirm `role` matches existing Forest Admin roles
- Allow time for synchronization
- Note that team updates may trigger agent restarts
