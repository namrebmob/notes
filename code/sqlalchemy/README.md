# [SQLAlchemy](https://docs.sqlalchemy.org) is the Python SQL toolkit and Object Relational Mapper that gives application developers the full power and flexibility of SQL

- [Common filters](https://docs.sqlalchemy.org/en/14/orm/tutorial.html#common-filter-operators)
- [Add columns to a query](https://www.javaer101.com/en/article/15975233.html)
- [Using Python enums in SQLAlchemy models](https://www.michaelcho.me/article/using-python-enums-in-sqlalchemy-models)
- [Using model callbacks in SQLAlchemy to generate slugs](https://www.michaelcho.me/article/using-model-callbacks-in-sqlalchemy-to-generate-slugs)
- [Simplifying SQLAlchemy models by creating a BaseModel](https://dev.to/chidioguejiofor/making-sqlalchemy-models-simpler-by-creating-a-basemodel-3m9c)
- [Eager Loading VS Lazy Loading in SQLAlchemy](https://dev.to/chidioguejiofor/eager-loading-vs-lazy-loading-in-sqlalchemy-5209)

## Slugify

```py
from slugify import slugify
return Person.query.get(person_id).name.filter(slugify(Person.name))
```

```py
@app.route('/posts/<slug>')
@login_required
def show(slug):
    post = db.session.query(Post).filter_by(slug = slug).first()
    if post:
        return render_template("post.html", post=post)

    abort(404)
```
