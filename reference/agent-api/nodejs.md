---
title: "Agent API reference"
description: "Complete API reference for the Forest Admin Node.js Agent"
---

Complete API reference for `@forestadmin/agent` (Node.js/TypeScript).

## Agent class

### Createagent(options)

Create and configure a Forest Admin agent.

```typescript

const agent = createAgent(options: AgentOptions): Agent;
```

**Parameters:**

| Option | Type | Required | Description |
|--------|------|----------|-------------|
| `authSecret` | string | Yes | Your FOREST_AUTH_SECRET |
| `envSecret` | string | Yes | Your FOREST_ENV_SECRET |
| `isProduction` | boolean | No | Enable production mode |
| `logger` | function | No | Custom logger function |
| `loggerLevel` | string | No | Log level: 'Debug', 'Info', 'Warn', 'Error' |
| `prefix` | string | No | API prefix (default: '/forest') |
| `schemaPath` | string | No | Path to .forestadmin-schema.json |

**Example:**

```typescript
const agent = createAgent({
  authSecret: process.env.FOREST_AUTH_SECRET,
  envSecret: process.env.FOREST_ENV_SECRET,
  isProduction: process.env.NODE_ENV === 'production',
});
```

---

### Agent.adddatasource(factory, options?)

Add a datasource to the agent.

```typescript
agent.addDataSource(
  factory: DataSourceFactory,
  options?: DataSourceOptions
): Agent;
```

**Parameters:**

| Option | Type | Description |
|--------|------|-------------|
| `factory` | DataSourceFactory | Datasource factory function |
| `options.include` | string[] | Collections to include |
| `options.exclude` | string[] | Collections to exclude |
| `options.rename` | Record\<string, string\> | Rename collections |

**Example:**

```typescript

agent.addDataSource(
  createSqlDataSource(process.env.DATABASE_URL),
  { exclude: ['internal_logs'] }
);
```

---

### Agent.customizecollection(name, callback)

Customize a specific collection with the provided callback.

```typescript
agent.customizeCollection(
  name: string,
  callback: (collection: CollectionCustomizer) => void
): Agent;
```

**Example:**

```typescript
agent.customizeCollection('users', collection => {
  collection.addAction('Send email', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      // Action logic
      return resultBuilder.success('Email sent!');
    },
  });
});
```

---

### Agent.addchart(name, definition)

Create a datasource-level API chart.

```typescript
agent.addChart(
  name: string,
  definition: DataSourceChartDefinition
): Agent;
```

**Example:**

```typescript
agent.addChart('overview', (context, resultBuilder) => {
  return resultBuilder.distribution({
    'Active': 150,
    'Inactive': 50,
  });
});
```

---

### Agent.removecollection(...names)

Remove collections from the exported schema (they remain usable within the agent).

```typescript
agent.removeCollection(...names: string[]): Agent;
```

**Example:**

```typescript
agent.removeCollection('internalLogs', 'debugData');
```

---

### Agent.use(plugin, options?)

Load a plugin across all collections.

```typescript
agent.use<Options>(
  plugin: Plugin<Options>,
  options?: Options
): Agent;
```

**Example:**

```typescript

agent.use(advancedExportPlugin, { format: 'xlsx' });
```

---

### Agent.start()

Start the agent and connect to Forest Admin servers.

```typescript
await agent.start(): Promise<void>;
```

**Example:**

```typescript
await agent.start();
console.log('Agent started successfully');
```

---

### Agent.stop()

Stop the agent and close all connections.

```typescript
await agent.stop(): Promise<void>;
```

---

### Agent.updatetypesonfilesystem(typingspath, typingsmaxdepth)

Update the TypeScript typings file generated from your datasources.

```typescript
await agent.updateTypesOnFileSystem(
  typingsPath: string,
  typingsMaxDepth: number
): Promise<void>;
```

**Example:**

```typescript
await agent.updateTypesOnFileSystem('./typings.ts', 5);
```

---

## Collection customizer

Methods available when customizing a collection through `agent.customizeCollection()`.

## Actions

### Collection.addaction(name, definition)

Add a Smart Action to the collection.

```typescript
collection.addAction(
  name: string,
  definition: ActionDefinition
): CollectionCustomizer;
```

