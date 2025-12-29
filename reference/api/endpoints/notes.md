---
title: Notes API
description: Create and manage collaboration notes programmatically
---

# Notes API

The Notes API allows you to programmatically create, read, update, and delete notes on records in your Forest Admin collections. This enables integration with external collaboration tools and automated note creation workflows.

## Overview

Notes provide a way to document actions, decisions, and context for specific records. With the API, you can:

- **Create Notes** - Add notes programmatically from external systems
- **Read Notes** - Retrieve notes for display or analysis
- **Update Notes** - Modify existing notes
- **Delete Notes** - Remove outdated or incorrect notes
- **Mention Users** - Tag team members using @mentions

## Endpoints

### GET /api/v1/collections/: Collection/records/: Record_id/notes

Retrieve all notes for a specific record.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

<ParamField path="collection" type="string" required>
  Name of the collection

  **Example:** `users`, `orders`, `customers`
</ParamField>

<ParamField path="record_id" type="string" required>
  ID of the record

  **Example:** `123`, `user_456`
</ParamField>

#### Query parameters

<ParamField query="page" type="integer" default="1">
  Page number for pagination (starts at 1)
</ParamField>

<ParamField query="limit" type="integer" default="50">
  Number of results per page (max: 100)
</ParamField>

<ParamField query="sort" type="string" default="-created_at">
  Sort order for results

  **Options:** `created_at`, `-created_at` (descending)
</ParamField>

#### Response


  Array of note objects


      Unique identifier for the note


      Resource type (always `notes`)


          The note content (supports Markdown and @mentions)


              Author user ID


              Author email address


              Author full name


          Array of mentioned user IDs


          ISO 8601 timestamp when the note was created


          ISO 8601 timestamp when the note was last updated


  Pagination metadata


      Current page number


      Results per page


      Total number of notes


      Total number of pages


<RequestExample>

```bash cURL
curl -X GET \
  'https://api.forestadmin.com/api/v1/collections/customers/records/123/notes' \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json'
```

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const response = await axios.get(
  'https://api.forestadmin.com/api/v1/collections/customers/records/123/notes',
  {
    headers: {
      'Authorization': `Bearer ${process.env.FOREST_API_TOKEN}`,
      'Content-Type': 'application/json'
    }
  }
);

console.log(response.data);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
require 'net/http'
require 'json'

uri = URI('https://api.forestadmin.com/api/v1/collections/customers/records/123/notes')

request = Net::HTTP::Get.new(uri)
request['Authorization'] = "Bearer #{ENV['FOREST_API_TOKEN']}"
request['Content-Type'] = 'application/json'

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts JSON.parse(response.body)
```
{% endtab %}
{% endtabs %}

</RequestExample>

<ResponseExample>

```json 200 Success
{
  "data": [
    {
      "id": "note_789",
      "type": "notes",
      "attributes": {
        "content": "Customer called regarding shipping delay. @john please follow up tomorrow.",
        "author": {
          "id": "user_123",
          "email": "support@company.com",
          "name": "Alice Support"
        },
        "mentions": ["user_456"],
        "created_at": "2024-01-15T14:30:00.000Z",
        "updated_at": "2024-01-15T14:30:00.000Z"
      }
    },
    {
      "id": "note_790",
      "type": "notes",
      "attributes": {
        "content": "Upgraded to premium plan. Discount applied: LOYAL20",
        "author": {
          "id": "user_456",
          "email": "sales@company.com",
          "name": "John Sales"
        },
        "mentions": [],
        "created_at": "2024-01-14T10:15:00.000Z",
        "updated_at": "2024-01-14T10:15:00.000Z"
      }
    }
  ],
  "meta": {
    "page": 1,
    "limit": 50,
    "total": 2,
    "total_pages": 1
  }
}
```

```json 404 Not Found
{
  "error": "Not Found",
  "message": "Record not found in collection 'customers'"
}
```

</ResponseExample>

### POST /api/v1/collections/: Collection/records/: Record_id/notes

Create a new note for a specific record.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

<ParamField path="collection" type="string" required>
  Name of the collection

  **Example:** `users`, `orders`, `customers`
</ParamField>

<ParamField path="record_id" type="string" required>
  ID of the record

  **Example:** `123`, `user_456`
</ParamField>

#### Request body

<ParamField body="content" type="string" required>
  The note content (max 10,000 characters)

  **Supports:**
  - Plain text
  - Markdown formatting
  - @mentions using `@username` or `@user_id`
</ParamField>

<ParamField body="mentions" type="array">
  Array of user IDs to mention (optional, can also use @mentions in content)

  **Example:** `["user_123", "user_456"]`
</ParamField>

#### Response


  The created note object with the same structure as GET response


<RequestExample>

```bash cURL
curl -X POST \
  'https://api.forestadmin.com/api/v1/collections/customers/records/123/notes' \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
    "content": "Customer requested callback. @john please reach out by EOD.",
    "mentions": ["user_456"]
  }'
