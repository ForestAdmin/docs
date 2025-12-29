---
title: "Manual SCIM integration with Okta"
description: "Manually configure Okta's SCIM provisioning with Forest Admin using the SCIMForest 2.0 Test App"
---

{% hint style="info" %}
This feature may not be available on all plans. Check the [pricing page](https://www.forestadmin.com/pricing) for more information, or [contact us](https://www.forestadmin.com/contact) to enable it for your organization.
{% endhint %}

{% hint style="warning" %}
Enabling SCIM disables Forest Admin user editing. All user management must be done through Okta.
{% endhint %}

## Supported features

The manual Okta SCIM integration enables:
- User provisioning from Okta to Forest Admin
- Updating user role, permission level, and tags
- User deletion when removed from the Okta app
- Team assignment via groups

## Setup steps

### 1. add Forest Admin app

Navigate to Applications > Browse App Catalog, then select "SCIMForest 2.0 Test App (Header Auth)". Name the application, keeping in mind each app links to one Forest Admin project.

### 2. authentication

Generate a provisioning token in Forest Admin project settings. In Okta's Integration tab, enter the token prefixed with "Bearer" (format: "Bearer [token]").

### 3. configuration

Keep "Sync Password" disabled as it's unsupported.

### 4. custom parameters

Four parameters require configuration:
- `permissionLevel`: Admin, Developer, Editor, or User
- `teams`: comma-separated team names (e.g., "Operators,Support")
- `role`: must match existing project roles
- `tags`: optional key/value pairs separated by semicolons

### 5. attribute setup

In Profile Editor, set external namespace to `urn:ietf:params:scim:schemas:extension:forest:2.0:User`

### 6. mapping rules

Create rules directing Okta to Forest Admin for automatic `role`, `permissionLevel`, and `tags` assignment.

### 7. group management

Configure Directory groups for team mapping, then use "Push Groups" to link Okta groups with Forest Admin teams. Optional: disable automatic team renaming in app settings.
