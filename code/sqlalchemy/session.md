# SQLAlchemy Session

```py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker


SQLALCHEMY_DATABASE_URI = "sqlite:///./db.sqlite3"

engine = create_engine(cnf.SQLALCHEMY_DATABASE_URI)
Session = sessionmaker(engine)

def get_sesion() -> Session:
    session = Session()
    try:
        yield session
    finally:
        session.close()
```
