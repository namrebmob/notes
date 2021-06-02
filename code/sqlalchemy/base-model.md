# [Simplifying SQLAlchemy models by creating a BaseModel](https://dev.to/chidioguejiofor/making-sqlalchemy-models-simpler-by-creating-a-basemodel-3m9c)

## BaseModel

### Attributes

```py
class BaseModel(db.Model):
    __abstract__ = True

    id = db.Column(db.Integer, primary_key=True, autoincrement=True)
    created_at = db.Column(
        db.DateTime(timezone=True), default=lambda: datetime.now(timezone.utc))
    updated_at = db.Column(db.DateTime(timezone=True), nullable=True)
```

### Instance Methods: Create, Update, Delete.

```py
class BaseModel(db.Model):

    def before_save(self, *args, **kwargs):
        pass

    def after_save(self, *args, **kwargs):
        pass

    def save(self, commit=True):
        self.before_save()
        db.session.add(self)
        if commit:
            try:
                db.session.commit()
            except Exception as e:
                db.session.rollback()
                raise e
        self.after_save()

    def before_update(self, *args, **kwargs):
        pass

    def after_update(self, *args, **kwargs):
        pass

    def update(self, *args, **kwargs):
        self.before_update(*args, **kwargs)
        db.session.commit()
        self.after_update(*args, **kwargs)

    def delete(self, commit=True):
        db.session.delete(self)
        if commit:
            db.session.commit()
```

### Class Methods: Eager Loading

```py
class BaseModel(db.Model):
    @classmethod
    def eager(cls, *args):
        cols = [orm.joinedload(arg) for arg in args]
        return cls.query.options(*cols)
```

`Eager loading` is a very important concept to be conscious of when querying your data. This method makes `eager loading` easier by allowing us to just specify only the fields we want to eager load. For example:

```py
Model.eager('rel1', 'rel2').all()
```

The above call would eager-load relationships `rel1` and `rel2`.

### Class Methods: Bulk Create

```py
class BaseModel(db.Model):

    @classmethod
    def before_bulk_create(cls, iterable, *args, **kwargs):
        pass

    @classmethod
    def after_bulk_create(cls, model_objs, *args, **kwargs):
        pass

    @classmethod
    def bulk_create(cls, iterable, *args, **kwargs):
        cls.before_bulk_create(iterable, *args, **kwargs)
        model_objs = []
        for data in iterable:
            if not isinstance(data, cls):
                data = cls(**data)
            model_objs.append(data)

        db.session.bulk_save_objects(model_objs)
        if kwargs.get('commit', True) is True:
            db.session.commit()
        cls.after_bulk_create(model_objs, *args, **kwargs)
        return model_objs


    @classmethod
    def bulk_create_or_none(cls, iterable, *args, **kwargs):
        try:
            return cls.bulk_create(iterable, *args, **kwargs)
        except exc.IntegrityError as e:
            db.session.rollback()
            return None
```

The `bulk_create` implementation above allows the user to **INSERT** multiple rows in the database either from an array of the `model instances` or array of `dictionaries`. I found the `dictionaries` part super helpful while writing automated tests.

Like the save method, the `bulk_create` method would persist to the **DB** when the commit `kwarg` is set to **True** and makes calls to `before_bulk_create` and `after_bulk_create`. It returns a list the created models on success but leaves error handling to the calling function

The `bulk_create_or_none` differs from the `bulk_create` by handling only `IntegrityError` and returning b when they occur.

## BaseModel in use

### Creating concrete Models

```py
class User(BaseModel):

    username = db.Column(db.String(20), nullable=False)
    password_hash = db.Column(db.VARCHAR(130), nullable=False)
    memberships = db.relationship('Membership',back_populates='member')

    def before_update(self, *args, **kwargs):
        print('Calling User models before_update')

    def after_update(self, *args, **kwargs):
        print('Calling User models after_update')


class Company(BaseModel):

    name = db.Column(db.String(120), nullable=False)
    website = db.Column(db.String(), nullable=False)
    address = db.Column(db.String(), nullable=False)
    memberships = db.relationship('Membership',back_populates='company')


class Membership(BaseModel):

    user_id = db.Column(db.Integer, db.ForeignKey('user.id'))
    company_id = db.Column(db.Integer,
                           db.ForeignKey('company.id'))
    role = db.Column(db.String, default='REGULAR_USER')
    member = db.relationship("User", back_populates="memberships")
    company = db.relationship('Company', back_populates="memberships")
```

### INSERTING

You can insert fields with something like:

```py
>>> user = User(username='some_username', password_hash='password')
>>> user.save()

>>> company = Company(name='Company 1', website='somewebsite.com', address='Atom 1 close, Ojodu Lagos')
>>> company.save()

>>> membership = Membership(user_id=user.id, company_id=company.id)
>>> membership.save()

>>> company = Company(name='SomeUnknownCompany', website='some.com',address='Aguda, Surulere Lagos')
>>> company.save(commit=False) # When commit is false

>>> Company.query.filter_by(name='SomeUnknownCompany').count()
1
>>> db.session.rollback()
>>> Company.query.filter_by(name='SomeUnknownCompany').count()
0
```

### Eager Loading

We can eager load fields:

```py
>>> Membership.eager('company', 'member').all()
[<Membership 1>, <Membership 2>, <Membership 3>, <Membership 4>, <Membership 5>, <Membership 6>, <Membership 7>, <Membership 8>, <Membership 9>, <Membership 10>]

# Generating the following SQL
INFO:sqlalchemy.engine.base.Engine:SELECT membership.id AS membership_id, membership.created_at AS membership_created_at, membership.updated_at AS membership_updated_at, membership.user_id AS membership_user_id, membership.company_id AS membership_company_id, membership.role AS membership_role, user_1.id AS user_1_id, user_1.created_at AS user_1_created_at, user_1.updated_at AS user_1_updated_at, user_1.username AS user_1_username, user_1.password_hash AS user_1_password_hash, company_1.id AS company_1_id, company_1.created_at AS company_1_created_at, company_1.updated_at AS company_1_updated_at, company_1.name AS company_1_name, company_1.website AS company_1_website, company_1.address AS company_1_address
FROM membership LEFT OUTER JOIN "user" AS user_1 ON user_1.id = membership.user_id LEFT OUTER JOIN company AS company_1 ON company_1.id = membership.company_id
INFO:sqlalchemy.engine.base.Engine:{}
```

### UPDATING

```py
>>> user = User.query.first()
>> user.username
'some_username'

>>> user.username = 'Adebayor1010'
>>> user.update()
Calling User models before_update
Calling User models after_update
>>> User.query.filter_by(username='Adebayor1010').count()
1

# Generating the following SQL query
INFO:sqlalchemy.engine.base.Engine:SELECT "user".id AS user_id, "user".created_at AS user_created_at, "user".updated_at AS user_updated_at, "user".username AS user_username, "user".password_hash AS user_password_hash
FROM "user"
WHERE "user".username = %(username_1)s
INFO:sqlalchemy.engine.base.Engine:{'username_1': 'user1010'}
```

### Bulk Creating

```py

>>> user_dicts = [
    {'username': 'user1010', 'password_hash': 'some_hash'},
    {'username': 'user1011', 'password_hash': 'some_hash'},
    {'username': 'user1012', 'password_hash': 'some_hash'},
    {'username': 'user1013', 'password_hash': 'some_hash'},
    ]
>>> users  = User.bulk_create(user_dicts)
