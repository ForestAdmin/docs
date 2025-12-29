---
title: Forest Admin public API
description: REST API for programmatic access to Forest Admin
---

# Forest Admin public API

The Forest Admin Public API provides programmatic access to Forest Admin data and features, enabling you to integrate Forest Admin with external tools and build custom workflows.

## Overview

The Public API is a RESTful API that allows you to:

- **Access Activity Logs** - Track and export user actions for audit and compliance
- **Manage Admin Logs** - Monitor admin panel usage and operations
- **Work with Notes** - Read and write collaboration notes programmatically
- **Integrate External Systems** - Connect Forest Admin with your data warehouse, monitoring tools, or custom applications

## Base URL

All API requests should be made to:

```
https://api.forestadmin.com
```

## Authentication

All API requests require authentication using API tokens. See [Authentication](/reference/api/authentication) for details on creating and using API tokens.

**Example request:**

```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
  https://api.forestadmin.com/api/v1/activity-logs
```

## Available endpoints


  * [Activity Logs](/reference/api/endpoints/activity-logs.md) - Track user actions and export audit trails
  * [Admin Logs](/reference/api/endpoints/admin-logs.md) - Monitor admin panel operations
  * [Notes](/reference/api/endpoints/notes.md) - Manage collaboration notes programmatically


## Key features

### Activity logs

Export and analyze user activity for compliance and security monitoring:

- Track all user actions (create, update, delete)
- Filter by user, collection, or date range
- Export for data warehousing
- Build custom audit dashboards

### Admin logs

Monitor and audit administrative operations:

- Track admin panel usage
- Security and compliance reporting
- User access patterns
- Operation history

### Notes

Integrate collaboration features with external systems:

- Read and write notes programmatically
- Sync with external collaboration tools
- Build custom notification systems
- Enhance team workflows

## Rate limits

The Public API implements rate limiting to ensure service stability. See [Rate Limits](/reference/api/rate-limits) for details on:

- Rate limit rules
- Response headers
- Handling rate limit errors
- Best practices

## Getting started

1. **Request API Access** - Contact Forest Admin to enable API access for your project
2. **Generate API Token** - Create an API token in your project settings
3. **Make Your First Request** - Use the authentication token to access endpoints
4. **Review Rate Limits** - Understand usage limits and best practices

## Use cases

### Compliance & audit

Export activity logs to your data warehouse for compliance reporting and long-term retention:

```bash
curl -H "Authorization: Bearer YOUR_API_TOKEN" \
  "https://api.forestadmin.com/api/v1/activity-logs?start_date=2024-01-01&end_date=2024-01-31"
```

### Security monitoring

Build custom dashboards to monitor user activity and detect anomalies:

- Track failed login attempts
- Monitor bulk operations
- Alert on suspicious activity
- Analyze access patterns

### Integration with external tools

Connect Forest Admin with your existing tools:

- Export notes to Slack or Teams
- Sync activity logs with Datadog or Splunk
- Integrate with your SIEM system
- Build custom reporting dashboards

### Automation

Automate workflows based on Forest Admin activity:

- Trigger actions based on specific events
- Sync data with external systems
- Generate automated reports
- Build custom notification systems

## API status

{% hint style="info" %}
**API Access Required**

The Forest Admin Public API is available for Enterprise plans. Contact your account manager or [reach out to us](https://www.forestadmin.com/contact) to enable API access for your project.
{% endhint %}

## Support

For questions or issues with the Public API:

- 📖 [Documentation](/reference/api/authentication)
- 💬 [Community Forum](https://community.forestadmin.com)
- 📧 [Support](mailto:support@forestadmin.com)
- 🎫 Enterprise: Contact your account manager

## Next steps


  * [Authentication](/reference/api/authentication.md) - Learn how to authenticate API requests
  * [Rate Limits](/reference/api/rate-limits.md) - Understand API usage limits
  * [Activity Logs](/reference/api/endpoints/activity-logs.md) - Start with activity logs endpoint
  * [Best Practices](/reference/api/rate-limits#best-practices.md) - API usage best practices


