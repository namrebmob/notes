# Using model callbacks in SQLAlchemy to generate slugs


```py
from sqlalchemy import event
from slugify import slugify

from models import db, BaseModel


class BlogPost(BaseModel):
    __tablename__ = 'blog_posts'
    post_id = db.Column(db.Integer, primary_key=True)
    post_title = db.Column(db.String(140))
    slug = db.Column(db.String(140))
    post_content = db.Column(db.Text)
    create_time = db.Column(db.DateTime, server_default=db.func.current_timestamp())

    @staticmethod
    def generate_slug(target, value, oldvalue, initiator):
        if value and (not target.slug or value != oldvalue):
            target.slug = slugify(value)

event.listen(BlogPost.post_title, 'set', BlogPost.generate_slug, retval=False)
```

This creates a listener so that everytime `post_title` is set, the `BlogPost.generate_slug` method will be called. The `target` in this method is the instance object, and the (new) `value` and `oldvalue` are also available.

I have set the `generate_slug` method to use python-slugify to generate the slugs.

Of course, if you have many models which need slugs you could also move this method into the BaseModel class which BlogPost is inheriting from instead.

