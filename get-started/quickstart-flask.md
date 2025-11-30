## Add dependency forestadmin-agent-flask and forestadmin-datasource-sqlalchemy to your flask app

```bash
pip install forestadmin-agent-flask forestadmin-datasource-sqlalchemy
```

In your `app.py`, create a new agent with your settings, setup your SQLAlchemy data source, and register it to your Flask app:

```python
import os

from flask_cors import CORS
from forestadmin.datasource_sqlalchemy.datasource import SqlAlchemyDatasource
from forestadmin.flask_agent.agent import build_agent


# create your flask app
app = Flask(__name__)

# define settings for forest agent in flask config
app.config["FOREST_ENV_SECRET"] = os.environ.get("FOREST_ENV_SECRET")
app.config["FOREST_AUTH_SECRET"] = os.environ.get("FOREST_AUTH_SECRET")

# create the agent
agent = create_agent(app)

# setting up the CORS
CORS(
    app,
    resources={
        r"/forest/*": {"origins": r".*\.forestadmin\.com.*"},
        # if you are using the 'prefix' options don't forget to adapt the path
        # rf"{agent.options['prefix']}/forest/*": {
        #     "origins": r".*\.forestadmin\.com.*"
        # },
    },
    supports_credentials=True,
)

# register your data source to the agent
# if using basic sqlalchemy declarative_base
agent.add_datasource(SqlAlchemyDatasource(Base, db_uri='sqlite:///path/to/db.sql'))
# elif using flask_sqlalchemy package (https://pypi.org/project/Flask-SQLAlchemy/)
with app.app_context():
    agent.add_datasource(SqlAlchemyDatasource(db))
# endif

# finally start the agent
agent.start()
```

Running

```bash
FOREST_ENV_SECRET="<This is provided during the onboarding steps>" FOREST_AUTH_SECRET="<This is provided during the onboarding steps>" flask run
```

should be enough to be redirected to the "rate-install" page.
