# User authentication and authorization

Django provides an authentication and authorization system, built on top of the session framework

For more information on sessions see the [docs](https://docs.djangoproject.com/en/5.0/topics/http/sessions/).

Sessions

- verify user credentials 
- built-in models
- permissions/flags
- forms and views for logging in

Django authentication

- Generic
- Missing some authentication features
- Third-party packages are available for solutions to throttling of login attempts and authentication against third parties (OAuth)

## Enabling authentication

- The `django-admin startproject` command automatically enabled authentication. 
- `python manage.py migrate` created the database tables for users and model permissions.

In `django-locallibrary-tutorial/locallibrary/settings.py`, `INSTALLED_APPS` and `MIDDLEWARE` handle configuration. 

## Creating users and groups

- Most users should not access to the admin site.
- Easier to add permissions to Groups vs individually
- The admin site is one of the quickest ways to create groups and website logins.
- Another option is programmatically creating users.

```PYTHON
from django.contrib.auth.models import User

user = User.objects.create_user('myusername', 'myemail@mail.com', 'mypassword')

user.first_name = 'firstname'
user.last_name = 'lastname'
user.save()
```

Alteratively, set up a custom user model for a more robust approach:

```PYTHON
from django.contrib.auth import get_user_model
User = get_user_model()

user = User.objects.create_user('myusername', 'myemail@mail.com', 'mypassword')

user.first_name = 'firstname'
user.last_name = 'lastname'
user.save()
```

Reference using a custom user model when starting a project at the (Django docs)[https://docs.djangoproject.com/en/5.0/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project]

## Setting up your authentication views

A URL mapper, views and forms for Authentication pages come out of the box. Templates must be created manually.

Integrate the default system into the LocalLibrary website and create the templates in the main project URLs.

## Project URLs

`django-locallibrary-tutorial/locallibrary/urls.py`

```PYTHON

urlpatterns += [
    path('accounts/', include('django.contrib.auth.urls')),
]
```

Navigate to `http://127.0.0.1:8000/accounts/` and note the trailing `/`.

Adding the `accounts/` path adds URLs and names to reverse the URL mappings.

The URL mapping automatically maps these URLs.

```PYTHON
accounts/ login/ [name='login']
accounts/ logout/ [name='logout']
accounts/ password_change/ [name='password_change']
accounts/ password_change/done/ [name='password_change_done']
accounts/ password_reset/ [name='password_reset']
accounts/ password_reset/done/ [name='password_reset_done']
accounts/ reset/<uidb64>/<token>/ [name='password_reset_confirm']
accounts/ reset/done/ [name='password_reset_complete']
```

Navigate to `http://127.0.0.1:8000/accounts/login/` to find an error about missing the required template on the search path.

```PYTHON
Exception Type:    TemplateDoesNotExist
Exception Value:    registration/login.html
```

Next create a directory for the templates named "registration" and then add `login.html`.