**Definition Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `scope` | 'Single' \| 'Bulk' \| 'Global' | Action scope |
| `execute` | function | Action execution handler |
| `form` | FormElement[] | Dynamic form configuration |
| `description` | string | Action description |
| `generateFile` | boolean | Whether action returns a file |
| `submitButtonLabel` | string | Custom button text |

**Execute Function:**

```typescript
execute: (
  context: ActionContext,
  resultBuilder: ResultBuilder
) => Promise<ActionResult>
```

**ActionContext Properties:**

- `context.collection` - Collection instance
- `context.filter` - Filter for selected records
- `context.caller` - User who triggered the action
- `context.formValues` - Form values submitted

**ActionContext Methods:**

- `context.getRecords(fields)` - Get multiple records (Bulk/Single scope)
- `context.getRecordIds()` - Get IDs of selected records
- `context.getCompositeRecordIds()` - Get composite IDs of selected records
- `context.hasFieldChanged(fieldName)` - Check if form field changed
- `context.getRecord(fields)` - Get single record (Single scope only)
- `context.getRecordId()` - Get single record ID (Single scope only)
- `context.getCompositeRecordId()` - Get composite ID (Single scope only)
- `context.getField(fieldName)` - Get single field value (Single scope only)

**ResultBuilder Methods:**

- `resultBuilder.success(message?, options?)` - Success response
  - `options.html` - Custom HTML to display
  - `options.invalidated` - Array of collection names to refresh
- `resultBuilder.error(message?, options?)` - Error response
  - `options.html` - Custom HTML to display
- `resultBuilder.webhook(url, method, headers, body)` - Trigger webhook
- `resultBuilder.file(stream, filename, mimeType)` - Return file download
- `resultBuilder.redirectTo(path)` - Redirect to URL
- `resultBuilder.setHeader(name, value)` - Add HTTP header to response

**Example - Simple Action:**

```typescript
collection.addAction('Mark as verified', {
  scope: 'Single',
  execute: async (context, resultBuilder) => {
    const user = await context.getRecord(['id']);
    await updateUser(user.id, { verified: true });
    return resultBuilder.success('User marked as verified');
  },
});
```

**Example - Action with Form:**

```typescript
collection.addAction('Send notification', {
  scope: 'Bulk',
  form: [
    {
      label: 'Message',
      type: 'String',
      isRequired: true,
    },
    {
      label: 'Channel',
      type: 'Enum',
      enumValues: ['email', 'sms', 'push'],
      isRequired: true,
    },
  ],
  execute: async (context, resultBuilder) => {
    const { message, channel } = context.formValues;
    const users = await context.getRecords(['email']);

    for (const user of users) {
      await sendNotification(user.email, message, channel);
    }

    return resultBuilder.success(`Sent ${channel} to ${users.length} users`);
  },
});
```

**Example - File Generation:**

```typescript
collection.addAction('Export to PDF', {
  scope: 'Bulk',
  generateFile: true,
  execute: async (context, resultBuilder) => {
    const records = await context.getRecords(['name', 'email']);
    const pdfStream = await generatePDF(records);

    return resultBuilder.file(pdfStream, 'export.pdf', 'application/pdf');
  },
});
```

---

## Fields

### Collection.addfield(name, definition)

Add a computed field to the collection.

```typescript
collection.addField(
  name: string,
  definition: ComputedDefinition
): CollectionCustomizer;
```

**Definition Properties:**

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `columnType` | ColumnType | Yes | Field data type |
| `dependencies` | string[] | Yes | Fields needed for computation |
| `getValues` | function | Yes | Value computation function |
| `defaultValue` | any | No | Default value |
| `enumValues` | string[] | No | Enum options (if type is Enum) |

**Column Types:**

- `'String'` - Text
- `'Number'` - Numeric value
- `'Boolean'` - True/false
- `'Date'` - Date with time
- `'Dateonly'` - Date without time
- `'Time'` - Time only
- `'Enum'` - Enumeration
- `'Json'` - JSON object
- `'Uuid'` - UUID
- `'Point'` - Geographic point
- `'File'` - File reference

**Example - Simple Computed Field:**

