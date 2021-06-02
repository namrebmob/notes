# [Flask-Migrate](https://flask-migrate.readthedocs.io/en/latest/) is an extension that handles SQLAlchemy database migrations for Flask applications using [Alembic](https://alembic.sqlalchemy.org/en/latest/)

```py
if db.engine.url.drivername is "sqlite":
    migrate.init_app(app, db, render_as_batch=True)
else:
    migrate.init_app(app, db)
```

## Fix Alembic `tmp table already exists` error

```sh
flask db stamp head
flask db migrate
```
