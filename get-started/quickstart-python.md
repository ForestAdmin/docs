## Add dependency forestadmin-agent-django to your Django app

```bash
pip install forestadmin-agent-django
```

You need to setup the forest Forest Admin agent, and CORS header settings.

{% tabs %} {% tab title="project/settings.py" %}

```python
# define the forest admin settings
FOREST_AUTH_SECRET = os.environ.get("FOREST_AUTH_SECRET")
FOREST_ENV_SECRET = os.environ.get("FOREST_ENV_SECRET")

# add it in the installed apps
INSTALLED_APPS = [
    # ...
    "forestadmin.django_agent",
    "corsheaders",
    # ...
]

# CORS settings
# see https://github.com/adamchainz/django-cors-headers for more details
MIDDLEWARE = [
    # ...
    # just before CommonMiddleware
    "corsheaders.middleware.CorsMiddleware",
    "django.middleware.common.CommonMiddleware",
    # ...
]
CORS_ALLOWED_ORIGIN_REGEXES = [
    r".*\.forestadmin\.com.*",
]
CORS_ALLOW_CREDENTIALS = True
```

{% endtab %} {% tab title="project/urls.py" %}

```python
from django.urls import include, path

urlpatterns = [
    # ...
    path("", include("forestadmin.django_agent.urls")),
    # ...
]

```

{% endtab %} {% endtabs %}
Running

```bash
FOREST_ENV_SECRET="<This is provided during the onboarding steps>" FOREST_AUTH_SECRET="<This is provided during the onboarding steps>" python manage.py runserver
```

should be enough to be redirected to the "rate-install" page.
