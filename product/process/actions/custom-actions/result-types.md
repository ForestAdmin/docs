Actions can be configured to achieve different results in the GUI.

Most actions will simply perform work and display the default notification, however, other behaviors are possible:

- [Displaying a notification with a custom message](#custom-notifications)
- [Displaying HTML content in a side panel](#html-result)
- [Generating a file download](#file-generation)
- [Redirecting the user to another page](#redirections)
- [Calling a webhook from the user's browser](#webhooks) (for instance to trigger a login in a third-party application)
  {{#nodejs,python,php}}
- [Setting up response headers](#response-headers)
  {{/nodejs,python,php}}
- [Invalidating related data](./related-data-invalidation.md)

## Default behavior

The default behavior, when no exception is thrown in the handler is to display a generic notification.

<img src="../../assets/actions-default-success-result.png" width="300">

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async context => {
      // Not using the resultBuilder here will display the generic success
      // notification (as long as no exception is thrown)
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('company') do |collection|
  collection.add_action(
    'Mark as live',
    BaseAction.new(scope: ActionScope::SINGLE) do |context|
      # Not using the resultBuilder here will display the generic success notification.
      # (as long as no exception is thrown)
    end
  )
end

```

</details>


## Custom notifications

When customizing the notification message, you can use the {{#nodejs}}`resultBuilder`{{/nodejs}}{{#ruby}}`ForestAdminDatasourceCustomizer::Decorators::Action::ResultBuilder`{{/ruby}} to generate different types of responses.

<img src="../../assets/actions-custom-success-result.png" width="300">
<img src="../../assets/actions-custom-error-result.png" width="300">

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      const isLive = false; // Company is not live

      if (!isLive) {
        // The success method will display a success notification.
        return resultBuilder.success('Company is now live!');
      } else {
        // The error method will display an error notification.
        return resultBuilder.error('The company was already live!');
      }
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('company') do |collection|
  collection.add_action(
    'Mark as live',
    BaseAction.new(scope: ActionScope::SINGLE) do |_context, result_builder|
      if # Company is not live
        result_builder.success(message: 'Company is now live!')
      else
        result_builder.error(message: 'The company was already live!')
      end
    end
  )
end
```

</details>


## HTML result

You can also return an HTML page to give more feedback to the user who triggered the Action.

![](../../assets/actions-html-result-success.png)

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Charge credit card', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      /* ... charge the credit card ... */
      const wasCharged = true; // the credit card was successfully charged

      if (wasCharged) {
        return resultBuilder.success('Success', {
          html: `
            <p class="c-clr-1-4 l-mt l-mb">
              \$${record.amount / 100} USD has been successfuly charged.
            </p>
            <strong class="c-form__label--read c-clr-1-2">Credit card</strong>
            <p class="c-clr-1-4 l-mb">**** **** **** ${record.source.last4}</p>
          `,
        });
      } else {
        return resultBuilder.error('An error occured', {
          html: `
            <p class="c-clr-1-4 l-mt l-mb">
              \$${record.amount / 100} USD has not been charged.
            </p>
            <strong class="c-form__label--read c-clr-1-2">Credit card</strong>
            <p class="c-clr-1-4 l-mb">**** **** **** ${record.source.last4}</p>
            <strong class="c-form__label--read c-clr-1-2">Reason</strong>
            <p class="c-clr-1-4 l-mb">
              You can not charge this credit card. The card is marked as blocked
            </p>
          `,
        });
      }
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('company') do |collection|
  collection.add_action(
    'Charge credit card',
    BaseAction.new(scope: ActionScope::SINGLE) do |context, result_builder|
      # ... charge the credit card ...
      record = context.get_record(['amount', 'source:last4'])
      if credit_card_successfully_charged
        result_builder.success(
          message: 'Success',
          options: {
            html: "<p class='c-clr-1-4 l-mt l-mb'>#{record['amount'] / 100} USD has been successfully charged.</p>
              <strong class='c-form__label--read c-clr-1-2'>Credit card</strong>
              <p class='c-clr-1-4 l-mb'>**** **** **** #{record['source']['last4']}</p>"
          }
        )
      else
        result_builder.error(
          message: 'An error occured',
          options: {
            html: "<p class='c-clr-1-4 l-mt l-mb'>#{record['amount'] / 100} USD has not been charged.</p>
              <strong class='c-form__label--read c-clr-1-2'>Credit card</strong>
              <p class='c-clr-1-4 l-mb'>**** **** **** #{record['source']['last4']}</p>
              <strong class='c-form__label--read c-clr-1-2'>Reason</strong>
              <p class='c-clr-1-4 l-mb'>You can not charge this credit card. The card is marked as blocked</p>"
          }
        )
      end
    end
  )
end
```

</details>


## File generation

{% hint style="warning" %}
Because of technical limitations, Smart Actions that generate files should be flagged as such with the `generateFile` option.

This will cause the GUI to download the output of the action, but will also prevent from being able to use the `resultBuilder` to display notifications, errors, or HTML content.
{% endhint %}

Smart actions can be used to generate or download files.

The example code below will trigger a file download (with the file named `filename.txt`, containing `StringThatWillBeInTheFile` using `text/plain` mime-type).

<details>
<summary><strong>collection.addAction('Download a file', {</strong></summary>

```javascript
  scope: 'Global',
  // This option is required to trigger the file download.
  generateFile: true,

  execute: async (context, resultBuilder) => {
    const random = Math.random();

    if (random < 0.33) {
      // Files can be generated from JavaScript strings.
      return resultBuilder.file(
        'StringThatWillBeInTheFile',
        'filename.txt',
        'text/plain',
      );
    } else if (random < 0.66) {
      // Or from a Buffer.
      const buffer = Buffer.from('StringThatWillBeInTheFile');
      return resultBuilder.file(buffer, 'filename.txt', 'text/plain');
    } else {
      // Or from a stream.
      const stream = fs.createReadStream('path/to/file');
      return resultBuilder.file(stream, 'filename.txt', 'text/plain');
    }
  },
});
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('company') do |collection|
  collection.add_action(
    'Download a file',
    BaseAction.new(scope: ActionScope::GLOBAL, is_generate_file: true) do |_context, result_builder|
      my_file = File.open('filename.txt', 'w')
      my_file.write('StringThatWillBeInTheFile')
      my_file.close

      result_builder.file(content: 'filename.txt', name: 'filename.txt', mime_type: 'text/plain')
    end
  )
end
```

</details>


## Redirections

To streamline your operation workflow, it could make sense to redirect to another page after an Action has successfully been executed.

It is possible using the `redirectTo` function.

The redirection works both for internal (`\*.forestadmin.com` pages) and external links.

{% tabs %} {% tab title="Internal link" %}

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      return resultBuilder.redirectTo(
        '/MyProject/MyEnvironment/MyTeam/data/20/index/record/20/108/activity',
      );
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context
```

</details>

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      return resultBuilder.redirectTo(
        'https://www.royalmail.com/portal/rm/track?trackNumber=ZW924750388GB',
      );
    },
  }),
);
```

</details>


{% endtab %} {% endtabs %}

## Webhooks

After an action you can set up an HTTP (or HTTPS) callback - a webhook - to forward information to other applications.

Note that the webhook will be triggered from the user's browser, so it will be subject to CORS restrictions.

Its intended use is often to perform a login on a third-party application or to trigger a background job on the current user's behalf.

<details>
<summary><strong>agent.customizeCollection('companies', collection =></strong></summary>

```javascript
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      return resultBuilder.webhook(
        'http://my-company-name', // Webhook URL.
        'POST', // Webhook method (typically a POST).
        {}, // Webhook headers if needed.
        { adminToken: 'your-admin-token' }, // Webhook body (only JSON is supported).
      );
    },
  }),
);
```

</details>

<details>
<summary><strong>include ForestAdminDatasourceCustomizer::Decorators::Action::Types</strong></summary>

```ruby
include ForestAdminDatasourceCustomizer::Decorators::Action::Context

