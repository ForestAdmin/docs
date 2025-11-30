[Route overrides](https://docs.forestadmin.com/documentation/reference-guide/routes/override-a-route) allowed customizing the behavior of the routes exposed by the agent.

This very low-level feature was used to implement many use cases:

- Attach handlers to events in the UI
- Customize filtering, search and sort behaviors
- Other advanced use cases.

Because our new agent API is higher-level, the protocol used to communicate between the agent and the application can no longer be manipulated.

# Code cheatsheet

| What was the route override used for?   | How to migrate it?                                                                                                                                                                    |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Add custom permissions                  | Use [{{#nodejs,php}}.addHook(){{/nodejs,php}}{{#python,ruby}}.add_hook{{/python,ruby}}](../../../../agent-customization/hooks/README.md) and throw `forbidden errors`                 |
| Add validation to fields                | Use [{{#nodejs,php}}.addFieldValidation(){{/nodejs,php}}{{#python,ruby}}.add_field_validation{{/python,ruby}}](../../../../agent-customization/fields/validation.md) instead          |
| Add validation to whole records         | Use [{{#nodejs,php}}.addHook(){{/nodejs,php}}{{#python,ruby}}.add_hook{{/python,ruby}}](../../../../agent-customization/hooks/README.md) and throw `validation errors`                |
| Run code on UI events                   | Use [{{#nodejs,php}}.addHook(){{/nodejs,php}}{{#python,ruby}}.add_hook{{/python,ruby}}](../../../../agent-customization/hooks/README.md) to perform custom logic                      |
| Change the search behavior              | Use [{{#nodejs,php}}.replaceSearch(){{/nodejs,php}}{{#python,ruby}}.replace_search{{/python,ruby}}](../../../../agent-customization/search.md) to implement your custom search logic  |
| Change the filtering behavior of fields | Use [{{#nodejs,php}}.replaceFieldOperator(){{/nodejs,php}}{{#python,ruby}}.replace_field_operator{{/python,ruby}}](../../../../agent-customization/fields/filter.md)                  |
| Change the sort behavior of fields      | Use [{{#nodejs,php}}.replaceFieldSorting(){{/nodejs,php}}{{#python,ruby}}.replace_field_sorting{{/python,ruby}}](../../../../agent-customization/fields/sort.md)                      |
| Other use case                          | If you are stuck or think that this guide can be improved, please [expose your use case in the community forums](https://community.forestadmin.com/) and we will be happy to help you |

# Examples

## Add custom permissions

{% hint style="warning" %}
Custom permissions would better be implemented by using the [Roles](https://docs.forestadmin.com/user-guide/project-settings/teams-and-users/manage-roles) feature from the UI.
{% endhint %}

{% tabs %} {% tab title="Before" %}

```javascript
router.delete(
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

{{#php}}

- Define a new route into your `web.php` file:
- Add a new method into your Controller.

```php
<?php

namespace App\Http\Controllers;

use App\Models\Book;
use Illuminate\Http\Response;

class BooksController extends Controller
{
    public function destroy(int $id): Response
    {
        Book::where('id', $id)->delete();

        return response()->noContent();
    }
}
```

{{/php}}
{{#python}}

- Define a new route into your `urls.py` file:
- Add a new method in your controller.

```python
from django.http import  HttpResponse
from django.views import generic

from app.models import Book

class BooksView(generic.ListView):
    def delete(self, request, pk, *args, **kwargs):
        Book.objects.filter(id=pk).delete()

        return HttpResponse(status=204)
```

{{/python}}
{{#ruby}}

- Define a new route into your `routes.rb` file:
- Add a new method in your controller.

```ruby
module ForestLiana
  class BooksController < ForestLiana::ApplicationController
    def destroy
      Book.find(params[:id]).destroy

      head :no_content
    end
  end
end
```

{{/ruby}}

{% endtab %} {% tab title="After" %}

```javascript
agent.customizeCollection('customers', companies => {
  // Add a hook to the "customers" collection
  companies.addHook('Before', 'Delete', async context => {
    if (context.caller.email !== 'sandro.munda@forestadmin.com')
      context.throwForbiddenError(
        'This collection is protected, you cannot remove from it.',
      );
  });
});
```

```php
$forestAgent->customizeCollection('Book', function (CollectionCustomizer $builder) {
    $builder->addHook('Before', 'Delete', function ($context) {
        $isAllowed = true; // YOUR LOGIC HERE

        if (!$isAllowed) {
            $context->throwForbiddenError('This collection is protected, you cannot remove from it.');
        }
    });
})
```

```python
from forestadmin.datasource_toolkit.decorators.hook.context.delete import (
    HookBeforeDeleteContext
)

def before_delete_hook(context: HookBeforeDeleteContext):
    is_allowed = True  # Your logic here

    if is_allowed is False:
        context.throw_forbidden_error(
            "This collection is protected, you cannot remove from it."
        )

agent.customize_collection('Book').add_hook('Before', 'Delete', before_delete_hook)
```

```ruby
module ForestAdminRails
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

{% endtab %} {% endtabs %}

{{#nodejs,python,ruby}}

## Add validation to fields

{{/nodejs,python,ruby}}
{{#nodejs,python,ruby}}
{% tabs %} {% tab title="Before" %}
{{/nodejs,python,ruby}}
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
{{#python}}

- Define a new route into your `urls.py` file.
- Add a new method to your controller.

```python
import re

from django.http import  HttpResponse
from django_forest.resources.views.detail import DetailView
from django_forest.resources.views.list import ListView

from app.models import Book

class CompanyView(ListView):
    def post(self, request, pk, *args, **kwargs):
        patch = self.get_body(request.body)["data"]["attributes"]

        if "name" in patch and re.search(r'^Forest', patch["name"]):
            return self.error_response(
                "All company names should begin with 'Forest'."
            )

        return super().post(request, pk, *args, **kwargs)

class CompanyDetailView(DetailView):
    def put(self, request, pk):
        patch = self.get_body(request.body)["data"]["attributes"]

        if "name" in patch and re.search(r'^Forest', patch["name"]):
            return self.error_response(
                "All company names should begin with 'Forest'."
            )

        return super().put(request, pk)
```

{{/python}}
{{#ruby}}

- Define a new route into your `routes.rb` file.
- Add a new method to your controller.

```ruby
module ForestLiana
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

{{/ruby}}
{{#nodejs,python,ruby}}
{% endtab %} {% tab title="After" %}
{{/nodejs,python,ruby}}

```javascript
agent.customizeCollection('companies', companies => {
  companies.addFieldValidation('name', 'Match', /^Forest/);
});
```

```python
agent.customize_collection('companies').add_field_validation(
    'name', 'match', r'^Forest'
)
```

```ruby
module ForestAdminRails
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

{{#nodejs,python,ruby}}
{% endtab %} {% endtabs %}

## Run code on UI events

{% tabs %} {% tab title="Before" %}
{{/nodejs,python,ruby}}

```javascript
// Override the POST /customers route
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

{{#python}}

- Define a new route into your `urls.py` file:
- Add a new method to your controller.

```python
from django_forest.resources.views.list import ListView

class CustomerView(ListView):
    def post(self, request, pk, *args, **kwargs):
        try:
            superagent.post('https://my-company/create-card').set(
                'X-API-Key', '**********'
            ).end()
        finally:
            return super().post(request, pk, *args, **kwargs)

```

{{/python}}
{{#ruby}}

```ruby
module ForestLiana
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

{{/ruby}}
{{#nodejs,python,ruby}}
{% endtab %} {% tab title="After" %}
{{/nodejs,python,ruby}}

```javascript
agent.customizeCollection('customers', companies => {
  // Add a hook to the "customers" collection
  companies.addHook('Before', 'Create', async context => {
    await superagent
      .post('https://my-company/create-card')
      .set('X-API-Key', '**********')
      .end();
  });
});
```

```python
from forestadmin.datasource_toolkit.decorators.hook.context.create import (
    HookBeforeCreateContext
)

def before_create_hook(context: HookBeforeCreateContext):
    superagent.post('https://my-company/create-card').set(
        'X-API-Key', '***********'
    ).end()

agent.customize_collection('customers').add_hook(
    'Before', 'Create', before_create_hook
)
```

```ruby
module ForestAdminRails
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

{{#nodejs,python,ruby}}
{% endtab %} {% endtabs %}
{{/nodejs,python,ruby}}
