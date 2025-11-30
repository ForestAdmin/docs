Forest Admin allows customizing at a very low level the behavior of any given Collection via the usage of Collection Hooks.

{% hint style="info" %}

Collection Hooks are a very powerful feature and require special care when using them.

{% endhint %}

## How it works

Any given Collection should implement all of the following functions:

- `list`
- `create`
- `update`
- `delete`
- `aggregate`

The Collection Hooks feature allows executing code before and/or after any of these functions, providing an easy way to interact with your Collections.

To declare a Hook on a Collection, the following information is required:

- A lifecycle position (`Before` | `After`)
- An action type (`List` | `Create` | `Update` | `Delete` | `Aggregate`)
- A callback, that will receive a context matching the provided hook position and hook definition.

{% hint style="warning" %}

A single Collection can have multiple Hooks with the same position and the same type. They will run in their declaration order.
Collection Hooks are only called when the Collection function is contacted by the UI. This means that any usage of the Forest Admin [query interface](../../datasources/getting-started/queries/README.md) will not trigger them.

{% endhint %}

## Basic use cases

In the following example, we want to prevent a set of users from updating any records of the `Transactions` table. We want to check if the user email is allowed to update a record via an external API call.

<details>
<summary><strong>transaction.addHook('Before', 'Update', async context => {</strong></summary>

```javascript
  // context.caller contains information about the current user, the defined
  // timezone, etc.
  // In this case, context.caller.email is the email used in Forest Admin by the user
  // that initiated the call
  const isAllowed = await myFunctionToCheckIfUserIsAllowed(context.caller.email);
  if (!isAllowed) {
    // Raising an error here will prevent the execution of the update function,
    // as well as any other hooks that may be defined afterwards.
    context.throwForbiddenError(`${context.caller.email} is not allowed!`);
  }
});
```

</details>

<details>
<summary><strong>@create_agent.customize_collection('transactions') do |collection|</strong></summary>

```ruby
  collection.add_hook('Before', 'Update') do |context|
    # context.caller contains information about the current user, the defined timezone, etc.
    # In this case, context.caller.email is the email used in Forest Admin by the user that initiated the call
    is_allowed = my_function_to_check_user_is_allowed(context.caller.email)
    unless is_allowed
      # Raising an error here will prevent the execution of the update function,
      # as well as any other hooks that may be defined afterwards.
      context.throw_forbidden_error("#{context.caller.email} is not allowed!")
    end
  end
end
```

</details>

<details>
<summary><strong>user.addHook('After', 'Create', async (context, responseBuilder) => {</strong></summary>

```javascript
  // The result of the create function always return an array of records
  const userEmail = context.records[0]?.email;
  await MyEmailSender.sendEmail({
    from: 'erlich@bachman.com',
    to: userEmail,
    message: 'Hey, a new account was created with this email.',
  });
});
```

</details>

<details>
<summary><strong>@create_agent.customize_collection('user') do |collection|</strong></summary>

```ruby
  collection.add_hook('After', 'Create') do |context|
    # The result of the create function always return a record
    email = context.record['email']
    MyEmailSender.send_email(
      from: 'erlich@bachman.com',
      to: email,
      message: 'Hey, a new account was created with this email.'
    )
  end
end
```

</details>


