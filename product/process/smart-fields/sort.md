Depending on the data source, not all fields may be sortable, or you may want to change how the native sorting works.

By using the {{#nodejs}}`replaceFieldSorting`{{/nodejs}}{{#ruby}}`replace_field_sorting`{{/ruby}} and {{#nodejs}}`emulateFieldSorting`{{/nodejs}}{{#ruby}}`emulate_field_sorting`{{/ruby}} methods, you can change a single column's sorting behavior.

### Substitution

You can also provide replacement sort clauses. In this example, we're telling Forest Admin "When a user sorts by full name, I want to sort by the last name, and then by the first name".

{% tabs %}
{% tab title="collection.replaceFieldSorting('fullName', [" %}
```javascript
{ field: 'lastName', ascending: true },
  { field: 'firstName', ascending: true },
]);
```
{% endtab %}

{% tab title="@create_agent.customize_collection('customer') do |collection|" %}
```ruby
collection.replace_field_sorting('fullName', [
    { field: 'lastName', ascending: true },
    { field: 'firstName', ascending: true },
  ])
end
```
{% endtab %}
{% endtabs %}

<details>
<summary><strong>// Sorting by creationDate ascending <=> Sorting by id descending</strong></summary>

```javascript
collection.replaceFieldSorting('creationDate', [{ field: 'id', ascending: false }]);
```

</details>

{% tabs %}
{% tab title="@create_agent.customize_collection('customer') do |collection|" %}
```ruby
collection.replace_field_sorting('creationDate', [{ field: 'id', ascending: false }])
end

```

</details>


### Emulation

Sorting emulation allows making any field automatically sortable. It will sort records by lexicographical order.
It is a convenient way to get things working quickly for Collections that have a low number of records (in the thousands at most).

{% hint style="warning" %}
This emulation forces the Agent to retrieve all the Collection records and compute the field values for each one of them.
As a consequence, sorting emulation performance cost is **linear** with the number of records in the Collection, so **activate it sparingly and with great care**.
{% endhint %}

<details>
<summary><strong>collection.emulateFieldSorting('fullName');</strong></summary>

```javascript
```
{% endtab %}

{% tab title="@create_agent.customize_collection('customer') do |collection|" %}
```ruby
collection.emulate_field_sorting('fullName')
end
```
{% endtab %}
{% endtabs %}


