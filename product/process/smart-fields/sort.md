Depending on the data source, not all fields may be sortable, or you may want to change how the native sorting works.

By using the {{#nodejs,php}}`replaceFieldSorting`{{/nodejs,php}}{{#python,ruby}}`replace_field_sorting`{{/python,ruby}} and {{#nodejs,php}}`emulateFieldSorting`{{/nodejs,php}}{{#python,ruby}}`emulate_field_sorting`{{/python,ruby}} methods, you can change a single column's sorting behavior.

### Substitution

You can also provide replacement sort clauses. In this example, we're telling Forest Admin "When a user sorts by full name, I want to sort by the last name, and then by the first name".

```javascript
collection.replaceFieldSorting('fullName', [
  { field: 'lastName', ascending: true },
  { field: 'firstName', ascending: true },
]);
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldSorting(
        	'fullName',
        	[
        		['field' => 'lastName', 'ascending' => true],
        		['field' => 'firstName', 'ascending' => true],
            ]
        );
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_sorting('fullName', [
    { field: 'lastName', ascending: true },
    { field: 'firstName', ascending: true },
  ])
end
```

```python
collection.replace_field_sorting(
    "fullName",
    [
        {"field": "lastName", "ascending": True},
        {"field": "firstName", "ascending": True},
    ],
)
```

Another very common reason is performance. For instance, with auto-incrementing ids, sorting by `creationDate` is equivalent to sorting by the primary key in reverse order.

Using sort substitution where needed can save you from adding many indexes to your database.

```javascript
// Sorting by creationDate ascending <=> Sorting by id descending
collection.replaceFieldSorting('creationDate', [{ field: 'id', ascending: false }]);
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldSorting('creationDate', [['field' => 'id', 'ascending' => false]]);
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_sorting('creationDate', [{ field: 'id', ascending: false }])
end

```

```python
collection.replace_field_sorting(
    "creationDate",
    [
        {"field": "id", "ascending": False},
    ],
)
```

### Emulation

Sorting emulation allows making any field automatically sortable. It will sort records by lexicographical order.
It is a convenient way to get things working quickly for Collections that have a low number of records (in the thousands at most).

{% hint style="warning" %}
This emulation forces the Agent to retrieve all the Collection records and compute the field values for each one of them.
As a consequence, sorting emulation performance cost is **linear** with the number of records in the Collection, so **activate it sparingly and with great care**.
{% endhint %}

```javascript
collection.emulateFieldSorting('fullName');
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->emulateFieldSorting('fullName');
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.emulate_field_sorting('fullName')
end
```

```python
collection.emulate_field_sorting("fullName")
```
