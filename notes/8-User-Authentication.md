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

The `django-extensions` package lets us run:

`python3 manage.py show_urls`

Displays the URL path, associated view logic, and name.

```bash
/       django.views.generic.base.RedirectView
/accounts/login/        django.contrib.auth.views.LoginView     login
/accounts/logout/       django.contrib.auth.views.LogoutView    logout
/accounts/password_change/      django.contrib.auth.views.PasswordChangeView    password_change
/accounts/password_change/done/ django.contrib.auth.views.PasswordChangeDoneView        password_change_done
/accounts/password_reset/       django.contrib.auth.views.PasswordResetView     password_reset
/accounts/password_reset/done/  django.contrib.auth.views.PasswordResetDoneView password_reset_done
/accounts/reset/<uidb64>/<token>/       django.contrib.auth.views.PasswordResetConfirmView     password_reset_confirm
/accounts/reset/done/   django.contrib.auth.views.PasswordResetCompleteView     password_reset_complete
/admin/ django.contrib.admin.sites.index        admin:index
/admin/<app_label>/     django.contrib.admin.sites.app_index    admin:app_list
/admin/<url>    django.contrib.admin.sites.catch_all_view
/admin/auth/group/      django.contrib.admin.options.changelist_view    admin:auth_group_changelist
/admin/auth/group/<path:object_id>/     django.views.generic.base.RedirectView
/admin/auth/group/<path:object_id>/change/      django.contrib.admin.options.change_view       admin:auth_group_change
/admin/auth/group/<path:object_id>/delete/      django.contrib.admin.options.delete_view       admin:auth_group_delete
/admin/auth/group/<path:object_id>/history/     django.contrib.admin.options.history_view      admin:auth_group_history
/admin/auth/group/add/  django.contrib.admin.options.add_view   admin:auth_group_add
/admin/auth/user/       django.contrib.admin.options.changelist_view    admin:auth_user_changelist
/admin/auth/user/<id>/password/ django.contrib.auth.admin.user_change_password  admin:auth_user_password_change
/admin/auth/user/<path:object_id>/      django.views.generic.base.RedirectView
/admin/auth/user/<path:object_id>/change/       django.contrib.admin.options.change_view       admin:auth_user_change
/admin/auth/user/<path:object_id>/delete/       django.contrib.admin.options.delete_view       admin:auth_user_delete
/admin/auth/user/<path:object_id>/history/      django.contrib.admin.options.history_view      admin:auth_user_history
/admin/auth/user/add/   django.contrib.auth.admin.add_view      admin:auth_user_add
/admin/autocomplete/    django.contrib.admin.sites.autocomplete_view    admin:autocomplete
/admin/catalog/author/  django.contrib.admin.options.changelist_view    admin:catalog_author_changelist
/admin/catalog/author/<path:object_id>/ django.views.generic.base.RedirectView
/admin/catalog/author/<path:object_id>/change/  django.contrib.admin.options.change_view       admin:catalog_author_change
/admin/catalog/author/<path:object_id>/delete/  django.contrib.admin.options.delete_view       admin:catalog_author_delete
/admin/catalog/author/<path:object_id>/history/ django.contrib.admin.options.history_view      admin:catalog_author_history
/admin/catalog/author/add/      django.contrib.admin.options.add_view   admin:catalog_author_add
/admin/catalog/book/    django.contrib.admin.options.changelist_view    admin:catalog_book_changelist
/admin/catalog/book/<path:object_id>/   django.views.generic.base.RedirectView
/admin/catalog/book/<path:object_id>/change/    django.contrib.admin.options.change_view       admin:catalog_book_change
/admin/catalog/book/<path:object_id>/delete/    django.contrib.admin.options.delete_view       admin:catalog_book_delete
/admin/catalog/book/<path:object_id>/history/   django.contrib.admin.options.history_view      admin:catalog_book_history
/admin/catalog/book/add/        django.contrib.admin.options.add_view   admin:catalog_book_add
/admin/catalog/bookinstance/    django.contrib.admin.options.changelist_view    admin:catalog_bookinstance_changelist
/admin/catalog/bookinstance/<path:object_id>/   django.views.generic.base.RedirectView
/admin/catalog/bookinstance/<path:object_id>/change/    django.contrib.admin.options.change_viewadmin:catalog_bookinstance_change
/admin/catalog/bookinstance/<path:object_id>/delete/    django.contrib.admin.options.delete_viewadmin:catalog_bookinstance_delete
/admin/catalog/bookinstance/<path:object_id>/history/   django.contrib.admin.options.history_view       admin:catalog_bookinstance_history
/admin/catalog/bookinstance/add/        django.contrib.admin.options.add_view   admin:catalog_bookinstance_add
/admin/catalog/genre/   django.contrib.admin.options.changelist_view    admin:catalog_genre_changelist
/admin/catalog/genre/<path:object_id>/  django.views.generic.base.RedirectView
/admin/catalog/genre/<path:object_id>/change/   django.contrib.admin.options.change_view       admin:catalog_genre_change
/admin/catalog/genre/<path:object_id>/delete/   django.contrib.admin.options.delete_view       admin:catalog_genre_delete
/admin/catalog/genre/<path:object_id>/history/  django.contrib.admin.options.history_view      admin:catalog_genre_history
/admin/catalog/genre/add/       django.contrib.admin.options.add_view   admin:catalog_genre_add
/admin/catalog/language/        django.contrib.admin.options.changelist_view    admin:catalog_language_changelist
/admin/catalog/language/<path:object_id>/       django.views.generic.base.RedirectView
/admin/catalog/language/<path:object_id>/change/        django.contrib.admin.options.change_viewadmin:catalog_language_change
/admin/catalog/language/<path:object_id>/delete/        django.contrib.admin.options.delete_viewadmin:catalog_language_delete
/admin/catalog/language/<path:object_id>/history/       django.contrib.admin.options.history_view       admin:catalog_language_history
/admin/catalog/language/add/    django.contrib.admin.options.add_view   admin:catalog_language_add
/admin/jsi18n/  django.contrib.admin.sites.i18n_javascript      admin:jsi18n
/admin/login/   django.contrib.admin.sites.login        admin:login
/admin/logout/  django.contrib.admin.sites.logout       admin:logout
/admin/password_change/ django.contrib.admin.sites.password_change      admin:password_change
/admin/password_change/done/    django.contrib.admin.sites.password_change_done admin:password_change_done
/admin/r/<int:content_type_id>/<path:object_id>/        django.contrib.contenttypes.views.shortcut      admin:view_on_site
/catalog/       catalog.views.index     index
/catalog/author/<int:pk>        catalog.views.AuthorDetailView  author-detail
/catalog/authors/       catalog.views.AuthorListView    authors
/catalog/book/<int:pk>  catalog.views.BookDetailView    book-detail
/catalog/books/ catalog.views.BookListView      books
/static/<path>  django.views.static.serve
vscode ➜ /workspaces/local-library-django-tutorial/locallibrary (auth) $ 
```

