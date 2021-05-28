# [SQLAlchemy](https://docs.sqlalchemy.org)

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

## modelAdapter class

```py
class MyAdapter(DBAdapter):
    def get_object(self, ObjectClass, id):
        """ Retrieve one object specified by the primary key 'pk' """
        pass

    def find_all_objects(self, ObjectClass, **kwargs):
         """ Retrieve all objects matching the case sensitive filters in 'kwargs'. """
        pass


    def find_first_object(self, ObjectClass, **kwargs):
        """ Retrieve the first object matching the case sensitive filters in 'kwargs'. """
        pass

    def ifind_first_object(self, ObjectClass, **kwargs):
        """ Retrieve the first object matching the case insensitive filters in 'kwargs'. """
        pass

    def add_object(self, ObjectClass, **kwargs):
        """ Add an object of class 'ObjectClass' with fields and values specified in '**kwargs'. """
        pass

    def update_object(self, object, **kwargs):
        """ Update object 'object' with the fields and values specified in '**kwargs'. """
        pass

    def delete_object(self, object):
        """ Delete object 'object'. """
        pass

    def commit(self):
        pass
        ```
