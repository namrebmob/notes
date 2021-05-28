# [Flask Admin](flask-admin.readthedocs.io)

- [Flask-Admin Hacks for Many-to-Many Relationships](https://wordpressify.ru/2018/09/flask-admin-hacks-for-many-to-many-relationships/)
- [SQLA examples](https://github.com/flask-admin/flask-admin/blob/master/examples/sqla/admin/main.py)
- [Example application for flask-admin with image upload](https://github.com/greyli/flask-ckeditor/blob/master/examples/flask-admin-upload/app.py)
- [Flask Admin Ckeditor Загрузка Изображений](https://coderoad.ru/50555668/Flask-Admin-Ckeditor-Загрузка-Изображений)
- <https://github.com/flask-admin/flask-admin/blob/master/flask_admin/form/fields.py>
- <https://blog.sneawo.com/blog/2017/02/10/flask-admin-formatters-examples/>
- <https://www.javaer101.com/en/article/33089718.html>
- <http://kalacode11.blogspot.com/2015/09/custom-flask-admin-form-with-some.html>
- <https://coderoad.ru/58164713/Как-динамически-загружать-варианты-выбора-для-одного-из-полей-form_extra_fields>
- <https://www.rupython.com/flask-admin-custom-select2-ajax-field-87151.html>
- [Ajax form field on Flask Admin](https://www.javaer101.com/en/article/42104882.html)
- [add a summary row for Flask Admin](https://www.javaer101.com/en/article/18111320.html)
- [add the database view to a different site](https://www.javaer101.com/en/article/29969122.html)
- [Adding Contact Page to Flask Admin](https://www.javaer101.com/en/article/119843192.html)

```py
from flask.ext.admin.form import Select2Widget

class MatrilineView(sqla.ModelView):
    column_hide_backrefs = False
    form_extra_fields = {
        'clan': sqla.fields.QuerySelectField(
            label='Clan',
            query_factory=lambda: Clan.query.all,
            widget=Select2Widget()
        )
    }
    column_list = ('name', 'pod', 'clan')
```

```py
from functools import partial
from flask.ext.admin import Admin as BaseAdmin, AdminIndexView
from flask.ext.principal import Permission, identity_loaded, Need
from flask.ext.security import current_user

PartnerAccessNeed = partial(Need, 'access')


class PartnerAccessPermission(Permission):
    def __init__(self, partner_id):
        need = PartnerAccessNeed(partner_id)
        super(PartnerAccessPermission, self).__init__(need)


@identity_loaded.connect
def on_post_identity_loaded(sender, identity):
    if hasattr(current_user, 'partner'):
        identity.provides.add(PartnerAccessNeed(current_user.partner.id))


class PartnerAdminIndexView(AdminIndexView):
    def __init__(self, partner_id, *args, **kwargs):
        self.partner_id = partner_id
        super(PartnerAdminIndexView, self).__init__(*args, **kwargs)


def is_accessible(self):
    if current_user.is_anonymous():
        return redirect(url_for_security('login'))
    if not current_user.is_partner():
        return False


permission = PartnerAccessPermission(self.partner_id)
if permission.can():
    return True
return False


class PartnerAdmin(BaseAdmin):
    def __init__(self, partner_id, endpoint, name, subdomain, *args, **kwargs):
        index = PartnerAdminIndexView(
            name=name, endpoint=endpoint, url='/dashboard', partner_id=partner_id)
        super(PartnerAdmin, self).__init__(
            base_template='mcnm/master.html', index_view=index, subdomain=subdomain)


class MyView(ModelView):
    @expose('/')
    @login_required
    def index(self):
        return self.render('admin/index.html')
```

```py
from flask_login.utils import current_user

class MyView(ModelView):
    @expose('/')
    def index(self):
        return self.render('admin/index.html')

    def is_accessible(self):
        return current_user.is_authenticated()
```

```py
from flask.ext.admin.model.template import macro

class mymodelview (basemodelview):
    column_formatters=dict(price=macro("render_price"))

#in template
{ % macro render_price (model, column)%}
{{model.price * 2}}{ % endmacro%}


#Create custom models view
class MyModelView(sqla.ModelView):
    @admin.expose('/login/')
    def index(self):
        return self.render('login.html')

    def on_model_change(self, form, User, is_created=False):
        User.password=form.password_hash.data

# Это предотвратит двойное хеширование

def on_model_change(self, form, User, is_created):
if form.password_hash.data:
    User.set_password(form.password_hash.data)
else:
    del form.password_hash

def on_form_prefill(self, form, id):
form.password_hash.data=''

#Что я сделал, так это добавил поле формы set_password, которое при заполнении создает пароль hash и обновляет password в модели. Один и готово!

class UserView(AdminModel):
can_create=True
column_list=('name', 'email',)
column_searchable_list=('name', 'email',)
form_excluded_columns=('password',)
form_extra_fields={
    'set_password': PasswordField('Set New Password')
}

def on_model_change(self, form, model, is_created):
    if is_created:
        model.active=True
        model.pending=False
    if form.email.data:
        # Strip spaces from the email
        form.email=form.email.data.strip()
    if form.set_password.data:
        model.password=bcrypt.generate_password_hash(
            form.set_password.data.strip())

def __init__(self, session, **kwargs):
    # You can pass name and other parameters if you want to
    super(UserView, self).__init__(User, session, **kwargs)
```