```

{% tabs %}
{% tab title="JavaScript" %}
```javascript
const axios = require('axios');

const response = await axios.post(
  'https://api.forestadmin.com/api/v1/collections/customers/records/123/notes',
  {
    content: 'Customer requested callback. @john please reach out by EOD.',
    mentions: ['user_456']
  },
  {
    headers: {
      'Authorization': `Bearer ${process.env.FOREST_API_TOKEN}`,
      'Content-Type': 'application/json'
    }
  }
);

console.log(response.data);
```
{% endtab %}

{% tab title="Ruby" %}
```ruby
require 'net/http'
require 'json'

uri = URI('https://api.forestadmin.com/api/v1/collections/customers/records/123/notes')

request = Net::HTTP::Post.new(uri)
request['Authorization'] = "Bearer #{ENV['FOREST_API_TOKEN']}"
request['Content-Type'] = 'application/json'
request.body = {
  content: 'Customer requested callback. @john please reach out by EOD.',
  mentions: ['user_456']
}.to_json

response = Net::HTTP.start(uri.hostname, uri.port, use_ssl: true) do |http|
  http.request(request)
end

puts JSON.parse(response.body)
```
{% endtab %}
{% endtabs %}

</RequestExample>

<ResponseExample>

```json 201 Created
{
  "data": {
    "id": "note_791",
    "type": "notes",
    "attributes": {
      "content": "Customer requested callback. @john please reach out by EOD.",
      "author": {
        "id": "api_user",
        "email": "api@company.com",
        "name": "API User"
      },
      "mentions": ["user_456"],
      "created_at": "2024-01-16T09:30:00.000Z",
      "updated_at": "2024-01-16T09:30:00.000Z"
    }
  }
}
```

```json 400 Bad Request
{
  "error": "Bad Request",
  "message": "Note content is required"
}
```

```json 413 Content Too Large
{
  "error": "Content Too Large",
  "message": "Note content exceeds 10,000 character limit"
}
```

</ResponseExample>

### GET /api/v1/notes/: Id

Retrieve a specific note by ID.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

<ParamField path="id" type="string" required>
  The unique identifier of the note
</ParamField>

#### Response


  Single note object with the same structure as the list endpoint


<RequestExample>

```bash cURL
curl -X GET \
  'https://api.forestadmin.com/api/v1/notes/note_789' \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json'
```

```javascript JavaScript
const axios = require('axios');

const response = await axios.get(
  'https://api.forestadmin.com/api/v1/notes/note_789',
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
    "id": "note_789",
    "type": "notes",
    "attributes": {
      "content": "Customer called regarding shipping delay. @john please follow up tomorrow.",
      "author": {
        "id": "user_123",
        "email": "support@company.com",
        "name": "Alice Support"
      },
      "mentions": ["user_456"],
      "created_at": "2024-01-15T14:30:00.000Z",
      "updated_at": "2024-01-15T14:30:00.000Z"
    }
  }
}
```

```json 404 Not Found
{
  "error": "Not Found",
  "message": "Note not found"
}
```

</ResponseExample>

### PUT /api/v1/notes/: Id

Update an existing note.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

<ParamField path="id" type="string" required>
  The unique identifier of the note
</ParamField>

#### Request body

<ParamField body="content" type="string" required>
  The updated note content (max 10,000 characters)
</ParamField>

<ParamField body="mentions" type="array">
  Updated array of user IDs to mention

  **Example:** `["user_123", "user_456"]`
</ParamField>

#### Response


  The updated note object


<RequestExample>

```bash cURL
curl -X PUT \
  'https://api.forestadmin.com/api/v1/notes/note_789' \
  -H 'Authorization: Bearer YOUR_API_TOKEN' \
  -H 'Content-Type: application/json' \
  -d '{
    "content": "Customer called regarding shipping delay. Issue resolved. @john thanks for the follow-up!"
  }'