```typescript
collection.addField('fullName', {
  columnType: 'String',
  dependencies: ['firstName', 'lastName'],
  getValues: (records) =>
    records.map(r => `${r.firstName} ${r.lastName}`),
});
```

**Example - Async Computed Field:**

```typescript
collection.addField('revenueThisYear', {
  columnType: 'Number',
  dependencies: ['id'],
  getValues: async (records) => {
    const ids = records.map(r => r.id);
    const revenues = await fetchRevenues(ids);
    return revenues;
  },
});
```

---

### Collection.importfield(name, options)

Import a field from a related collection.

```typescript
collection.importField(
  name: string,
  options: { path: string; readonly?: boolean }
): CollectionCustomizer;
```

**Example:**

```typescript
// Import author's name into books collection
collection.importField('authorName', {
  path: 'author:fullName',
  readonly: true,
});
```

---

### Collection.renamefield(currentname, newname)

Rename a field in the exported schema.

```typescript
collection.renameField(
  currentName: string,
  newName: string
): CollectionCustomizer;
```

**Example:**

```typescript
collection.renameField('created_at', 'createdAt');
```

---

### Collection.removefield(...names)

Remove fields from the exported schema (they remain usable within the agent).

```typescript
collection.removeField(...names: string[]): CollectionCustomizer;
```

**Example:**

```typescript
collection.removeField('password', 'internalNotes', 'debugData');
```

---

### Collection.addfieldvalidation(name, operator, value?)

Add a validation rule to a field.

```typescript
collection.addFieldValidation(
  name: string,
  operator: Operator,
  value?: any
): CollectionCustomizer;
```

**Operators:**

`'Present'`, `'LongerThan'`, `'ShorterThan'`, `'Contains'`, `'Like'`, `'Match'`, `'GreaterThan'`, `'LessThan'`, `'Before'`, `'After'`

**Example:**

```typescript
collection
  .addFieldValidation('email', 'Present')
  .addFieldValidation('email', 'Match', /^[\w-\.]+@([\w-]+\.)+[\w-]{2,4}$/)
  .addFieldValidation('age', 'GreaterThan', 18)
  .addFieldValidation('username', 'LongerThan', 3);
```

---

### Collection.setfieldnullable(name)

Mark a field as optional (nullable).

```typescript
collection.setFieldNullable(name: string): CollectionCustomizer;
```

{% hint style="warning" %}
Your database might still refuse empty values if it requires one.
{% endhint %}

**Example:**

```typescript
collection.setFieldNullable('middleName');
```

---

### Collection.replacefieldwriting(name, definition)

Replace the write behavior of a field.

```typescript
collection.replaceFieldWriting(
  name: string,
  definition: WriteDefinition
): CollectionCustomizer;
```

**Example:**

```typescript
// Write fullName as firstName + lastName
collection.replaceFieldWriting('fullName', fullName => {
  const [firstName, lastName] = fullName.split(' ');
  return { firstName, lastName };
});
```

---

### Collection.replacefieldbinarymode(name, mode)

Choose how binary data should be transported to the GUI.

```typescript
collection.replaceFieldBinaryMode(
  name: string,
  mode: 'datauri' | 'hex'
): CollectionCustomizer;
```

**Modes:**
- `'datauri'` - Best for file uploads, uses FilePicker widget
- `'hex'` - Best for short binary data like UUIDs

**Example:**

```typescript
collection.replaceFieldBinaryMode('avatar', 'datauri');
collection.replaceFieldBinaryMode('uuid', 'hex');
```

---

## Relationships

### Collection.addmanytoonerelation(name, foreigncollection, options)

Add a many-to-one relationship.

```typescript
collection.addManyToOneRelation(
  name: string,
  foreignCollection: string,
  options: {
    foreignKey: string;
    foreignKeyTarget?: string;
  }
): CollectionCustomizer;
```

**Example:**

```typescript
// books.authorId → persons.id
books.addManyToOneRelation('author', 'persons', {
  foreignKey: 'authorId',
});
```

---

### Collection.addonetomanyrelation(name, foreigncollection, options)

Add a one-to-many relationship.

