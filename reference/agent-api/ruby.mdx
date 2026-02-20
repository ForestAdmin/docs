---
title: "Ruby Agent API Reference"
description: "Complete API reference for the Forest Admin Ruby Agent"
---

Complete API reference for Forest Admin Ruby agent packages.

## Agent Setup

### Creating an Agent

The Ruby agent is designed for Rails applications and automatically introspects your data models.

```ruby
# Gemfile
gem 'forest_admin_agent'
gem 'forest_admin_datasource_active_record' # For ActiveRecord
# or
gem 'forest_admin_datasource_mongoid' # For Mongoid
```

**Installation:**

```bash
bundle install
rails generate forest_admin:install
```

**Configuration:**

```ruby
# config/initializers/forest_admin.rb
require 'forest_admin_agent'

ForestAdmin.agent = ForestAdminAgent::Agent.new do |options|
  options.auth_secret = ENV.fetch('FOREST_AUTH_SECRET')
  options.env_secret = ENV.fetch('FOREST_ENV_SECRET')
  options.is_production = Rails.env.production?
  options.logger = Rails.logger
end

ForestAdmin.agent.add_datasource(
  ForestAdminDatasourceActiveRecord::Datasource.new
)

ForestAdmin.agent.start
```

**Configuration Options:**

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `auth_secret` | String | Yes | Your FOREST_AUTH_SECRET |
| `env_secret` | String | Yes | Your FOREST_ENV_SECRET |
| `is_production` | Boolean | No | Enable production mode |
| `logger` | Logger | No | Custom logger instance |
| `prefix` | String | No | API prefix (default: '/forest') |
| `schema_path` | String | No | Path to .forestadmin-schema.json |

---

## Customizing Collections

### agent.customize_collection(name, &block)

Customize a specific collection with the provided block.

```ruby
ForestAdmin.agent.customize_collection('User') do |collection|
  collection.add_action('Send email', {
    scope: 'Single',
    execute: ->(context, result_builder) {
      # Action logic
      result_builder.success('Email sent!')
    }
  })
end
```

**Parameters:**

| Parameter | Type | Description |
|-----------|------|-------------|
| `name` | String | Collection name |
| `block` | Block | Customization block |

---

## Datasources

### agent.add_datasource(datasource, options = {})

Add a datasource to the agent.

```ruby
ForestAdmin.agent.add_datasource(
  ForestAdminDatasourceActiveRecord::Datasource.new,
  exclude: ['internal_logs']
)
```

**Options:**

| Option | Type | Description |
|--------|------|-------------|
| `include` | Array\<String\> | Collections to include |
| `exclude` | Array\<String\> | Collections to exclude |
| `rename` | Hash | Rename collections |

**Example with Mongoid:**

```ruby
ForestAdmin.agent.add_datasource(
  ForestAdminDatasourceMongoid::Datasource.new,
  rename: { 'old_name' => 'new_name' }
)
```

---

## Actions

### collection.add_action(name, definition)

Add a Smart Action to the collection.

```ruby
collection.add_action('Send email', {
  scope: 'Single',
  execute: ->(context, result_builder) {
    user = context.get_record(['id', 'email'])
    UserMailer.notification(user['email']).deliver_later
    result_builder.success('Email sent!')
  }
})
```

**Definition Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `scope` | Symbol | Action scope: `:Single`, `:Bulk`, or `:Global` |
| `execute` | Proc | Action execution handler |
| `form` | Array | Dynamic form configuration |
| `description` | String | Action description |
| `generate_file` | Boolean | Whether action returns a file |
| `submit_button_label` | String | Custom button text |

**Execute Block:**

```ruby
execute: ->(context, result_builder) {
  # Action logic
}
```

**ActionContext Methods:**

- `context.collection` - Collection instance
- `context.filter` - Filter for selected records
- `context.caller` - User who triggered the action
- `context.form_values` - Form values submitted
- `context.get_records(fields)` - Get multiple records
- `context.get_record(fields)` - Get single record (Single scope)
- `context.get_record_ids` - Get IDs of selected records
- `context.has_field_changed(field_name)` - Check if form field changed

**ResultBuilder Methods:**

- `result_builder.success(message, options = {})` - Success response
  - `options[:html]` - Custom HTML to display
  - `options[:invalidated]` - Array of collection names to refresh
- `result_builder.error(message, options = {})` - Error response
- `result_builder.webhook(url, method, headers, body)` - Trigger webhook
- `result_builder.file(stream, filename, mime_type)` - Return file download
- `result_builder.redirect_to(path)` - Redirect to URL
- `result_builder.set_header(name, value)` - Add HTTP header

**Example - Action with Form:**

