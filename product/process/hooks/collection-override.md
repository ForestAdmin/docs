{{#php}}

## Coming soon...

{{/php}}
{{#nodejs,python,ruby}}
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

To replace the default create operation, use {{#nodejs}}`overrideCreate`{{/nodejs}}{{#python,ruby}}`override_create`{{/python,ruby}} with your custom handler:

{{#nodejs}}
{% hint style="warning" %}
Unknown properties in returned records will be removed.
{% endhint %}
{{/nodejs}}

```javascript
collection.overrideCreate(async context => {
  // Custom logic to handle creation
  // context.data contains the data intended for creation
  // Return an array of created records
});
```

```ruby
collection.override_create do |context|
  # Custom logic to handle creation
  # context.data contains the data intended for creation
  # Return an array of created records
end
```

```python
from forestadmin.datasource_toolkit.decorators.override.context import CreateOverrideCustomizationContext

async def create_handle(context: CreateOverrideCustomizationContext)
    # Custom logic to handle creation
    # context.data contains the data intended for creation
    # Return an array of created records

collection.override_create(create_handle)
```

### Custom Update Operation

To replace the default update operation, use {{#nodejs}}`overrideUpdate`{{/nodejs}}{{#python}}`override_update`{{/python}} with your custom handler:

```javascript
collection.overrideUpdate(async context => {
  // Custom logic to handle update
  // context.filter to determine which records are targeted
  // context.patch contains the data for update
  // Perform update operation
});
```

```ruby
collection.override_update do |context|
  # Custom logic to handle update
  # context.filter to determine which records are targeted
  # context.patch contains the data for update
  # Perform update operation
end

```

```python
from forestadmin.datasource_toolkit.decorators.override.context import UpdateOverrideCustomizationContext

async def update_handle(context: UpdateOverrideCustomizationContext)
    # Custom logic to handle update
    # context.filter to determine which records are targeted
    # context.patch contains the data for update
    # Perform update operation

collection.override_update(update_handle)
```

### Custom Delete Operation

To replace the default delete operation, use {{#nodejs}}`overrideDelete`{{/nodejs}}{{#python}}`override_delete`{{/python}} with your custom handler:

```javascript
collection.overrideDelete(async context => {
  // Custom logic to handle deletion
  // context.filter to determine which records are targeted
  // Perform deletion operation
});
```

```ruby
collection.override_delete do |context|
  # Custom logic to handle deletion
  # context.filter to determine which records are targeted
  # Perform deletion operation
end
```

```python
from forestadmin.datasource_toolkit.decorators.override.context import DeleteOverrideCustomizationContext

async def delete_handle(context: DeleteOverrideCustomizationContext)
    # Custom logic to handle deletion
    # context.filter to determine which records are targeted
    # Perform deletion operation

collection.override_delete(delete_handle)
```

{{#python}}
{% hint style="info" %}
In theses examples, we use async function as handler, but you can also use sync or lambda function.
{% endhint %}
{{/python}}

{% hint style="warning" %}
Overrides take precedence over the default operation. Ensure your custom handlers properly manage all necessary logic for the operation, as the default behavior will not be executed.
{% endhint %}

## Basic Use Cases

### Create over API

You might want to create the record with your custom API:

```javascript
const { MissingFieldError } = require('@forestadmin/datasource-toolkit');

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

```ruby
product.override_create do |context|
  response = HTTParty.post("https://my-product-api.com/products", body: context.data)
  response.parsed_response
end
```

```python
import requests
from forestadmin.datasource_toolkit.decorators.override.context import CreateOverrideCustomizationContext

def create_handle(context: CreateOverrideCustomizationContext)
    response = requests.post("https://my-product-api.com/products", json=context.data)
    return response.json()

collection.override_create(create_handle)
```

### Modify data before update

You might want to modify payload data before update your record:

```javascript
product.overrideUpdate(async context => {
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

```ruby
product.override_update do |context|
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

```python
import requests
from forestadmin.datasource_toolkit.decorators.override.context import UpdateOverrideCustomizationContext

async def update_handle(context: UpdateOverrideCustomizationContext)
    # Execute data modification and validation only if one of name or slug was edited
    if "name" in context.patch or "slug" in context.patch:
        name = context.get("name", context["slug"].split("-")[0])
        response = requests.get("https://my-product-api.com/slug", json={"name": name})
        uuid = response.text

        context.patch["name"] = name
        context.patch["slug"] = f"{name}-{uuid}"

    await context.collection.update(context.filter, context.patch)

collection.override_update(update_handle)
```

{{/nodejs,python,ruby}}
