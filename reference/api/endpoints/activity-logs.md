---
title: Activity Logs API
description: Access and export user activity logs for audit and compliance
---

# Activity Logs API

The Activity Logs API allows you to programmatically access user activity records from Forest Admin. This is essential for compliance, security monitoring, and building custom audit dashboards.

## Overview

Activity logs capture all user actions performed in your Forest Admin interface, including:

- **CRUD Operations** - Create, read, update, delete actions
- **User Information** - Who performed each action
- **Timestamps** - When actions occurred
- **Context** - Collection and record details
- **Changes** - What was modified

## Endpoints

### GET /api/v1/activity-logs

Retrieve a paginated list of activity logs with optional filtering.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

#### Query Parameters

<ParamField query="start_date" type="string">
  Filter logs from this date (ISO 8601 format: `YYYY-MM-DD` or `YYYY-MM-DDTHH:mm:ss.sssZ`)

  **Example:** `2024-01-01` or `2024-01-01T00:00:00.000Z`
</ParamField>

<ParamField query="end_date" type="string">
  Filter logs up to this date (ISO 8601 format: `YYYY-MM-DD` or `YYYY-MM-DDTHH:mm:ss.sssZ`)

  **Example:** `2024-01-31` or `2024-01-31T23:59:59.999Z`
</ParamField>

<ParamField query="user_id" type="string">
  Filter logs by specific user ID

  **Example:** `123456`
</ParamField>

<ParamField query="collection" type="string">
  Filter logs by collection name

  **Example:** `users` or `orders`
</ParamField>

<ParamField query="action" type="string">
  Filter by action type

  **Options:** `create`, `read`, `update`, `delete`, `export`, `custom_action`
</ParamField>

<ParamField query="page" type="integer" default="1">
  Page number for pagination (starts at 1)
</ParamField>

<ParamField query="limit" type="integer" default="100">
  Number of results per page (max: 1000)
</ParamField>

<ParamField query="sort" type="string" default="-created_at">
  Sort order for results

  **Options:** `created_at`, `-created_at` (descending)
</ParamField>

#### Response


  Array of activity log objects


      Unique identifier for the activity log


      Resource type (always `activity_logs`)


          Type of action performed: `create`, `read`, `update`, `delete`, `export`, `custom_action`


          Name of the collection affected


          ID of the record affected (if applicable)


              User ID


              User email address


              User full name


          Object containing the changes made (for update actions)


              Field values before the change


              Field values after the change


          ISO 8601 timestamp when the action occurred


          IP address from which the action was performed


          Browser/client user agent string


  Pagination metadata


      Current page number


      Results per page


      Total number of results


      Total number of pages


<RequestExample>

```bash cURL
curl -X GET \
  'https://api.forestadmin.com/api/v1/activity-logs?start_date=2024-01-01&end_date=2024-01-31&limit=100' \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json'
```

<details>
<summary><strong>JavaScript</strong></summary>

```javascript
const axios = require('axios');

const response = await axios.get(
  'https://api.forestadmin.com/api/v1/activity-logs',
  {
    headers: {
      'Authorization': `Bearer ${process.env.FOREST_API_TOKEN}`,
      'Content-Type': 'application/json'
    },
    params: {
      start_date: '2024-01-01',
      end_date: '2024-01-31',
      limit: 100
    }
  }
);

console.log(response.data);
```

</details>

<details>
<summary><strong>Ruby</strong></summary>

```ruby
require 'net/http'
require 'json'
require 'uri'

uri = URI('https://api.forestadmin.com/api/v1/activity-logs')
uri.query = URI.encode_www_form({
  start_date: '2024-01-01',
  end_date: '2024-01-31',
  limit: 100
})

request = Net::HTTP::Get.new(uri)
request['Authorization'] = "Bearer #{ENV['FOREST_API_TOKEN']}"
request['Content-Type'] = 'application/json'

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts JSON.parse(response.body)
```

</details>

</RequestExample>

