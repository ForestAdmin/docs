The Django data source allows importing collections from all models of your Django project.

To make everything work as expected, you need to install the package `django`.

Note that:

- Django relationships will be respected
- The Django data source works with multiple databases
- The Django data source is added by default when using Django agent. You can disable this behavior by adding `FOREST_AUTO_ADD_DJANGO_DATASOURCE = False` in your settings.py

## Customization function

{% tabs %}
{% tab title="my_app.forest_admin.py" %}

```python
from forestadmin.datasource_django.datasource import DjangoDatasource
from forestadmin.django_agent.agent import DjangoAgent


def customize_forest(agent: DjangoAgent):
    #this is done automatically when FOREST_AUTO_ADD_DJANGO_DATASOURCE=True (default)
    agent.add_datasource(DjangoDatasource())

```

{% endtab %} {% tab title="my_project.settings.py" %}

```python
FOREST_CUSTOMIZE_FUNCTION = "my_app.forest_admin.customize_agent"
# or it can be a function directly
# from my_app.forest_admin import customize_agent
# FOREST_CUSTOMIZE_FUNCTION = my_app.forest_admin.customize_agent

# FOREST_AUTO_ADD_DJANGO_DATASOURCE = True

```

{% endtab %} {% endtabs %}

## Enable support of live queries

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a Live Query component, it should have a clear meaning for your Forest users.

```python
agent.add_datasource(
    DjangoDatasource(live_query_connection="main_database"),
)

```

At this stage, ForestAdmin should display a `connection` field next to the `live query` input box.

### Multi databases

If you are working with [multiple databases](https://docs.djangoproject.com/en/dev/topics/db/multi-db/#defining-your-databases) and using the previous example, the connection `main_database` will be bind to the `default` database django is using.

To support multiple databases, the `live_query_connection` can support a mapping of `{"connection_name": "database_name"}`.

{% tabs %}
{% tab title="my_app.forest_admin.py" %}

```python
agent.add_datasource(
    DjangoDatasource(live_query_connection={
        "main_database":"default",
        "users_database":"users"
        }
    ),
)

```

{% endtab %} {% tab title="my_project.settings.py" %}

```python
DATABASES = {
    "default": {
        "NAME": "app_data",
        "ENGINE": "django.db.backends.postgresql",
        "USER": "postgres_user",
        "PASSWORD": "s3krit",
    },
    "users": {
        "NAME": "user_data",
        "ENGINE": "django.db.backends.mysql",
        "USER": "mysql_user",
        "PASSWORD": "priv4te",
    },
}

```

{% endtab %} {% endtabs %}

## Django and Async

The Forest Admin agent uses async internally, while [Django ORM is part of Django async unsafe](https://docs.djangoproject.com/en/5.0/topics/async/#envvar-DJANGO_ALLOW_ASYNC_UNSAFE). Under the hood, the agent uses `sync_to_async` to query the ORM.

For custom functions such as `actions`, `computed fields`, `hooks`, etc..., if an asynchronous function is specified, it will be called in an asynchronous thread (also known as an event loop).
Whereas if it's a synchronous function, it will be executed directly within a synchronous thread. The method call will be wrapped with `sync_to_async`.

Because the agent core operates asynchronously, some APIs are asynchronous. This can result in a combination of asynchronous calls to the Forest Admin API and synchronous calls to Django ORM from custom functions.

### Asynchronous function

In an asynchronous function, you can query the Django ORM with the [async methods provided by Django](https://docs.djangoproject.com/en/5.0/topics/async/#queries-the-orm):

```python
async def refund_order_execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> ActionResult:
    id_ = await context.get_record_id()
    order = await Order.objects.aget(id=id_)
    # ...
    return result_builder.success(f"{order.amount}$ refunded.")
```

Or use `sync_to_async`:

```python
from asgiref.sync import sync_to_async

async def refund_order_execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> ActionResult:
    id_ = await context.get_record_id()
    order = await sync_to_async(Order.objects.get)(id=id_)
    # ...
    return result_builder.success(f"{order.amount}$ refunded.")
```

### Synchronous function

In a synchronous context, you can query Forest Admin asynchronous methods with `asyncio`:

```python
import asyncio

def refund_order_execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> ActionResult:
    id_ = asyncio.run(context.get_record_id())
    order = Order.objects.get(id=id_)
    # ...
    return result_builder.success(f"{order.amount}$ refunded.")
```

Or use `async_to_sync`:

```python
from asgiref.sync import async_to_sync

def refund_order_execute(
    context: ActionContextSingle, result_builder: ResultBuilder
) -> ActionResult:
    id_ = async_to_sync(context.get_record_id)()
    order = Order.objects.get(id=id_)
    # ...
    return result_builder.success(f"{order.amount}$ refunded.")
```