```

```javascript JavaScript
const axios = require('axios');

const response = await axios.put(
  'https://api.forestadmin.com/api/v1/notes/note_789',
  {
    content: 'Customer called regarding shipping delay. Issue resolved. @john thanks for the follow-up!'
  },
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
    "id": "note_789",
    "type": "notes",
    "attributes": {
      "content": "Customer called regarding shipping delay. Issue resolved. @john thanks for the follow-up!",
      "author": {
        "id": "user_123",
        "email": "support@company.com",
        "name": "Alice Support"
      },
      "mentions": ["user_456"],
      "created_at": "2024-01-15T14:30:00.000Z",
      "updated_at": "2024-01-16T10:45:00.000Z"
    }
  }
}
```

```json 403 Forbidden
{
  "error": "Forbidden",
  "message": "You don't have permission to edit this note"
}
```

```json 404 Not Found
{
  "error": "Not Found",
  "message": "Note not found"
}
```

</ResponseExample>

### DELETE /api/v1/notes/: Id

Delete a note.

<ParamField header="Authorization" type="string" required>
  Bearer token for API authentication. Format: `Bearer YOUR_API_TOKEN`
</ParamField>

<ParamField path="id" type="string" required>
  The unique identifier of the note to delete
</ParamField>

#### Response


  Returns true if deletion was successful


<RequestExample>

```bash cURL
curl -X DELETE \
  'https://api.forestadmin.com/api/v1/notes/note_789' \
  -H 'Authorization: Bearer YOUR_API_TOKEN'
```

```javascript JavaScript
const axios = require('axios');

const response = await axios.delete(
  'https://api.forestadmin.com/api/v1/notes/note_789',
  {
    headers: {
      'Authorization': `Bearer ${process.env.FOREST_API_TOKEN}`
    }
  }
);

console.log(response.data);
```


</RequestExample>

<ResponseExample>

```json 204 No Content
```

```json 403 Forbidden
{
  "error": "Forbidden",
  "message": "You don't have permission to delete this note"
}
```

```json 404 Not Found
{
  "error": "Not Found",
  "message": "Note not found"
}
```

</ResponseExample>

## Use cases

### Automated note creation from webhooks

Create notes automatically when external events occur:

```javascript
// Webhook handler for support ticket system
app.post('/webhooks/support', async (req, res) => {
  const { customerId, ticketId, message } = req.body;

  // Create note in Forest Admin
  await axios.post(
    `https://api.forestadmin.com/api/v1/collections/customers/records/${customerId}/notes`,
    {
      content: `Support ticket #${ticketId} created: ${message}`
    },
    {
      headers: { 'Authorization': `Bearer ${apiToken}` }
    }
  );

  res.sendStatus(200);
});
```

### Sync notes from external systems

Import notes from other collaboration tools:

```javascript
// Sync Slack messages as notes
async function syncSlackMessages(customerId, slackThreadId) {
  const messages = await slackAPI.getThreadMessages(slackThreadId);

  for (const message of messages) {
    await axios.post(
      `https://api.forestadmin.com/api/v1/collections/customers/records/${customerId}/notes`,
      {
        content: `[Slack] ${message.user}: ${message.text}`,
        mentions: extractMentions(message.text)
      },
      {
        headers: { 'Authorization': `Bearer ${apiToken}` }
      }
    );
  }
}
```

### Bulk note operations

Add context to multiple records:

```javascript
// Add notes to all affected customers
async function notifyAffectedCustomers(customerIds, message) {
  const promises = customerIds.map(customerId =>
    axios.post(
      `https://api.forestadmin.com/api/v1/collections/customers/records/${customerId}/notes`,
      {
        content: `[System Alert] ${message}`
      },
      {
        headers: { 'Authorization': `Bearer ${apiToken}` }
      }
    )
  );

  await Promise.all(promises);
}

// Usage
await notifyAffectedCustomers(
  ['123', '456', '789'],
  'Service disruption from 2:00 PM to 2:30 PM. Credits applied.'
);
```

### Audit trail creation

Document important actions:

```javascript
// Create audit note after sensitive operation
async function createAuditNote(recordId, action, details) {
  await axios.post(
    `https://api.forestadmin.com/api/v1/collections/users/records/${recordId}/notes`,
    {
      content: `[Audit] ${action}\n\nDetails:\n${JSON.stringify(details, null, 2)}\n\nTimestamp: ${new Date().toISOString()}`
    },
    {
      headers: { 'Authorization': `Bearer ${apiToken}` }
    }
  );
}