```typescript
collection.addOneToManyRelation(
  name: string,
  foreignCollection: string,
  options: {
    originKey: string;
    originKeyTarget?: string;
  }
): CollectionCustomizer;
```

**Example:**

```typescript
// persons.id ← books.authorId
persons.addOneToManyRelation('writtenBooks', 'books', {
  originKey: 'authorId',
});
```

---

### Collection.addonetoonerelation(name, foreigncollection, options)

Add a one-to-one relationship.

```typescript
collection.addOneToOneRelation(
  name: string,
  foreignCollection: string,
  options: {
    originKey: string;
    originKeyTarget?: string;
  }
): CollectionCustomizer;
```

**Example:**

```typescript
// persons.id ← profiles.personId (unique)
persons.addOneToOneRelation('profile', 'profiles', {
  originKey: 'personId',
});
```

---

### Collection.addmanytomanyrelation(name, foreigncollection, throughcollection, options)

Add a many-to-many relationship.

```typescript
collection.addManyToManyRelation(
  name: string,
  foreignCollection: string,
  throughCollection: string,
  options: {
    originKey: string;
    foreignKey: string;
    originKeyTarget?: string;
    foreignKeyTarget?: string;
  }
): CollectionCustomizer;
```

**Example:**

```typescript
// students ↔ student_courses ↔ courses
students.addManyToManyRelation('enrolledCourses', 'courses', 'student_courses', {
  originKey: 'studentId',
  foreignKey: 'courseId',
});
```

---

### Collection.addexternalrelation(name, definition)

Add a virtual collection into the related data of a record.

```typescript
collection.addExternalRelation(
  name: string,
  definition: ExternalRelationDefinition
): CollectionCustomizer;
```

**Example:**

```typescript
collection.addExternalRelation('relatedProducts', {
  schema: { id: 'Number', name: 'String', price: 'Number' },
  listRecords: async ({ id }) => {
    return await fetchRelatedProducts(id);
  },
});
```

---

## Segments

### Collection.addsegment(name, definition)

Add a segment (saved filter) to the collection.

```typescript
collection.addSegment(
  name: string,
  definition: SegmentDefinition
): CollectionCustomizer;
```

**Example - Static Segment:**

```typescript
collection.addSegment('Premium users', {
  field: 'plan',
  operator: 'Equal',
  value: 'premium',
});
```

**Example - Dynamic Segment:**

```typescript
collection.addSegment('Active this month', async (context) => {
  const startOfMonth = new Date();
  startOfMonth.setDate(1);

  return {
    field: 'lastActiveAt',
    operator: 'After',
    value: startOfMonth,
  };
});
```

---

## Hooks

### Collection.addhook(position, type, handler)

Add a hook to execute code before or after operations.

```typescript
collection.addHook(
  position: 'Before' | 'After',
  type: HookType,
  handler: HookHandler
): CollectionCustomizer;
```

**Hook Types:**

`'List'`, `'Create'`, `'Update'`, `'Delete'`, `'Aggregate'`

**Example - Before Hook:**

```typescript
collection.addHook('Before', 'Create', async (context) => {
  // Validate data before creation
  if (!context.data.email) {
    throw new Error('Email is required');
  }
});
```

**Example - After Hook:**

```typescript
collection.addHook('After', 'Update', async (context) => {
  // Send notification after update
  const records = await context.collection.list(context.filter, ['email']);
  for (const record of records) {
    await sendUpdateNotification(record.email);
  }
});
```

---

## Search

### Collection.replacesearch(definition)

Replace the default search behavior.

```typescript
collection.replaceSearch(
  definition: SearchDefinition
): CollectionCustomizer;
```

**Example:**

```typescript
collection.replaceSearch(async (searchString) => {
  // Search in multiple fields
  return {
    aggregator: 'Or',
    conditions: [
      { field: 'firstName', operator: 'Contains', value: searchString },
      { field: 'lastName', operator: 'Contains', value: searchString },
      { field: 'email', operator: 'Contains', value: searchString },
    ],
  };
});
```

---

### Collection.disablesearch()

Disable search functionality on the collection.

```typescript
collection.disableSearch(): CollectionCustomizer;
```

---

## Sorting

### Collection.emulatefieldsorting(name)

Enable in-memory sorting on a field.