<ResponseExample>

```json 200 Success
{
  "data": [
    {
      "id": "log_12345",
      "type": "activity_logs",
      "attributes": {
        "action": "update",
        "collection": "users",
        "record_id": "user_789",
        "user": {
          "id": "user_123",
          "email": "admin@company.com",
          "name": "John Doe"
        },
        "changes": {
          "before": {
            "status": "pending",
            "email": "old@email.com"
          },
          "after": {
            "status": "active",
            "email": "new@email.com"
          }
        },
        "created_at": "2024-01-15T14:30:00.000Z",
        "ip_address": "192.168.1.1",
        "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)"
      }
    },
    {
      "id": "log_12346",
      "type": "activity_logs",
      "attributes": {
        "action": "create",
        "collection": "orders",
        "record_id": "order_456",
        "user": {
          "id": "user_456",
          "email": "sales@company.com",
          "name": "Jane Smith"
        },
        "changes": null,
        "created_at": "2024-01-15T15:45:00.000Z",
        "ip_address": "192.168.1.2",
        "user_agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64)"
      }
    }
  ],
  "meta": {
    "page": 1,
    "limit": 100,
    "total": 1523,
    "total_pages": 16
  }
}
```

```json 401 Unauthorized
{
  "error": "Unauthorized",
  "message": "Invalid or missing API token"
}
```

```json 403 Forbidden
{
  "error": "Forbidden",
  "message": "Insufficient permissions for this operation"
}
```

```json 429 Too Many Requests
{
  "error": "Too Many Requests",
  "message": "Rate limit exceeded. Please retry after 60 seconds.",
  "retry_after": 60
}
```

</ResponseExample>

### GET /api/v1/activity-logs/:id

Retrieve a specific activity log by ID.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

<ParamField path="id" type="string" required>
  The unique identifier of the activity log
</ParamField>

#### Response


  Single activity log object with the same structure as the list endpoint


<RequestExample>

```bash cURL
curl -X GET \
  'https://api.forestadmin.com/api/v1/activity-logs/log_12345' \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json'
```

```javascript JavaScript
const axios = require('axios');

const response = await axios.get(
  'https://api.forestadmin.com/api/v1/activity-logs/log_12345',
  {
    headers: {
      'Authorization': `Bearer ${process.env.FOREST_API_TOKEN}`,
      'Content-Type': 'application/json'
    }
  }
);

console.log(response.data);
```


</RequestExample>

<ResponseExample>

```json 200 Success
{
  "data": {
    "id": "log_12345",
    "type": "activity_logs",
    "attributes": {
      "action": "update",
      "collection": "users",
      "record_id": "user_789",
      "user": {
        "id": "user_123",
        "email": "admin@company.com",
        "name": "John Doe"
      },
      "changes": {
        "before": {
          "status": "pending",
          "email": "old@email.com"
        },
        "after": {
          "status": "active",
          "email": "new@email.com"
        }
      },
      "created_at": "2024-01-15T14:30:00.000Z",
      "ip_address": "192.168.1.1",
      "user_agent": "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)"
    }
  }
}
```

```json 404 Not Found
{
  "error": "Not Found",
  "message": "Activity log not found"
}
```

</ResponseExample>

## Use Cases

### Compliance and Audit

Export activity logs for regulatory compliance:

```javascript
// Export last 30 days of activity
const thirtyDaysAgo = new Date(Date.now() - 30*24*60*60*1000)
  .toISOString().split('T')[0];
const today = new Date().toISOString().split('T')[0];

const logs = await fetch(
  `https://api.forestadmin.com/api/v1/activity-logs?start_date=${thirtyDaysAgo}&end_date=${today}&limit=1000`,
  {
    headers: { 'Authorization': `Bearer ${apiToken}` }
  }
);
```

### Security Monitoring

Monitor suspicious activity patterns:

```javascript
// Track failed login attempts or unusual actions
const suspiciousLogs = await fetch(
  'https://api.forestadmin.com/api/v1/activity-logs?action=delete&limit=100',
  {
    headers: { 'Authorization': `Bearer ${apiToken}` }
  }
);