// Usage
await createAuditNote('user_123', 'Password Reset', {
  requestedBy: 'admin@company.com',
  ipAddress: '192.168.1.1',
  reason: 'User request via support ticket #456'
});
```

### Integration with CRM systems

Sync CRM activities as notes:

```javascript
// Sync Salesforce activities
async function syncSalesforceActivities(customerId) {
  const activities = await salesforce.getActivities(customerId);

  for (const activity of activities) {
    const content = `**${activity.subject}** (${activity.type})

${activity.description}

Status: ${activity.status}
Assigned to: @${activity.assignedTo}`;

    await axios.post(
      `https://api.forestadmin.com/api/v1/collections/customers/records/${customerId}/notes`,
      {
        content,
        mentions: [activity.assignedTo]
      },
      {
        headers: { 'Authorization': `Bearer ${apiToken}` }
      }
    );
  }
}
```

## Best practices

### 1. use meaningful content

Write clear, actionable notes:

```javascript
// Good - clear and actionable
await createNote({
  content: 'Customer reported login issue. IT ticket #1234 created. @john please follow up by EOD.'
});

// Bad - vague
await createNote({
  content: 'Issue reported'
});
```

### 2. structure notes with markdown

Use Markdown for better readability:

```javascript
const note = `**Customer Feedback: Premium Plan**

Rating: ⭐⭐⭐⭐⭐

Highlights:
- Fast support response time
- Easy to use interface
- Great value for money

Action items:
- [ ] Send thank you email @marketing
- [ ] Add to testimonials @sales
- [ ] Schedule quarterly check-in @success`;

await createNote({ content: note });
```

### 3. batch operations wisely

Group related notes, but avoid overwhelming the API:

```javascript
// Good - reasonable batch size
async function addNotesToBatch(records, message) {
  const batchSize = 10;

  for (let i = 0; i < records.length; i += batchSize) {
    const batch = records.slice(i, i + batchSize);

    await Promise.all(
      batch.map(record =>
        createNote(record.id, message)
      )
    );

    // Rate limiting pause between batches
    await sleep(1000);
  }
}
```

### 4. handle mentions properly

Extract and include mentions correctly:

```javascript
function extractMentions(content) {
  const mentionRegex = /@(\w+)/g;
  const matches = content.matchAll(mentionRegex);
  return Array.from(matches, m => m[1]);
}

// Use in note creation
const content = 'Customer escalation. @john and @sarah please review urgently.';
await createNote({
  content,
  mentions: extractMentions(content) // ['john', 'sarah']
});
```

### 5. error handling and retries

Implement robust error handling:

```javascript
async function createNoteWithRetry(recordId, content, maxRetries = 3) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    try {
      return await axios.post(
        `https://api.forestadmin.com/api/v1/collections/customers/records/${recordId}/notes`,
        { content },
        { headers: { 'Authorization': `Bearer ${apiToken}` } }
      );
    } catch (error) {
      if (error.response?.status === 429) {
        // Rate limited - wait and retry
        await sleep(Math.pow(2, attempt) * 1000);
        continue;
      }

      if (attempt === maxRetries - 1) {
        console.error(`Failed to create note after ${maxRetries} attempts:`, error.message);
        throw error;
      }
    }
  }
}
```

## Rate limits

Notes API is subject to standard rate limits:

- **60 requests per minute** per API token
- **1,000 requests per hour** per API token
- **10,000 requests per day** per API token

See [Rate Limits](/reference/api/rate-limits) for detailed information on handling rate limits.

## Character limits

- **Note content:** 10,000 characters maximum
- **Markdown:** Fully supported within character limit
- **Mentions:** No limit on number of mentions

## Permissions

Note permissions follow your Forest Admin role configuration:

- **Create:** Requires write access to the collection
- **Read:** Requires read access to the collection
- **Update:** Can only edit own notes or admin role
- **Delete:** Can only delete own notes or admin role

## Related resources


  * [Authentication](/reference/api/authentication.md) - Learn about API authentication
  * [Rate Limits](/reference/api/rate-limits.md) - Understand API usage limits
  * [Activity Logs](/reference/api/endpoints/activity-logs.md) - Track user activity logs
  * [Collaborate - Notes](/product/collaborate/notes.md) - Learn about Notes feature