```ruby
collection.add_action('Send notification', {
  scope: 'Bulk',
  form: [
    {
      label: 'Message',
      type: 'String',
      is_required: true
    },
    {
      label: 'Channel',
      type: 'Enum',
      enum_values: ['email', 'sms', 'push'],
      is_required: true
    }
  ],
  execute: ->(context, result_builder) {
    message = context.form_values['message']
    channel = context.form_values['channel']
    users = context.get_records(['email'])

    users.each do |user|
      NotificationService.send(user['email'], message, channel)
    end

    result_builder.success("Sent #{channel} to #{users.length} users")
  }
})
```

**Example - File Generation:**

```ruby
collection.add_action('Export to CSV', {
  scope: 'Bulk',
  generate_file: true,
  execute: ->(context, result_builder) {
    records = context.get_records(['name', 'email'])
    csv_stream = CsvGenerator.generate(records)

    result_builder.file(csv_stream, 'export.csv', 'text/csv')
  }
})
```

---

## Fields

### collection.add_field(name, definition)

Add a computed field to the collection.

```ruby
collection.add_field('full_name', {
  column_type: 'String',
  dependencies: ['first_name', 'last_name'],
  get_values: ->(records) {
    records.map { |r| "#{r['first_name']} #{r['last_name']}" }
  }
})
```

**Definition Properties:**

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `column_type` | String | Yes | Field data type |
| `dependencies` | Array | Yes | Fields needed for computation |
| `get_values` | Proc | Yes | Value computation function |
| `default_value` | Any | No | Default value |
| `enum_values` | Array | No | Enum options (if type is Enum) |

**Column Types:**

- `'String'` - Text
- `'Number'` - Numeric value
- `'Boolean'` - True/false
- `'Date'` - Date with time
- `'Dateonly'` - Date without time
- `'Enum'` - Enumeration
- `'Json'` - JSON object
- `'Uuid'` - UUID

**Example - Async Computed Field:**

```ruby
collection.add_field('revenue_this_year', {
  column_type: 'Number',
  dependencies: ['id'],
  get_values: ->(records) {
    ids = records.map { |r| r['id'] }
    RevenueCalculator.fetch_for_ids(ids)
  }
})
```

---

### collection.import_field(name, options)

Import a field from a related collection.

```ruby
# Import author's name into books collection
collection.import_field('author_name', {
  path: 'author:full_name',
  readonly: true
})
```

**Options:**

| Option | Type | Description |
|--------|------|-------------|
| `path` | String | Relationship path (e.g., 'author:full_name') |
| `readonly` | Boolean | Whether field is read-only |

---

### collection.rename_field(current_name, new_name)

Rename a field in the exported schema.

```ruby
collection.rename_field('created_at', 'createdAt')
```

---

### collection.remove_field(*names)

Remove fields from the exported schema.

```ruby
collection.remove_field('password', 'internal_notes', 'debug_data')
```

---

### collection.replace_field_writing(name, definition)

Replace the write behavior of a field.

```ruby
# Write full_name as first_name + last_name
collection.replace_field_writing('full_name', ->(full_name) {
  parts = full_name.split(' ', 2)
  { 'first_name' => parts[0], 'last_name' => parts[1] }
})
```

---

## Segments

### collection.add_segment(name, definition)

Add a segment (saved filter) to the collection.

```ruby
collection.add_segment('Premium users', {
  field: 'plan',
  operator: 'Equal',
  value: 'premium'
})
```

**Example - Static Segment:**

```ruby
collection.add_segment('Active users', {
  field: 'status',
  operator: 'Equal',
  value: 'active'
})
```

**Example - Dynamic Segment:**

```ruby
collection.add_segment('Active this month', ->(context) {
  start_of_month = Date.today.beginning_of_month

  {
    field: 'last_active_at',
    operator: 'After',
    value: start_of_month
  }
})
```

**Example - Complex Segment:**

```ruby
collection.add_segment('VIP customers', ->(context) {
  {
    aggregator: 'And',
    conditions: [
      { field: 'status', operator: 'Equal', value: 'active' },
      { field: 'lifetime_value', operator: 'GreaterThan', value: 10000 }
    ]
  }
})
```

---

## Relationships

### collection.add_many_to_one_relation(name, foreign_collection, options)

Add a many-to-one relationship.

```ruby
# books.author_id → persons.id
collection.add_many_to_one_relation('author', 'Person', {
  foreign_key: 'author_id'
})
```

**Options:**

| Option | Type | Description |
|--------|------|-------------|
| `foreign_key` | String | Foreign key field name |
| `foreign_key_target` | String | Target field (default: 'id') |

---

### collection.add_one_to_many_relation(name, foreign_collection, options)

Add a one-to-many relationship.

