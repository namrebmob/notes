# Flask SQLAlchemy

## Custom Base Query

```py
from flask_sqlalchemy import BaseQuery

class FooQuery(BaseQuery):
    def all_latest_paginated(self):
        pagination = (
            self.get_updated()
            .filter(text("status == True"))
            .paginate(per_page=current_app.config.get("POSTS_PER_PAGE"))
        )
        return pagination, pagination.items

class Foo(db.Model):
    query_class = FooQuery

```
