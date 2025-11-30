When building your admin panel, you will probably want to hide as much complexity from your users as you can.

This includes:

- Hiding technical and confidential fields
- Using naming conventions that the final user understands.

# Moving fields

You can import fields from [single record relationships](../relationships/single-record.md) into your collections.

The imported fields will behave as if they were on that collection.

```javascript
// Assuming the following structure:
// User    { id, firstName, lastName, addressId }
// Address { id, streetName, streetNumber, city, countryId }
// Country { id, name }

userCollection
  .importField('city', { path: 'address:city', readonly: true })
  .importField('country', { path: 'address:country:name', readonly: true });
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;

// Assuming the following structure:
// User    { id, firstName, lastName, addressId }
// Address { id, streetName, streetNumber, city, countryId }
// Country { id, name }

$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->importField('city', ['path' => 'address:city', 'readonly' => true])
            ->importField('country', ['path' => 'address:country:name', 'readonly' => true]);
    }
);
```

```ruby
# Assuming the following structure:
# User    { id, firstName, lastName, addressId }
# Address { id, streetName, streetNumber, city, countryId }
# Country { id, name }
@create_agent.customize_collection('user') do |collection|
  collection.import_field('city', { path: 'address:city', readonly: true })
            .import_field('country', { path: 'address:country:name', readonly: true })
end
```

```python
# Assuming the following structure:
# User    { id, firstName, lastName, addressId }
# Address { id, streetName, streetNumber, city, countryId }
# Country { id, name }
agent.customize_collection("User").import_field(
    "city", {"path": "address:city", "readonly": True}
).import_field("country", {"path": "address:country:name", "readonly": True})
```

{% hint style="info" %}
Note that when using `readonly: false`, the referenced record fields can be edited.
{% endhint %}

# Renaming and removing fields and relations

Renaming and removing fields or relations can be done simply by calling the {{#nodejs,php}}`renameField`{{/nodejs,php}}{{#python,ruby}}`rename_field`{{/python,ruby}} and {{#nodejs,php}}`removeField`{{/nodejs,php}}{{#python,ruby}}`remove_field`{{/python,ruby}} methods.

```javascript
collection.renameField('account_v3_uuid_new', 'account').removeField('password');
```

```php
use ForestAdmin\AgentPHP\DatasourceCustomizer\CollectionCustomizer;

$forestAgent->customizeCollection(
    'User',
    function (CollectionCustomizer $builder) {
        $builder->renameField('account_v3_uuid_new', 'account')->removeField('password');
    }
);
```

```ruby
@create_agent.customize_collection('user') do |collection|
  collection.rename_field('account_v3_uuid_new', 'account').remove_field('password')
end

```

```python
# Assuming the following structure:
# User    { id, firstName, lastName, addressId }
# Address { id, streetName, streetNumber, city, countryId }
# Country { id, name }
user_collection.rename_field('account_v3_uuid_new', 'account').remove_field(
    'password'
)
```

{% hint style="warning" %}
Renamed and removed fields are renamed and removed ONLY in the admin panel.

In your code:

- Removed fields are still accessible (for instance, as dependencies to compute new fields)
- Renamed fields must still be referred to by using their original name.

{% endhint %}