```ruby
# persons.id ← books.author_id
collection.add_one_to_many_relation('written_books', 'Book', {
  origin_key: 'author_id'
})
```

**Options:**

| Option | Type | Description |
|--------|------|-------------|
| `origin_key` | String | Foreign key in related collection |
| `origin_key_target` | String | Target field (default: 'id') |

---

### collection.add_one_to_one_relation(name, foreign_collection, options)

Add a one-to-one relationship.

```ruby
# persons.id ← profiles.person_id (unique)
collection.add_one_to_one_relation('profile', 'Profile', {
  origin_key: 'person_id'
})
```

---

### collection.add_many_to_many_relation(name, foreign_collection, through_collection, options)

Add a many-to-many relationship.

```ruby
# students ↔ student_courses ↔ courses
collection.add_many_to_many_relation('enrolled_courses', 'Course', 'StudentCourse', {
  origin_key: 'student_id',
  foreign_key: 'course_id'
})
```

**Options:**

| Option | Type | Description |
|--------|------|-------------|
| `origin_key` | String | Foreign key to origin collection |
| `foreign_key` | String | Foreign key to target collection |
| `origin_key_target` | String | Origin target field |
| `foreign_key_target` | String | Foreign target field |

---

## Hooks

### collection.add_hook(position, type, handler)

Add a hook to execute code before or after operations.

```ruby
collection.add_hook('Before', 'Create', ->(context) {
  # Validate data before creation
  if context.data['email'].nil?
    raise 'Email is required'
  end
})
```

**Hook Types:**

- `'List'` - Before/after listing records
- `'Create'` - Before/after creating records
- `'Update'` - Before/after updating records
- `'Delete'` - Before/after deleting records
- `'Aggregate'` - Before/after aggregating data

**Example - Before Hook:**

```ruby
collection.add_hook('Before', 'Create', ->(context) {
  # Set default values
  context.data['status'] ||= 'active'
  context.data['created_by'] = context.caller.id
})
```

**Example - After Hook:**

```ruby
collection.add_hook('After', 'Update', ->(context) {
  # Send notification after update
  records = context.collection.list(context.filter, ['email'])
  records.each do |record|
    NotificationService.send_update_email(record['email'])
  end
})
```

---

## Charts

### collection.add_chart(name, definition)

Add a chart to the collection.

```ruby
collection.add_chart('total_revenue', ->(context, result_builder) {
  total = Order.sum(:total)
  result_builder.value(total)
})
```

**Chart Types:**

**Value Chart:**
```ruby
collection.add_chart('user_count', ->(context, result_builder) {
  count = User.count
  result_builder.value(count)
})
```

**Distribution Chart:**
```ruby
collection.add_chart('users_by_plan', ->(context, result_builder) {
  distribution = User.group(:plan).count
  result_builder.distribution(distribution)
})
```

**Time-based Chart:**
```ruby
collection.add_chart('signups_over_time', ->(context, result_builder) {
  data = User.group_by_day(:created_at).count
  result_builder.time_based('Day', data)
})
```

**Percentage Chart:**
```ruby
collection.add_chart('completion_rate', ->(context, result_builder) {
  completed = Task.where(status: 'completed').count
  total = Task.count
  rate = (completed.to_f / total * 100).round(2)
  result_builder.percentage(rate)
})
```

**Objective Chart:**
```ruby
collection.add_chart('sales_goal', ->(context, result_builder) {
  current = Order.sum(:total)
  target = 100_000
  result_builder.objective(current, target)
})
```

**Leaderboard Chart:**
```ruby
collection.add_chart('top_sellers', ->(context, result_builder) {
  top = User.joins(:orders)
    .group('users.name')
    .sum('orders.total')
  result_builder.leaderboard(top)
})
```

---

## Search & Sorting

### collection.replace_search(definition)

Replace the default search behavior.

```ruby
collection.replace_search(->(search_string) {
  {
    aggregator: 'Or',
    conditions: [
      { field: 'first_name', operator: 'Contains', value: search_string },
      { field: 'last_name', operator: 'Contains', value: search_string },
      { field: 'email', operator: 'Contains', value: search_string }
    ]
  }
})
```

---

### collection.disable_search

Disable search functionality on the collection.

```ruby
collection.disable_search
```

---

### collection.replace_field_sorting(name, equivalent_sort)

Replace sorting implementation for a field.

```ruby
collection.replace_field_sorting('full_name', [
  { field: 'last_name', ascending: true },
  { field: 'first_name', ascending: true }
])
```

---

## Form Field Types

When defining action forms, you can use various field types.