```typescript
collection.emulateFieldSorting(name: string): CollectionCustomizer;
```

**Example:**

```typescript
collection.emulateFieldSorting('fullName');
```

---

### Collection.replacefieldsorting(name, equivalentsort)

Replace sorting implementation for a field.

```typescript
collection.replaceFieldSorting(
  name: string,
  equivalentSort: SortClause[]
): CollectionCustomizer;
```

**Example:**

```typescript
collection.replaceFieldSorting('fullName', [
  { field: 'lastName', ascending: true },
  { field: 'firstName', ascending: true },
]);
```

---

### Collection.disablefieldsorting(name)

Disable sorting on a specific field.

```typescript
collection.disableFieldSorting(name: string): CollectionCustomizer;
```

---

## Filtering

### Collection.emulatefieldfiltering(name)

Enable in-memory filtering on all operators for a field.

```typescript
collection.emulateFieldFiltering(name: string): CollectionCustomizer;
```

---

### Collection.emulatefieldoperator(name, operator)

Enable in-memory filtering for a specific operator on a field.

```typescript
collection.emulateFieldOperator(
  name: string,
  operator: Operator
): CollectionCustomizer;
```

**Example:**

```typescript
collection.emulateFieldOperator('fullName', 'Contains');
```

---

### Collection.replacefieldoperator(name, operator, replacer)

Replace the implementation of a filter operator.

```typescript
collection.replaceFieldOperator(
  name: string,
  operator: Operator,
  replacer: OperatorDefinition
): CollectionCustomizer;
```

**Example:**

```typescript
collection.replaceFieldOperator('fullName', 'Contains', (value) => {
  return {
    aggregator: 'Or',
    conditions: [
      { field: 'firstName', operator: 'Contains', value },
      { field: 'lastName', operator: 'Contains', value },
    ],
  };
});
```

---

## Charts

### Collection.addchart(name, definition)

Add a chart to the collection.

```typescript
collection.addChart(
  name: string,
  definition: ChartDefinition
): CollectionCustomizer;
```

**Example - Value Chart:**

```typescript
collection.addChart('totalRevenue', async (context, resultBuilder) => {
  const total = await calculateTotalRevenue();
  return resultBuilder.value(total);
});
```

**Example - Distribution Chart:**

```typescript
collection.addChart('usersByPlan', async (context, resultBuilder) => {
  const distribution = await getUserDistributionByPlan();
  return resultBuilder.distribution(distribution);
});
```

**Example - Time-based Chart:**

```typescript
collection.addChart('signupsOverTime', async (context, resultBuilder) => {
  const data = await getSignupsOverTime(context.timezone);
  return resultBuilder.timeBased(data);
});
```

---

## Collection overrides

### Collection.overridecreate(handler)

Replace the default create operation.

```typescript
collection.overrideCreate(
  handler: CreateOverrideHandler
): CollectionCustomizer;
```

**Example:**

```typescript
collection.overrideCreate(async (context) => {
  const { data } = context;

  // Custom creation logic
  const record = await customCreateAPI(data);

  return [record];
});
```

---

### Collection.overrideupdate(handler)

Replace the default update operation.

```typescript
collection.overrideUpdate(
  handler: UpdateOverrideHandler
): CollectionCustomizer;
```

**Example:**

```typescript
collection.overrideUpdate(async (context) => {
  const { filter, patch } = context;

  // Custom update logic
  await customUpdateAPI(filter, patch);
});
```

---

### Collection.overridedelete(handler)

Replace the default delete operation.

```typescript
collection.overrideDelete(
  handler: DeleteOverrideHandler
): CollectionCustomizer;
```

**Example:**

```typescript
collection.overrideDelete(async (context) => {
  const { filter } = context;

  // Custom deletion logic (e.g., soft delete)
  await customSoftDeleteAPI(filter);
});
```

---

## Other methods

### Collection.disablecount()

Disable count in list view pagination for improved performance.

```typescript
collection.disableCount(): CollectionCustomizer;
```

---

### Collection.use(plugin, options?)

Load a plugin on a specific collection.

```typescript
collection.use(
  plugin: Plugin,
  options?: any
): CollectionCustomizer;
```

**Example:**

