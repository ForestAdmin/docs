---
title: SSO with OneLogin
description: Configure Single Sign-On with OneLogin
---

This guide covers setting up Single Sign-On (SSO) with OneLogin for Forest Admin authentication.

## Accessing organization settings

To configure SSO or SCIM:

1. Click your **Gravatar** in the bottom-left corner
2. Select **Organization Settings**
3. Navigate to the **SSO** or **SCIM** tab

## Prerequisites

{% hint style="warning" %}
This feature may not be available on all plans. Check the [pricing page](https://www.forestadmin.com/pricing) for more information, or [contact us](https://www.forestadmin.com/contact) to enable it for your organization.
{% endhint %}

## Configuration steps

### Step 1: Add application

Access your OneLogin admin dashboard and select "Add app".

### Step 2: Select connector

Choose "SAML Custom Connector (Advanced)" and complete the setup wizard.

### Step 3: Configure parameters

Enter required parameters in the Configuration section:

| Setting | Value |
|---------|-------|
| ACS URL | `https://api.forestadmin.com/api/saml/callback` |
| ACS Consumer URL Validator | `^https://(api\|app).forestadmin.com/.*$` |
| Logout URL | `https://app.forestadmin.com/login` |
| SAML nameID format | Email (must match Forest Admin account email) |
| Audience (EntityID) | `forestadmin-OrganizationName` |

### Step 4: Upload metadata

Navigate to the SSO section and copy the Issuer URL. In Forest Admin, select "XML file upload or XML file endpoint" and paste the Issuer URL as the Metadata XML endpoint.

### Step 5: Test configuration

Test before enabling organization-wide.

## Idp-initiated login

For IDP-initiated login setup, the optional RelayState parameter should be: `{"organizationName": "<OrganizationName>", "destinationUrl": "organization.projects"}`

## Troubleshooting

- Verify all endpoints and certificate expiration dates
- Ensure nameID configured on your Identity Provider matches the email used for Forest Admin accounts
