Forest Admin provides extensive customization options for your collections, allowing you to tailor CRUD operations, implement business logic, and enhance data integrity through two powerful features:

- [Collection Hooks](./collection-hook.md)
- [Collection Overrides](./collection-override.md)

Both features offer unique capabilities to developers, from augmenting existing behaviors to completely redefining how collection operations are executed.

{% hint style="info" %}
These two features are almost identical, but they are executed at very different stages of the customizations.
This means that Collection Hooks will be executed even if you choose to use Collection Override.
{% endhint %}

# Collection hooks

Collection Hooks allow you to execute custom code before or after CRUD operations, giving you the ability to enforce business rules, or integrate with external services seamlessly.

## Features

- **Pre** and **Post** Operation Execution: Execute custom logic before or after a specific collection operation.
- **Flexible Trigger Points**: Hook into any of the standard CRUD operations (`list`, `create`, `update`, `delete`, `aggregate`).
- **Contextual Information**: Access to a rich high level context providing details about the operation, enabling precise and informed logic execution.

[Learn More About Collection Hooks](./collection-hook.md)

Dive deeper into Collection Hooks, understand how to implement them, and explore advanced use cases to make the most out of your collections.

# Collection overrides

Collection Overrides provide the ability to completely replace the default behavior of CUD operations. This powerful feature allows for custom implementations of `create`, `update`, and `delete` operations, offering unparalleled control over data handling.

## Features

- **Complete Control Over CUD**: Directly replace the standard behavior of `create`, `update`, and `delete` operations with custom logic.
- **Custom Operation Logic**: Implement entirely custom workflows or integrate external services directly into your CUD operations.
- **Full Operation Context**: Receive detailed low level context about the operation, enabling complex logic and integrations.

[Learn More About Collection Overrides](./collection-override.md)

Explore the capabilities of Collection Overrides, learn how to set them up, and discover how they can offer complete control over your data operations.