```typescript

collection.use(createFileField, {
  fieldname: 'avatar',
  bucket: 'my-bucket',
});
```

---

## Chart result builders

When creating charts with `collection.addChart()` or `agent.addChart()`, the result builder provides methods to format chart data.

### Resultbuilder.value(value, previousvalue?)

Create a Value chart (single number).

```typescript
collection.addChart('totalRevenue', async (context, resultBuilder) => {
  const total = await calculateRevenue();
  const previous = await calculateRevenue(lastMonth);
  return resultBuilder.value(total, previous);
});
```

---

### Resultbuilder.distribution(obj)

Create a Distribution/Pie chart.

```typescript
collection.addChart('usersByPlan', async (context, resultBuilder) => {
  return resultBuilder.distribution({
    'Free': 1000,
    'Pro': 500,
    'Enterprise': 50,
  });
});
```

---

### Resultbuilder.timebased(timerange, values)

Create a Time-based/Line chart.

```typescript
collection.addChart('signupsOverTime', async (context, resultBuilder) => {
  return resultBuilder.timeBased('Day', [
    { date: new Date('2024-01-01'), value: 10 },
    { date: new Date('2024-01-02'), value: 15 },
    { date: new Date('2024-01-03'), value: null }, // Missing data
  ]);
});
```

**Time Ranges:** `'Day'`, `'Week'`, `'Month'`, `'Quarter'`, `'Year'`

---

### Resultbuilder.multipletimebased(timerange, dates, lines)

Create a Multi-line Time-based chart.

```typescript
collection.addChart('comparison', async (context, resultBuilder) => {
  const dates = [new Date('2024-01-01'), new Date('2024-01-02'), new Date('2024-01-03')];

  return resultBuilder.multipleTimeBased('Day', dates, [
    { label: 'Sales', values: [100, 150, 200] },
    { label: 'Returns', values: [10, 15, null] },
  ]);
});
```

---

### Resultbuilder.percentage(value)

Create a Percentage chart.

```typescript
collection.addChart('completionRate', async (context, resultBuilder) => {
  const rate = (completed / total) * 100;
  return resultBuilder.percentage(rate);
});
```

---

### Resultbuilder.objective(value, objective)

Create an Objective chart (progress toward goal).

```typescript
collection.addChart('salesGoal', async (context, resultBuilder) => {
  const current = await getCurrentSales();
  const target = 100000;
  return resultBuilder.objective(current, target);
});
```

---

### Resultbuilder.leaderboard(obj)

Create a Leaderboard chart (sorted distribution).

```typescript
collection.addChart('topSellers', async (context, resultBuilder) => {
  return resultBuilder.leaderboard({
    'John': 5000,
    'Jane': 7500,
    'Bob': 3000,
  });
  // Automatically sorted: Jane (7500), John (5000), Bob (3000)
});
```

---

### Resultbuilder.smart(data)

Create a Smart chart (custom format).

```typescript
collection.addChart('custom', async (context, resultBuilder) => {
  return resultBuilder.smart({
    // Custom chart data structure
    type: 'custom',
    data: [/* your data */],
  });
});
```

---

## Form field types

When defining action forms, you can use various field types with different widgets.

### Basic field types

```typescript
collection.addAction('Example', {
  scope: 'Single',
  form: [
    {
      label: 'User Name',
      type: 'String',
      isRequired: true,
      description: 'Enter the user name',
    },
    {
      label: 'Age',
      type: 'Number',
      isRequired: false,
      defaultValue: 18,
    },
    {
      label: 'Is Active',
      type: 'Boolean',
      defaultValue: true,
    },
    {
      label: 'Birth Date',
      type: 'Date',
    },
    {
      label: 'Metadata',
      type: 'Json',
    },
  ],
  execute: async (context, resultBuilder) => {
    const { userName, age, isActive, birthDate, metadata } = context.formValues;
    // ... action logic
    return resultBuilder.success();
  },
});
```

**Available Types:**