Next create a directory for the templates named "registration" and then add `login.html`.

## Template directory

The URLs expect to find associated templates in `/registration/` in the templates search path.

Put HTML pages in `templates/registration/` in the project root directory

Folder structure

```bash
django-locallibrary-tutorial/   # Django top level project folder
  catalog/
  locallibrary/
  templates/
    registration/
```

Make the `templates/` visible to the template loader, add it in the template search path

In `/django-locallibrary-tutorial/locallibrary/settings.py` import the os module

```PYTHON
import os
```

```PYTHON
    # …
    TEMPLATES = [
      {
       # …
       'DIRS': [os.path.join(BASE_DIR, 'templates')],
       'APP_DIRS': True,
       # …
```

## Login template

Create `/django-locallibrary-tutorial/templates/registration/login.html`

```DJANGO
{% extends "base_generic.html" %}

{% block content %}

  {% if form.errors %}
    <p>Your username and password didn't match. Please try again.</p>
  {% endif %}

  {% if next %}
    {% if user.is_authenticated %}
      <p>Your account doesn't have access to this page. To proceed,
      please login with an account that has access.</p>
    {% else %}
      <p>Please login to see this page.</p>
    {% endif %}
  {% endif %}

  <form method="post" action="{% url 'login' %}">
    {% csrf_token %}
    <table>
      <tr>
        <td>{{ form.username.label_tag }}</td>
        <td>{{ form.username }}</td>
      </tr>
      <tr>
        <td>{{ form.password.label_tag }}</td>
        <td>{{ form.password }}</td>
      </tr>
    </table>
    <input type="submit" value="login">
    <input type="hidden" name="next" value="{{ next }}">
  </form>

  {# Assumes you set up the password_reset view in your URLconf #}
  <p><a href="{% url 'password_reset' %}">Lost password?</a></p>

{% endblock %}
```

