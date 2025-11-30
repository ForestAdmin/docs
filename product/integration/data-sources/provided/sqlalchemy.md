The SQLAlchemy data source allows importing collections from all models class that extends the class build with `declarative_base` or that inherit from `sqlalchemy.orm.DeclarativeBase`.

To make everything work as expected, you need to install the package `sqlalchemy`.

Note that:

- SQLAlchemy relationships will be respected

```python
from forestadmin.datasource_sqlalchemy.datasource import SqlAlchemyDatasource

from sqlalchemy.orm import DeclarativeBase
class Base(DeclarativeBase):
    pass
# or
from sqlalchemy.orm import declarative_base
Base = declarative_base()


agent.add_datasource(
    SqlAlchemyDatasource(
        Base,
        db_uri="postgres://user:pass@localhost:5432/myDatabase",
    )
)

```

the `db_uri` is mandatory only in the cases when the engine cannot be found in the base class.

```python
engine = create_engine("postgres://user:pass@localhost:5432/myDatabase", echo=False)
Base = declarative_base(engine)
agent.add_datasource(SqlAlchemyDatasource(Base))

```

When using the package Flask-SQLAlchemy, the setup is a bit different. In most of the case, the `db_uri` is not needed

```python
from forestadmin.datasource_sqlalchemy.datasource import SqlAlchemyDatasource
from flask_sqlalchemy import SQLAlchemy

db = SQLAlchemy()
class Address(db.Model):
    __tablename__ = "address"
    id = Column(Integer, primary_key=True)
    # ...

agent.add_datasource(SqlAlchemyDatasource(db))

```

## Enable support of live queries

By enabling this feature, users with the required permission level can create Live Query components ([charts](https://docs.forestadmin.com/user-guide/dashboards/charts/create-a-chart#creating-a-chart-with-sql), [analytics charts](https://docs.forestadmin.com/user-guide/dashboards/charts/analytics) and [segments](https://docs.forestadmin.com/user-guide/collections/segments#create-sql-query-segments)), allowing them to create more sophisticated requests to your database, by leveraging the underlying query language, SQL in this case.

You can enable this feature by setting a `connection name` (works as an identifier) when creating your datasource. This `connection name` will be reflected on the UI when configuring a LiveQuery component, it should have a clear meaning for your Forest users.

```python

agent.add_datasource(
    SqlAlchemyDatasource(
        Base, db_uri="postgres://user:pass@localhost:5432/mainDatabase", live_query_connection="main_database"
    ),
)
agent.add_datasource(
    SqlAlchemyDatasource(
        OtherBase, db_uri="postgres://user:pass@localhost:5432/secondaryDatabase", live_query_connection="secondary_database"
    ),
)

```

After this point you should see on ForestAdmin an input asking for the `connection` next to the `live query` text input.