- `'String'` - Text input
- `'Number'` - Numeric input
- `'Boolean'` - Checkbox
- `'Date'` - Date picker
- `'Dateonly'` - Date without time
- `'Time'` - Time picker
- `'Enum'` - Single selection
- `'EnumList'` - Multiple selection
- `'File'` - File upload
- `'FileList'` - Multiple file upload
- `'Json'` - JSON editor
- `'Collection'` - Record picker
- `'NumberList'` - Array of numbers
- `'StringList'` - Array of strings

---

### Enum fields

```typescript
{
  label: 'Status',
  type: 'Enum',
  enumValues: ['pending', 'approved', 'rejected'],
  isRequired: true,
}
```

---

### Collection fields

Pick a record from another collection:

```typescript
{
  label: 'Assign to User',
  type: 'Collection',
  collectionName: 'users',
}
```

---

### Dropdown widget

```typescript
{
  label: 'Country',
  type: 'String',
  widget: 'Dropdown',
  options: [
    { label: 'United States', value: 'US' },
    { label: 'United Kingdom', value: 'UK' },
    { label: 'France', value: 'FR' },
  ],
  placeholder: 'Select a country',
  search: 'static', // or 'disabled'
}
```

---

### Dynamic dropdown

```typescript
{
  label: 'Product',
  type: 'String',
  widget: 'Dropdown',
  search: 'dynamic',
  options: async (context, searchValue) => {
    const products = await searchProducts(searchValue);
    return products.map(p => ({
      label: p.name,
      value: p.id,
    }));
  },
}
```

---

### Conditional fields

Show/hide fields based on other field values:

```typescript
form: [
  {
    label: 'Notification Type',
    type: 'Enum',
    enumValues: ['email', 'sms', 'push'],
  },
  {
    label: 'Email Address',
    type: 'String',
    if: (context) => context.formValues.notificationType === 'email',
  },
  {
    label: 'Phone Number',
    type: 'String',
    if: (context) => context.formValues.notificationType === 'sms',
  },
]
```

---

### Dynamic field values

```typescript
{
  label: 'Department',
  type: 'Enum',
  enumValues: async (context) => {
    // Fetch departments based on selected company
    const companyId = context.formValues.companyId;
    return await getDepartments(companyId);
  },
}
```

---

### Read-only fields

```typescript
{
  label: 'Created At',
  type: 'Date',
  isReadOnly: true,
  value: async (context) => {
    const record = await context.getRecord(['createdAt']);
    return record.createdAt;
  },
}
```

---

## Conditiontree utilities

Build complex filter conditions programmatically.

### Conditiontreeleaf

Simple condition on a single field:

```typescript

const condition = new ConditionTreeLeaf('status', 'Equal', 'active');
// Equivalent to: { field: 'status', operator: 'Equal', value: 'active' }
```

---

### Conditiontreebranch

Combine multiple conditions with AND/OR:

```typescript

const condition = new ConditionTreeBranch('And', [
  new ConditionTreeLeaf('status', 'Equal', 'active'),
  new ConditionTreeLeaf('age', 'GreaterThan', 18),
]);
```

---

### Conditiontree factory

```typescript

// From plain object
const condition = ConditionTreeFactory.fromPlainObject({
  field: 'email',
  operator: 'Contains',
  value: '@example.com',
});

// Combine conditions
const combined = ConditionTreeFactory.intersect([
  condition1,
  condition2,
]);

// Union (OR)
const union = ConditionTreeFactory.union([
  condition1,
  condition2,
]);
```

---

### Available operators

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
- `'Today'`, `'Yesterday'`, `'PreviousWeek'`, `'PreviousMonth'`, `'PreviousQuarter'`, `'PreviousYear'`
- `'Past'`, `'Future'`

**Array:**
- `'IncludesAll'`, `'IncludesNone'`

---

## Related types

### Caller

User information available in all contexts:

```typescript
interface Caller {
  id: number;
  email: string;
  firstName: string;
  lastName: string;
  team: string;
  role: string;
  tags: Record<string, string>;
  timezone: string;
}
```

---

### Filter

```typescript

const filter = new Filter({
  conditionTree: new ConditionTreeLeaf('status', 'Equal', 'active'),
  search: 'john',
  searchExtended: false,
  segment: 'premium-users',
});
```

---

### Projection

Specify which fields to retrieve:

```typescript

const projection = new Projection('id', 'name', 'email', 'company:name');
```
