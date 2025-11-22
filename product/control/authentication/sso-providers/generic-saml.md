---
title: Generic SAML SSO
---
> **Status:** TODO - Priority: MEDIUM
>
> **Audience:** Admins

## Description

Configure Single Sign-On with any SAML 2.0 compatible identity provider.

## Sources to migrate

- `documentation-user-guide/project-settings/teams-and-users/sso/`
- Generic SAML documentation

## Content outline

1. **SAML Overview**
   - What is SAML
   - How SAML works
   - Requirements

2. **Generic Configuration**
   - ACS URL
   - Entity ID
   - SSO URL
   - Certificate upload
   - Attribute mapping

3. **Forest Admin Setup**
   - Enable SSO
   - Configure metadata
   - Attribute configuration
   - Test connection

4. **Attribute Mapping**
   - Email (required)
   - First name
   - Last name
   - Role
   - Team

5. **Supported Providers**
   - Okta → [detailed guide](/product/control/authentication/sso-providers/okta)
   - Azure AD → [detailed guide](/product/control/authentication/sso-providers/azure)
   - Google Workspace → [detailed guide](/product/control/authentication/sso-providers/google)
   - AWS SSO → [detailed guide](/product/control/authentication/sso-providers/aws)
   - OneLogin
   - Auth0
   - Ping Identity
   - Custom SAML IdP

6. **Troubleshooting**
   - SAML response validation
   - Certificate issues
   - Attribute mapping problems
   - Clock skew errors

## Related pages

- [SSO Overview](/product/control/authentication/sso)
- [Provider-Specific Guides](/product/control/authentication/sso-providers/overview)
