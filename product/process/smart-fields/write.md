Forest Admin allows replacing the default field writing behavior with your own custom logic.

This is useful when you want to change how a given field behaves, but also to make [computed fields](./computed.md) writable.

## How does it work

The {{#nodejs,php}}`replaceFieldWriting`{{/nodejs,php}}{{#python,ruby}}`replace_field_writing`{{/python,ruby}} function allows to change the behavior of any change by creating a new patch that will be applied to the record.

You should refrain from using handlers that have side effects (to perform error handling, validation, ...) and [use hooks instead](../hooks/README.md).

## Making a field read-only

{% hint style="info" %}
Can be achieved without any code [in the field settings](https://docs.forestadmin.com/user-guide/collections/customize-your-fields#basic-settings).
{% endhint %}

## Examples

### Changing other fields in the same record

In the following example, editing or creating a `fullName` will update both `firstName` and `lastName` fields of the record.

```javascript
collection.replaceFieldWriting('fullName', value => {
  const [firstName, lastName] = value.split(' ');

  return { firstName, lastName };
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldWriting(
        	'fullName',
        	function($value) {
        		[$firstName, $lastName] = explode(' ', $value);

        		return compact('firstName', 'lastName');
        	}
        );
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_writing('fullName') do |value|
    first_name, last_name = value.split(' ')
    {
      'firstName' => first_name,
      'lastName' => last_name
    }
  end
end
```

```python
from forestadmin.datasource_toolkit.decorators.write.write_replace.write_customization_context import (
    WriteCustomizationContext,
)
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias

def full_name_write_fn(
    value, context: WriteCustomizationContext
) -> RecordsDataAlias:
    first_name, last_name = value.split(' ')
    return {"firstName": first_name, "LastName": last_name}


collection.replace_field_writing('fullName', full_name_write_fn)
```

### Having specific behavior only for updates

You can have different behavior for `creations` and `updates`.

In this example, each time the `firstName` field is edited, we also want to update a timestamp field.

```javascript
collection.replaceFieldWriting('firstName', async (value, context) => {
  switch (context.action) {
    case 'create':
      return { firstName, firstNameLastEdited: null };

    case 'update':
      return { firstName, firstNameLastEdited: new Date().toISOString() };

    default:
      throw new Error('Unexpected value');
  }
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;
use ForestAdmin\AgentPHP\DatasourceCustomizer\Decorators\Write\WriteReplace\WriteCustomizationContext;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldWriting(
        	'firstName',
        	function($value,  WriteCustomizationContext $context) {
        		return match ($context->getAction()) {
        			'create' => ['firstName' => $value, 'firstNameLastEdited' => null],
        			'update' => ['firstName' => $value, 'firstNameLastEdited' => 'updated' => date('c')],
        			default => throw new ForestException('invalid action'),
                };
        	}
        );
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_writing('firstName') do |value, context|
    case context.action
    when 'create'
      {
        'firstName' => value,
        'firstNameLastEdited' => nil
      }
    when 'update'
      {
        'firstName' => value,
        'firstNameLastEdited' => Time.now.iso8601
      }
    else
      raise 'Unexpected value'
    end
  end
end
```

```python
from datetime import date
from forestadmin.datasource_toolkit.decorators.write.write_replace.write_customization_context import (
    WriteCustomizationContext,
)
from forestadmin.datasource_toolkit.interfaces.records import RecordsDataAlias


def first_name_write_fn(
    value, context: WriteCustomizationContext
) -> RecordsDataAlias:
    if context.action == "create":
        return {"firstName": value, "firstNameLastEdited": None}
    elif context.action == "update":
        return {"firstName": value, "firstNameLastEdited": date.today().isoformat()}
    else:
      raise Exception("Unexpected value")


collection.replace_field_writing('firstName', first_name_write_fn)
```

### Changing fields in related records

{% hint style="info" %}
Handling relationships inside a {{#nodejs,php,ruby}}`replaceFieldWriting`{{/nodejs,php,ruby}}{{#python,ruby}}`replace_field_writing`{{/python,ruby}} will only work for `ManyToOne` and `OneToOne` relationships.
{% endhint %}

In this simple example, we have two collections that are linked together:

- The `Users` collection has a `job` and a `portfolioId` as foreignKey
- The `Portfolios` collection has a `title`

When the user updates his `job` field we want also to update the `title` of the portfolio by the `job` name.

```javascript
collection.replaceFieldWriting('job', (job, { action }) => {
  return { job, portfolio: { title: job } };
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldWriting(
        	'job',
        	function($value) {
        		return ['job' => $value, 'portfolio' => ['title' => $value]];
        	}
        );
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_writing('job') do |value, _context|
    {
      'job' => value,
      'portfolio' => { 'title' => value }
    }
  end
end

```

```python
collection.replace_field_writing(
    "job", lambda value, context: {"job": value, "portfolio": {"title": value}}
)
```

{% hint style="info" %}
If the relationships do not exist it will create them with the given field values.
{% endhint %}

You can also provide another `portfolioId` to update the relationships and their fields:

```javascript
collection.replaceFieldWriting('job', (job, { action }) => {
  return { job, portfolioId: 8, portfolio: { title: job } };
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldWriting(
        	'job',
        	function($value) {
        		return ['job' => $value, 'portfolioId' => 8, 'portfolio' => ['title' => $value]];
        	}
        );
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_writing('job') do |value, _context|
    {
      'job' => value,
      'portfolioId' => 8,
      'portfolio' => { 'title' => value }
    }
  end
end
```

```python
collection.replace_field_writing(
    "job",
    lambda value, context: {
        "job": value,
        "portfolioId": 8,
        "portfolio": {"title": value},
    },
)
```

Of course, you can chain the relationships. For example, if a portfolio has a `one-to-one` relationship
with the `formats` collection, you can update it by writing the right path.

```javascript
collection.replaceFieldWriting('job', (job, { action }) => {
  return { job, portfolioId: 8, portfolio: { title: job, format: { name: 'pdf' } } };
});
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;
use ForestAdmin\AgentPHP\DatasourceToolkit\Components\Query\ConditionTree\Operators;

$forestAgent->customizeCollection(
    'Customer',
    function (CollectionCustomizer $builder) {
        $builder->replaceFieldWriting(
        	'job',
        	function($value) {
        		return [
        			'job' => $value,
        			'portfolioId' => 8,
        			'portfolio' => [
        				'title' => $value,
        				'format' => [
        					'name' => 'pdf'
        				]
        			]
        		];
        	}
        );
    }
);
```

```ruby
@create_agent.customize_collection('customer') do |collection|
  collection.replace_field_writing('job') do |value, _context|
    {
      'job' => value,
      'portfolioId' => 8,
      'portfolio' => {
        'title' => value,
        'format' => { 'name' => 'pdf' }
      }
    }
  end
end
```

```python
collection.replace_field_writing(
    "job",
    lambda value, context: {
        "job": value,
        "portfolioId": 8,
        "portfolio": {"title": value, "format": {"name": "pdf"}},
    },
)
```
