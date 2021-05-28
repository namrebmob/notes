# SQLAlchemy Session

```py
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

from config import cnf

engine = create_engine(cnf.SQLALCHEMY_DATABASE_URI)

Session = sessionmaker(engine, autocommit=cnf.SQLALCHEMY_AUTOCOMMIT, autoflush=cnf.SQLALCHEMY_AUTOFLUSH)

def get_sesion() -> Session:
    session = Session()
    try:
        yield session
    finally:
        session.close()
