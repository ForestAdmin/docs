---
title: Override writing behavior
---

Forest Admin allows replacing the default field writing behavior with your own custom logic.

This is useful when you want to change how a given field behaves, but also to make computed fields writable.

## How does it work

The `replaceFieldWriting` (or `replace_field_writing` in Ruby) function allows to change the behavior of any change by creating a new patch that will be applied to the record.

You should refrain from using handlers that have side effects (to perform error handling, validation, ...) and use hooks instead for these purposes.

## Making a field read-only

{% hint style="info" %}
Can be achieved without any code [in the field settings](https://docs.forestadmin.com/user-guide/collections/customize-your-fields#basic-settings).
{% endhint %}

## Examples

### Changing other fields in the same record

In the following example, editing or creating a `fullName` will update both `firstName` and `lastName` fields of the record.

{% tabs %}
{% tab title="Node.js" %}
```javascript
collection.replaceFieldWriting('fullName', value => {
  const [firstName, lastName] = value.split(' ');

  return { firstName, lastName };
});
```
{% endtab %}

{% tab title="Ruby" %}
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
{% endtab %}
{% endtabs %}

### Having specific behavior only for updates

You can have different behavior for `creations` and `updates`.

In this example, each time the `firstName` field is edited, we also want to update a timestamp field.

{% tabs %}
{% tab title="Node.js" %}
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
{% endtab %}

{% tab title="Ruby" %}
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
{% endtab %}
{% endtabs %}

### Changing fields in related records

{% hint style="info" %}
Handling relationships inside a `replaceFieldWriting``replace_field_writing` will only work for `ManyToOne` and `OneToOne` relationships.
{% endhint %}

In this simple example, we have two collections that are linked together:

- The `Users` collection has a `job` and a `portfolioId` as foreignKey
- The `Portfolios` collection has a `title`

When the user updates his `job` field we want also to update the `title` of the portfolio by the `job` name.

{% tabs %}
{% tab title="Node.js" %}
```javascript
collection.replaceFieldWriting('job', (job, { action }) => {
  return { job, portfolio: { title: job } };
});
```
{% endtab %}

{% tab title="Ruby" %}
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
{% endtab %}
{% endtabs %}

{% hint style="info" %}
If the relationships do not exist it will create them with the given field values.
{% endhint %}

You can also provide another `portfolioId` to update the relationships and their fields:

{% tabs %}
{% tab title="Node.js" %}
```javascript
collection.replaceFieldWriting('job', (job, { action }) => {
  return { job, portfolioId: 8, portfolio: { title: job } };
});
```
{% endtab %}

{% tab title="Ruby" %}
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
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Node.js" %}
```javascript
collection.replaceFieldWriting('job', (job, { action }) => {
  return { job, portfolioId: 8, portfolio: { title: job, format: { name: 'pdf' } } };
});
```
{% endtab %}

{% tab title="Ruby" %}
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
{% endtab %}
{% endtabs %}