This template extends the base template and overrides the content block.


`{% csrf_token %}`  is a template helper.

A form object is available in the form and can point to the associated model (User model). Use `form.username.label_tag` and `form.username` to generate HTML label and input elements.

Navigate to `http://127.0.0.1:8000/accounts/login/`.

Log in and get redirected to `http://127.0.0.1:8000/accounts/profile/`

Django expects that the user want to be taken to a profile page. Right now this page is not defined yet so there is another error.

In `/django-locallibrary-tutorial/locallibrary/settings.py` redirect to the site homepage.

## Logout template

Note: Django 5 does not allow logout using GET, only POST.

Create `/django-locallibrary-tutorial/templates/registration/logged_out.html`.

## Testing in templates

- Get information about the logged in user in templates with the template variable `{{ user }}`
- Determine whether the user is eligible to see specific content with {{ user.is_authenticated }}

In `/django-locallibrary-tutorial/catalog/templates/base_generic.html`, update the sidebar to display a login or logout link depending on the users status.

Note: In Django 5 to logout you must POST to the `admin:logout` URL, using a form with a button.

## Testing in views

For function-based views, the `login_required` decorator is the easiest way to restrict.

For class-based views, derive from `LoginRequiredMixin` to restrict access. Declare this mixin first in the superclass list, before the main view class.

## Mixins and decorators

Mixin

- multiple inheritance for classes
- Functionality to be combined with other classes
- Extend the behavior of classes with reusable components
- DRY

Decorators

- Wrap a function with additional functionality
- Modify behavior without altering the code of the function being decorated
- Used for a variety of purposes including `@login_required`, `@permission_required`

## Listing the current user's books

Before creating the book list, first extend the `BookInstance` model to support borrowing and use Django Admin to loan a number of books to a user.

## Models

- Make it possible for users to have a `BookInstance` on loan.
- Already have a `status` and `due_back` date
- Create an association between the model and a user using a ForeignKey (one-to-many) field
- In `catalog/models.py` import the settings from `django.conf`.
- Add the borrower field to `BookInstance` model
- Add a property that to call from templates to tell if a particular book instance is overdue. Read more about [properties](https://docs.python.org/3/library/functions.html#property)
- Read more about [custom user models](https://docs.djangoproject.com/en/5.0/topics/auth/customizing/)
- Importing the model like this improves maintainability
- Be sure to migrate updated models

## Admin

- Make the `borrower` field visible in Admin section to assign a `User` to a `BookInstance`
- In `catalog/admin.py` add `borrower` to the `BookInstanceAdmin` class in the `list_display` and the `fieldsets`

## On loan view

- Add a view for getting the list of books loaned to the current user
- Use a generic class-based list view
- Import and derive from `LoginRequiredMixin`
- Declare a `template_name` instead of using the default

- In `catalog/views.py` restrict the query to just `BookInstance` objects for the current user
- Re-implement `get_queryset()`
- Order by `due_back` date to display oldest items first

## URL conf for on loan books

In `/catalog/urls.py` add a `path()` pointing to the view.

## Add template for on-loan books

Create `/catalog/templates/catalog/bookinstance_list_borrowed_user.html`. In this template file we use the method in the model to change the color of overdue items

# Add the list to the sidebar

- In the base template `/django-locallibrary-tutorial/catalog/templates/base_generic.html` add `My Borrowed` to the sidebar
- When a user is logged in, they see the `My Borrowed` link in the sidebar and the list of books

See the Django docs

[User authentication in Django](https://docs.djangoproject.com/en/5.0/topics/auth/)
[Using the (default) Django authentication system](https://docs.djangoproject.com/en/5.0/topics/auth/default/)
[Introduction to class-based views > Decorating class-based views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/#decorating-class-based-views)
