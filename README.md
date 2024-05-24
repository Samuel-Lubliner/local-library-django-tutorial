# local-library-tutorial

This is a companion repository to [Mozilla's Django Tutorial](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django).

## Introduction to Django

From: https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Introduction

## About Django

- Python
- High-level web framework
- Rapid development
- Secure
- Maintainable
- Scalable
- Free and open source
- Batteries included
- Somewhat opinionated

## Model View Template

Django uses the Model View Template (MVT) architecture, similar to the Model View Controller architecture.

## URL mapper

- User requests go to `urls.py`, which maps the request to a view.

## View

- The request handler function in `views.py` processes the request and may interact with models.
- Django views are similar to controllers and actions in Rails.

## Models

- Python objects that model data
- Provides an API to interact with the database

## Templates

- Define the structure of any type of file
- Placeholders populated with data from a model
- Renders HTML and other file types
- The rendered template is sent back as the response to the user.

From: https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/skeleton_website

## Creating the library project
- Open the terminal in a virtual or development environment
- Navigate to the project folder
- Run:

```bash
django-admin startproject locallibrary

cd locallibrary`
```

`django-admin` generates:

- a project folder
- the file templates
- `manage.py`
- a `locallibrary` sub-folder with the same name as the project folder. This is the entry point for the website.

The sub-folder entry point contains:

`__init__.py`: Indicates the directory is a Python package

`settings.py`: Website settings

`urls.py`: URL-to-view mappings

`wsgi.py`: Web Server Gateway Interface for synchronous apps

`asgi.py`: Asynchronous Server Gateway Interface

`manage.py`: Project management script that creates one or more applications.

Applications:

- Separate, reusable components of a website
- Register the new applications
- url/path mapper for each application

## Creating the catalog application

Run this command from the same folder as `manage.py`:

`python3 manage.py startapp catalog`

This creates the following folder structure:

```bash
catalog/
        admin.py
        apps.py
        models.py
        tests.py
        views.py
        __init__.py
        migrations/
```

The `migrations` folder updates the database when models are modified.

## Note for codespace development:

Add `*` wildcard to `ALLOWED_HOSTS` in `settings.py`. This allows the host to run the live preview.

## Register the catalog application

Add applications to `INSTALLED_APPS` in the project settings.

## Specifying the database

SQLite is the default database. It is suitable if you don't a lot of concurrent access. Postgres requires additional setup and is more suitable for larger sites.

## Other settings

`TIME_ZONE` can be configured.

`SECRET_KEY` can be changed to read from an environment variable in production.

`DEBUG` logs errors in a file instead of HTTP status code responses. Set to `False` in production.

## Hook up the URL mapper

While `urls.py` can handle all the URL mappings, it's more common to route these mappings to the corresponding application.

The URL mappings are managed through the `urlpatterns` variable, which is a list of `path()` functions.

`path()` forwards requests to the module with the relative URL.

`RedirectView` redirects the root URL of the site.

Django does not serve static files by default. Serving CSS, JavaScript, and images can be useful for the development web server. Do not do this in production.

In the catalog folder, create `urls.py` and define the imported `urlpatterns`. Add patterns here as the application grows.

Before doing this, first run a database migration to update the database.

## Running database migrations

Django uses an Object-Relational-Mapper. Django tracks the changes to model definitions and can create database migration scripts.

In the directory that contains `manage.py`, run

`python3 manage.py makemigrations`
`python3 manage.py migrate`

`makemigrations` creates the migrations.

`migrate` applies the migrations to the database.

## Running the website

`python3 manage.py runserver`

Runs the development web server.

An error page is expected if there are no pages/urls defined in the the root of the site.