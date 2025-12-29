---
title: "Authenticate with Forest Admin"
description: "Learn how to authenticate and manage access to your Forest Admin organization"
---

## Authentication methods

Forest Admin supports multiple authentication methods to access your admin panel:

### Email and password

The default authentication method. Users create an account with their email address and password to access Forest Admin.

### Google sign-in

Users can authenticate using their Google account for faster access without managing separate credentials.

## Accessing organization settings

To configure authentication, security, and other organization-level settings:

1. Click your **Gravatar** in the bottom-left corner
2. Select **Organization Settings**
3. Navigate to the appropriate tab:
   - **SSO** - Configure Single Sign-On with identity providers
   - **Security** - Manage IP whitelisting, 2FA enforcement, session timeouts
   - **SCIM** - Set up automatic user provisioning

## Single sign-on (SSO)

Single Sign-On allows your team to authenticate using your organization's existing identity provider, centralizing access control and improving security.

### Supported providers


  * [Google Workspace](/product/control/authentication/sso-google.md) - Configure SSO with Google Workspace for your organization
  * [Azure AD](/product/control/authentication/sso-azure.md) - Set up SSO with Microsoft Azure Active Directory
  * [Okta](/product/control/authentication/sso-okta.md) - Integrate Forest Admin with Okta identity provider
  * [AWS IAM](/product/control/authentication/sso-aws.md) - Configure SSO using AWS IAM Identity Center
  * [OneLogin](/product/control/authentication/sso-onelogin.md) - Set up SSO with OneLogin identity platform


## SCIM provisioning

SCIM (System for Cross-domain Identity Management) automatically provisions and deprovisions users from your identity provider to Forest Admin.

### Prerequisites

Before configuring SCIM:
- SSO must be enabled and configured for your organization
- You must have admin access to both Forest Admin and your identity provider

### Supported providers


  * [Okta SCIM](/product/control/authentication/scim-okta.md) - Automatic user provisioning with Okta
  * [OneLogin SCIM](/product/control/authentication/scim-onelogin.md) - Automatic user provisioning with OneLogin
  * [Okta Manual Setup](/product/control/authentication/scim-okta-manual.md) - Manual SCIM configuration for Okta
