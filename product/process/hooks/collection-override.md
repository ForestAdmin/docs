
{{#nodejs,ruby}}
Forest Admin allows customizing at a very low level the behavior of any given Collection via the usage of Collection Overrides.

{% hint style="info" %}
Collection Overrides provide a powerful means to completely replace the default behavior of CUD operations (`create`, `update`, `delete`) for your Collections. This feature should be used with caution, as it directly affects the core operations on your data.
{% endhint %}

## How it Works

In addition to the standard Collection functions:

- `create`
- `update`
- `delete`

Collection Overrides allow you to define custom behavior that will entirely replace the default implementation of the `create`, `update`, and `delete` operations.

To define an Override for a Collection, you must specify:

- The handler function that will be executed instead of the default operation.

The custom handler function will receive a context object containing relevant information for the operation, allowing for comprehensive control over the behavior of these CUD operations.

## Setting Up Overrides

Overrides are declared similarly to hooks but are aimed at replacing an entire operation rather than augmenting its execution. However this can also be used to enrich the default behavior. Here's how to set up overrides in your Collection:

### Custom Create Operation

To replace the default create operation, use {{#nodejs}}`overrideCreate`{{/nodejs}}{{#ruby}}`override_create`{{/ruby}} with your custom handler:

{{#nodejs}}
{% hint style="warning" %}
Unknown properties in returned records will be removed.
{% endhint %}
{{/nodejs}}

<details>
<summary><strong>collection.overrideCreate(async context => {</strong></summary>

```javascript
  // Custom logic to handle creation
  // context.data contains the data intended for creation
  // Return an array of created records
});
```

</details>

<details>
<summary><strong>collection.override_create do |context|</strong></summary>

```ruby
  # Custom logic to handle creation
  # context.data contains the data intended for creation
  # Return an array of created records
end
```

</details>

<details>
<summary><strong>collection.overrideUpdate(async context => {</strong></summary>

```javascript
  // Custom logic to handle update
  // context.filter to determine which records are targeted
  // context.patch contains the data for update
  // Perform update operation
});
```

</details>

<details>
<summary><strong>collection.override_update do |context|</strong></summary>

```ruby
  # Custom logic to handle update
  # context.filter to determine which records are targeted
  # context.patch contains the data for update
  # Perform update operation
end

```

</details>

<details>
<summary><strong>collection.overrideDelete(async context => {</strong></summary>

```javascript
  // Custom logic to handle deletion
  // context.filter to determine which records are targeted
  // Perform deletion operation
});
```

</details>

<details>
<summary><strong>collection.override_delete do |context|</strong></summary>

```ruby
  # Custom logic to handle deletion
  # context.filter to determine which records are targeted
  # Perform deletion operation
end
```

</details>


{% hint style="warning" %}
Overrides take precedence over the default operation. Ensure your custom handlers properly manage all necessary logic for the operation, as the default behavior will not be executed.
{% endhint %}

## Basic Use Cases

### Create over API

You might want to create the record with your custom API:

<details>
<summary><strong>const { MissingFieldError } = require('@forestadmin/datasource-toolkit');</strong></summary>

```javascript
product.overrideCreate(async context => {
  const { data } = context;

  if (data.some(product => !product.name)) {
    throw new MissingFieldError('name', 'products');
  }

  const response = await fetch('https://my-product-api.com/products', {
    method: 'POST',
    body: data,
  });
  const products = await response.json();

  // structure is an array of Partial<Product>
  // [ { name: 'CoffeeMaker3000, price: "$300" } ]
  return products;
});
```

</details>

<details>
<summary><strong>product.override_create do |context|</strong></summary>

```ruby
  response = HTTParty.post("https://my-product-api.com/products", body: context.data)
  response.parsed_response
end
```

</details>

<details>
<summary><strong>product.overrideUpdate(async context => {</strong></summary>

```javascript
  const { patch } = context;

  // Execute data modification and validation only if one of name or slug was edited
  if (patch.name || patch.slug) {
    const name = patch.name || patch.slug.split('-')[0];
    const uuid = await fetch('https://my-product-api.com/slug', {
      method: 'GET',
      body: { name, slug },
    });

    patch.name = name;
    patch.slug = `${name}-${uuid}`;
  }

  await context.collection.update(context.filter, context.patch);
});
```

</details>

<details>
<summary><strong>product.override_update do |context|</strong></summary>

```ruby
  # Execute data modification and validation only if one of name or slug was edited
  if context.patch.key?('name') || context.patch.key?('slug')
    name = context.patch['name'] || context.patch['slug'].split("-")[0]
    uuid = HTTParty.get('https://my-product-api.com/slug', body: { name: name }).parsed_response

    context.patch['name'] = name
    context.patch['slug'] = "#{name}-#{uuid}"
  end

  context.collection.update(context.filter, context.patch)
end
```

</details>


{{/nodejs,ruby}}
