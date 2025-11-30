---
title: Move, rename and remove fields
---

When building your admin panel, you will probably want to hide as much complexity from your users as you can.

This includes:

- Hiding technical and confidential fields
- Using naming conventions that the final user understands.

## Moving fields

You can import fields from single record relationships (many-to-one or one-to-one) into your collections.

The imported fields will behave as if they were on that collection.

<details>
<summary><strong>Node.js</strong></summary>

```javascript
// Assuming the following structure:
// User    { id, firstName, lastName, addressId }
// Address { id, streetName, streetNumber, city, countryId }
// Country { id, name }

userCollection
  .importField('city', { path: 'address:city', readonly: true })
  .importField('country', { path: 'address:country:name', readonly: true });
```

</details>

<details>
<summary><strong>Ruby</strong></summary>

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

</details>

{% hint style="info" %}
Note that when using `readonly: false`, the referenced record fields can be edited.
{% endhint %}

## Renaming and removing fields and relations

Renaming and removing fields or relations can be done simply by calling the `renameField` (or `rename_field` in Ruby) and `removeField` (or `remove_field` in Ruby) methods.

<details>
<summary><strong>Node.js</strong></summary>

```javascript
collection.renameField('account_v3_uuid_new', 'account').removeField('password');
```

</details>

<details>
<summary><strong>Ruby</strong></summary>

```ruby
@create_agent.customize_collection('user') do |collection|
  collection.rename_field('account_v3_uuid_new', 'account').remove_field('password')
end

```

</details>

{% hint style="warning" %}
Renamed and removed fields are renamed and removed ONLY in the admin panel.

In your code:

- Removed fields are still accessible (for instance, as dependencies to compute new fields)
- Renamed fields must still be referred to by using their original name.
{% endhint %}