```ruby
collection.add_action('Example', {
  scope: 'Single',
  form: [
    {
      label: 'User Name',
      type: 'String',
      is_required: true,
      description: 'Enter the user name'
    },
    {
      label: 'Age',
      type: 'Number',
      default_value: 18
    },
    {
      label: 'Is Active',
      type: 'Boolean',
      default_value: true
    },
    {
      label: 'Birth Date',
      type: 'Date'
    },
    {
      label: 'Status',
      type: 'Enum',
      enum_values: ['pending', 'approved', 'rejected']
    }
  ],
  execute: ->(context, result_builder) {
    values = context.form_values
    # ... action logic
    result_builder.success
  }
})
```

**Available Types:**

- `'String'` - Text input
- `'Number'` - Numeric input
- `'Boolean'` - Checkbox
- `'Date'` - Date picker
- `'Dateonly'` - Date without time
- `'Enum'` - Single selection
- `'EnumList'` - Multiple selection
- `'File'` - File upload
- `'Json'` - JSON editor
- `'Collection'` - Record picker

**Collection Field:**

```ruby
{
  label: 'Assign to User',
  type: 'Collection',
  collection_name: 'User'
}
```

**Conditional Fields:**

```ruby
form: [
  {
    label: 'Notification Type',
    type: 'Enum',
    enum_values: ['email', 'sms', 'push']
  },
  {
    label: 'Email Address',
    type: 'String',
    if: ->(context) { context.form_values['notification_type'] == 'email' }
  }
]
```

---

## Complete Example

```ruby
# config/initializers/forest_admin.rb
require 'forest_admin_agent'

ForestAdmin.agent = ForestAdminAgent::Agent.new do |options|
  options.auth_secret = ENV.fetch('FOREST_AUTH_SECRET')
  options.env_secret = ENV.fetch('FOREST_ENV_SECRET')
  options.is_production = Rails.env.production?
  options.logger = Rails.logger
end

ForestAdmin.agent.add_datasource(
  ForestAdminDatasourceActiveRecord::Datasource.new
)

# Customize User collection
ForestAdmin.agent.customize_collection('User') do |collection|
  # Add computed field
  collection.add_field('full_name', {
    column_type: 'String',
    dependencies: ['first_name', 'last_name'],
    get_values: ->(records) {
      records.map { |r| "#{r['first_name']} #{r['last_name']}" }
    }
  })

  # Add segment
  collection.add_segment('Active users', {
    field: 'status',
    operator: 'Equal',
    value: 'active'
  })

  # Add action
  collection.add_action('Send promotional email', {
    scope: 'Bulk',
    form: [
      {
        label: 'Campaign',
        type: 'Enum',
        enum_values: ['summer', 'winter', 'black_friday']
      },
      {
        label: 'Discount',
        type: 'Number',
        is_required: true
      }
    ],
    execute: ->(context, result_builder) {
      campaign = context.form_values['campaign']
      discount = context.form_values['discount']
      users = context.get_records(['email'])

      users.each do |user|
        PromotionalMailer.campaign_email(
          user['email'],
          campaign,
          discount
        ).deliver_later
      end

      result_builder.success("Email sent to #{users.length} users")
    }
  })

  # Add hook
  collection.add_hook('Before', 'Create', ->(context) {
    context.data['status'] ||= 'active'
    context.data['created_by'] = context.caller.id
  })

  # Add chart
  collection.add_chart('user_count', ->(context, result_builder) {
    count = User.count
    result_builder.value(count)
  })
end

ForestAdmin.agent.start
```

---

## Available Operators

**Comparison:**
- `'Equal'`, `'NotEqual'`
- `'GreaterThan'`, `'LessThan'`
- `'In'`, `'NotIn'`
- `'Present'`, `'Blank'`

**String:**
- `'Contains'`, `'NotContains'`
- `'StartsWith'`, `'EndsWith'`
- `'Like'`, `'ILike'` (case-insensitive)

**Date:**
- `'Before'`, `'After'`
- `'Today'`, `'Yesterday'`
- `'PreviousWeek'`, `'PreviousMonth'`, `'PreviousQuarter'`, `'PreviousYear'`
- `'Past'`, `'Future'`

---

## Context Objects

### Caller

User information available in all contexts:

```ruby
context.caller.id          # User ID
context.caller.email       # User email
context.caller.first_name  # User first name
context.caller.last_name   # User last name
context.caller.team        # User team
context.caller.role        # User role
context.caller.timezone    # User timezone
```

---

## Related Documentation

- [Ruby Setup Guide](/product/integration/setup/ruby)
- [Actions Guide](/product/process/actions/overview)
- [Fields Guide](/product/process/fields/overview)
- [Segments Guide](/product/process/segments/overview)
- [Hooks Guide](/product/process/for-tech-teams/hooks/overview)
