---
title: Security
---
> **Status:** TODO - Priority: HIGH
>
> **Audience:** Admins, DevOps, Security teams

## Description

Security features, best practices, and hardening guidance for Forest Admin.

## Sources to migrate

- `documentation-agent-v1/how-tos/maintain/forest-admin-and-security.md`
- `documentation-user-guide/project-settings/security/`
- Security documentation

## Content outline

1. **Security Overview**
   - Security model
   - Shared responsibility
   - Compliance considerations

2. **Authentication Security**
   - [SSO](/product/control/authentication/sso) (recommended)
   - [2FA](/product/control/authentication/2fa) enforcement
   - Password policies
   - Session management

3. **Network Security**
   - **[IP Whitelisting](/product/control/security/ip-whitelisting)**
   - VPN access
   - HTTPS enforcement
   - CORS configuration

4. **Data Protection**
   - Data in transit (TLS/SSL)
   - Data at rest (database encryption)
   - PII handling
   - Sensitive field masking

5. **Access Control**
   - [Roles & Permissions](/product/control/roles-permissions)
   - [Scopes & Data Visibility](/product/control/scopes-and-visibility)
   - Principle of least privilege
   - Regular access reviews

6. **API Security**
   - API key management
   - Token rotation
   - Rate limiting
   - Request validation

7. **Self-Hosted Security**
   - Server hardening
   - Database security
   - Firewall configuration
   - Regular updates
   - Dependency scanning
   - Secrets management

8. **Audit & Monitoring**
   - [Audit logs](/product/control/audit)
   - Activity monitoring
   - Security alerts
   - Incident response

9. **Compliance**
   - GDPR considerations
   - HIPAA considerations
   - SOC 2
   - Data residency
   - Right to be forgotten

10. **Security Checklist**
    - [ ] SSO enabled
    - [ ] 2FA enforced
    - [ ] Granular roles defined
    - [ ] Scopes configured
    - [ ] IP whitelist (if needed)
    - [ ] HTTPS enforced
    - [ ] Database encryption
    - [ ] Regular security audits
    - [ ] Incident response plan
    - [ ] Backup strategy

11. **Incident Response**
    - Suspicious activity detection
    - Response procedures
    - User revocation
    - Investigation tools

12. **Security Updates**
    - Staying informed
    - Update procedures
    - Testing updates
    - Rollback plans

## Migration notes

- HIGH priority - critical for enterprise
- Link to detailed guides
- Clear actionable checklist

## Related pages

- [Authentication](/product/control/authentication/overview)
- [Roles & Permissions](/product/control/roles-permissions)
- [Audit](/product/control/audit)
- [Guides > Security Best Practices](/guides/best-practices/security)
