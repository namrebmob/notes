# Useful Flask extensions

## [Flask-Blogging](https://flask-blogging.readthedocs.io/en/latest/): adding Markdown based blog support to your site

- <https://github.com/gouthambs/Flask-Blogging>
- <https://github.com/gouthambs/blogging_plugins>

## [Flask-APScheduler](https://github.com/viniciuschiele/flask-apscheduler): adds [APScheduler](https://apscheduler.readthedocs.io/en/stable/) support to Flask

## [Flask-Talisman](https://github.com/GoogleCloudPlatform/flask-talisman): HTTP security headers for Flask

## [Flask-Assets](https://flask-assets.readthedocs.io/en/latest/): helps you to integrate webassets into your Flask application

## [Flask-Security](https://pythonhosted.org/Flask-Security/): allows you to quickly add common security mechanisms to your Flask application

## [Flask-Login](https://flask-login.readthedocs.io/en/latest/): provides user session management for Flask. It handles the common tasks of logging in, logging out, and remembering your users’ sessions over extended periods of time

Login/Logout example

```py
# forms.py

from flask_wtf import FlaskForm
from wtforms import BooleanField, PasswordField, StringField, SubmitField
from wtforms.validators import DataRequired


class LoginForm(FlaskForm):
    email = StringField("Email", [DataRequired()])
    password = PasswordField("Password", [DataRequired()])
    remember_me = BooleanField("Remember Me")
    submit = SubmitField("Log In")
```

```py
# routes.py

from flask import flash, redirect, render_template, request, url_for
from flask_login import current_user, login_user, logout_user
from werkzeug.urls import url_parse

from .forms import LoginForm
from .models import User


@app.route("/login", methods=["GET", "POST"])
def login():
    if current_user.is_authenticated:
        return redirect(url_for("home"))
    form = LoginForm()
    if form.validate_on_submit():
        user = User.query.filter_by(email=form.email.data).first()
        if user is None or not user.check_password(form.password.data):
            flash("Invalid username or password", "error")
            return redirect(url_for("login"))
        login_user(user, remember=form.remember_me.data)
        next_page = request.args.get("next")
        if not next_page or url_parse(next_page).netloc != "":
            next_page = url_for("home")
        flash(f"Log in as {user}", "success")
        return redirect(next_page)
    return render_template("login.html", form=form, page_title="Login")


@auth.route("/logout")
def logout():
    logout_user()
    flash("You are logged out", "success")
    return redirect(url_for("home"))
```

## [Flask-Mail](https://pythonhosted.org/Flask-Mail/): provides a simple interface to set up SMTP with your Flask application and to send messages from your views and scripts

## [Flask-WTF](https://flask-wtf.readthedocs.io/en/latest/): Simple integration of Flask and [WTForms](https://wtforms.readthedocs.io/en/master/), including CSRF, file upload, and reCAPTCHA