@create_agent.customize_collection('company') do |collection|
  collection.add_action(
    'Mark as live',
    BaseAction.new(scope: ActionScope::SINGLE) do |_context, result_builder|
      result_builder.webhook(
        url: 'http://my-company-name', # The url of the company providing the service.
        method: 'POST', # The method you would like to use (typically a POST).
        headers: {}, # You can add some headers if needed.
        body: { adminToken: 'your-admin-token' } # A body to send to the url.
      )
    end
  )
end
```

</details>


{{#nodejs,python}}

{% hint style="warning" %}
Please note that the webhook function and the setHeader function operate independently and do not modify the same HTTP call. Webhook headers will be sent along with the webhook call, while setHeaders will modify directly the Action response headers.
{% endhint %}

## Response headers

Sometimes you may want to setup custom response headers after action execution, the {{#nodejs,php}}`setHeader`{{/nodejs,php}} function is here to reach out this goal.

Before executing any end function described above, you should be able to add headers to the action response like the exemple below.

```javascript
agent.customizeCollection('companies', collection =>
  collection.addAction('Mark as live', {
    scope: 'Single',
    execute: async (context, resultBuilder) => {
      return resultBuilder
        .setHeader('myHeaderName', 'myHeaderValue')
        .redirectTo(
          'https://www.royalmail.com/portal/rm/track?trackNumber=ZW924750388GB',
        );
    },
  }),
);
```


{{/nodejs,python}}
