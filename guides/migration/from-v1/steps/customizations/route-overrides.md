[Route overrides](https://docs.forestadmin.com/documentation/reference-guide/routes/override-a-route) allowed customizing the behavior of the routes exposed by the agent.

This very low-level feature was used to implement many use cases:

- Attach handlers to events in the UI
- Customize filtering, search and sort behaviors
- Other advanced use cases.

Because our new agent API is higher-level, the protocol used to communicate between the agent and the application can no longer be manipulated.

# Code cheatsheet

| What was the route override used for?   | How to migrate it?                                                                                                                                                                    |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add custom permissions                  | Use [{{#nodejs}}.addHook(){{/nodejs}}{{#ruby}}.add_hook{{/ruby}}](../../../../agent-customization/hooks/README.md) and throw `forbidden errors`                 |
| Add validation to fields                | Use [{{#nodejs}}.addFieldValidation(){{/nodejs}}{{#ruby}}.add_field_validation{{/ruby}}](../../../../agent-customization/fields/validation.md) instead          |
| Add validation to whole records         | Use [{{#nodejs}}.addHook(){{/nodejs}}{{#ruby}}.add_hook{{/ruby}}](../../../../agent-customization/hooks/README.md) and throw `validation errors`                |
| Run code on UI events                   | Use [{{#nodejs}}.addHook(){{/nodejs}}{{#ruby}}.add_hook{{/ruby}}](../../../../agent-customization/hooks/README.md) to perform custom logic                      |
| Change the search behavior              | Use [{{#nodejs}}.replaceSearch(){{/nodejs}}{{#ruby}}.replace_search{{/ruby}}](../../../../agent-customization/search.md) to implement your custom search logic  |
| Change the filtering behavior of fields | Use [{{#nodejs}}.replaceFieldOperator(){{/nodejs}}{{#ruby}}.replace_field_operator{{/ruby}}](../../../../agent-customization/fields/filter.md)                  |
| Change the sort behavior of fields      | Use [{{#nodejs}}.replaceFieldSorting(){{/nodejs}}{{#ruby}}.replace_field_sorting{{/ruby}}](../../../../agent-customization/fields/sort.md)                      |
| Other use case                          | If you are stuck or think that this guide can be improved, please [expose your use case in the community forums](https://community.forestadmin.com/) and we will be happy to help you |

# Examples

## Add custom permissions

{% hint style="warning" %}
Custom permissions would better be implemented by using the [Roles](https://docs.forestadmin.com/user-guide/project-settings/teams-and-users/manage-roles) feature from the UI.
{% endhint %}

{% tabs %} {% tab title="Before" %}

<details>
<summary><strong>router.delete(</strong></summary>

```javascript
  '/companies/:recordId',
  permissionMiddlewareCreator.delete(),
  (request, response, next) => {
    const { params, query, user } = request;

    if (user.email !== 'sandro.munda@forestadmin.com') {
      response
        .status(403)
        .send('This collection is protected, you cannot remove from it.');
      return;
    }

    next();
  },
);
```

</details>

<details>
<summary><strong>module ForestLiana</strong></summary>

```ruby
  class BooksController < ForestLiana::ApplicationController
    def destroy
      Book.find(params[:id]).destroy

      head :no_content
    end
  end
end
```

</details>

<details>
<summary><strong>agent.customizeCollection('customers', companies => {</strong></summary>

```javascript
  // Add a hook to the "customers" collection
  companies.addHook('Before', 'Delete', async context => {
    if (context.caller.email !== 'sandro.munda@forestadmin.com')
      context.throwForbiddenError(
        'This collection is protected, you cannot remove from it.',
      );
  });
});
```

</details>

<details>
<summary><strong>module ForestAdminRails</strong></summary>

```ruby
  class CreateAgent
    def self.customize
      @create_agent.customize_collection('Book') do |collection|
        collection.add_hook('Before', 'Delete') do |context|
          is_allowed = true # YOUR LOGIC HERE

          if !is_allowed
            context.raise_forbidden_error('This collection is protected, you cannot remove from it.')
          end
        end
      end
    end
  end
end
```

</details>

{% endtab %} {% endtabs %}

{{#nodejs,ruby}}

## Add validation to fields

{{/nodejs,ruby}}
{{#nodejs,ruby}}
{% tabs %} {% tab title="Before" %}
{{/nodejs,ruby}}
{{#nodejs}}

```javascript
function handler(request, response, next) {
  const patch = request.body.data.attributes;

  if (path.name && /^Forest/.test(path.name)) {
    return "All company names should begin with 'Forest'.";
  }

  if (error) {
    response.status(400).send(error);
  } else {
    next();
  }
}

router.post('/companies', permissionMiddlewareCreator.create(), handler);
router.put('/companies/:id', permissionMiddlewareCreator.update(), handler);
```

{{/nodejs}}

{{#ruby}}

- Define a new route into your `routes.rb` file.
- Add a new method to your controller.

<details>
<summary><strong>module ForestLiana</strong></summary>

```ruby
  class CompaniesController < ForestLiana::ApplicationController
    def create
      if params[:name] && params[:name].match?(/^Forest/)
        head :bad_request, content_type: 'application/json'
      else
        super
      end
    end

    def update
      if params[:name] && params[:name].match?(/^Forest/)
        head :bad_request, content_type: 'application/json'
      else
        super
      end
    end
  end
end
```

</details>

<details>
<summary><strong>agent.customizeCollection('companies', companies => {</strong></summary>

```javascript
  companies.addFieldValidation('name', 'Match', /^Forest/);
});
```

</details>

<details>
<summary><strong>module ForestAdminRails</strong></summary>

```ruby
  class CreateAgent
    include ForestAdminDatasourceToolkit::Components::Query::ConditionTree

    def self.customize
      @create_agent.customize_collection('Company') do |collection|
        collection.add_field_validation('name', Operators::MATCH, /^Forest/)
      end
    end
  end
end
```

</details>

{{#nodejs,ruby}}
{% endtab %} {% endtabs %}

## Run code on UI events

{% tabs %} {% tab title="Before" %}
{{/nodejs,ruby}}

<details>
<summary><strong>// Override the POST /customers route</strong></summary>

```javascript
router.post(
  '/customers',
  permissionMiddlewareCreator.create(),
  async (req, res, next) => {
    try {
      // Call an external API.
      await superagent
        .post('https://my-company/create-card')
        .set('X-API-Key', '**********')
        .end();

      // Calls next() to executes Forest Admin's default behavior
      next();
    } catch (err) {
      next(err);
    }
  },
);
```

</details>

<details>
<summary><strong>module ForestLiana</strong></summary>

```ruby
  class CustomersController < ForestLiana::ApplicationController
    def create
      begin
        superagent.post('https://my-company/create-card').set(
          'X-API-Key', '**********'
        ).end()
      ensure
        super
      end
    end
  end
end
```

</details>

<details>
<summary><strong>agent.customizeCollection('customers', companies => {</strong></summary>

```javascript
  // Add a hook to the "customers" collection
  companies.addHook('Before', 'Create', async context => {
    await superagent
      .post('https://my-company/create-card')
      .set('X-API-Key', '**********')
      .end();
  });
});
```

</details>

<details>
<summary><strong>module ForestAdminRails</strong></summary>

```ruby
  class CreateAgent
    def self.customize
      @create_agent.customize_collection('Customer') do |collection|
        collection.add_hook('Before', 'Create') do |context|
          superagent.post('https://my-company/create-card').set(
            'X-API-Key', '**********'
          )
        end
      end
    end
  end
end
```

</details>

{{#nodejs,ruby}}
{% endtab %} {% endtabs %}
{{/nodejs,ruby}}