// Alert on bulk deletions
const data = await suspiciousLogs.json();
if (data.data.length > 10) {
  sendSecurityAlert('Multiple deletion actions detected');
}
```

### User Activity Tracking

Track specific user actions:

```javascript
// Get all actions by a specific user
const userLogs = await fetch(
  `https://api.forestadmin.com/api/v1/activity-logs?user_id=user_123&limit=50`,
  {
    headers: { 'Authorization': `Bearer ${apiToken}` }
  }
);
```

### Data Warehouse Integration

Sync activity logs to your data warehouse:

```javascript
async function syncToWarehouse() {
  let page = 1;
  let hasMore = true;

  while (hasMore) {
    const response = await fetch(
      `https://api.forestadmin.com/api/v1/activity-logs?page=${page}&limit=1000`,
      {
        headers: { 'Authorization': `Bearer ${apiToken}` }
      }
    );

    const { data, meta } = await response.json();

    // Insert into data warehouse
    await warehouse.insert('activity_logs', data);

    hasMore = page < meta.total_pages;
    page++;
  }
}
```

## Best Practices

### 1. Use Date Filters

Always filter by date range to reduce data transfer:

```javascript
// Good - filtered request
const logs = await getActivityLogs({
  start_date: '2024-01-01',
  end_date: '2024-01-31'
});

// Bad - fetch everything
const logs = await getActivityLogs();
```

### 2. Implement Pagination

Process large result sets in chunks:

```javascript
async function getAllLogs(startDate, endDate) {
  const allLogs = [];
  let page = 1;
  let totalPages = 1;

  while (page <= totalPages) {
    const response = await getActivityLogs({
      start_date: startDate,
      end_date: endDate,
      page,
      limit: 1000
    });

    allLogs.push(...response.data);
    totalPages = response.meta.total_pages;
    page++;
  }

  return allLogs;
}
```

### 3. Cache Results

Cache frequently accessed data:

```javascript
const cache = new Map();
const CACHE_TTL = 5 * 60 * 1000; // 5 minutes

async function getCachedLogs(params) {
  const key = JSON.stringify(params);
  const cached = cache.get(key);

  if (cached && Date.now() - cached.timestamp < CACHE_TTL) {
    return cached.data;
  }

  const data = await getActivityLogs(params);
  cache.set(key, { data, timestamp: Date.now() });

  return data;
}
```

### 4. Handle Rate Limits

Implement retry logic with exponential backoff:

```javascript
async function getActivityLogsWithRetry(params, maxRetries = 3) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await getActivityLogs(params);
    } catch (error) {
      if (error.status === 429) {
        const retryAfter = error.headers['retry-after'] * 1000 ||
                          Math.pow(2, attempt) * 1000;
        await sleep(retryAfter);
        continue;
      }
      throw error;
    }
  }
  throw new Error('Max retries exceeded');
}
```

### 5. Filter by Collection

Request only the collections you need:

```javascript
// Good - specific collection
const userLogs = await getActivityLogs({ collection: 'users' });

// Bad - all collections when you only need one
const allLogs = await getActivityLogs();
const userLogs = allLogs.filter(log => log.collection === 'users');
```

## Rate Limits

Activity Logs API is subject to standard rate limits:

- **60 requests per minute** per API token
- **1,000 requests per hour** per API token
- **10,000 requests per day** per API token

See [Rate Limits](/reference/api/rate-limits) for detailed information on handling rate limits.

## Related Resources


  * [Authentication](/reference/api/authentication.md) - Learn about API authentication
  * [Rate Limits](/reference/api/rate-limits.md) - Understand API usage limits
  * [Admin Logs](/reference/api/endpoints/admin-logs.md) - Monitor admin panel operations
  * [Audit Control](/product/control/audit.md) - Learn about audit features


