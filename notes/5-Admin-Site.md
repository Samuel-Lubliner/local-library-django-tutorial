# Django admin application

- Can use models to create, view, update, and delete records
- Makes it easy to test your models and data in development
- Also be useful for managing data in production
- Only recommended for internal data management

Dependencies needed:https://docs.djangoproject.com/en/5.0/ref/contrib/admin/

- Register models to add them to the admin application
- Configure the admin area for improved display
- Create a new "superuser"

## Registering models

Register models in `/django-locallibrary-tutorial/catalog/admin.py`

This is the simplest way of registering models with the site.

##  Creating a superuser

A superuser account has full access to the site and all needed permissions using `manage.py`.

- log into the admin site
  - user account with Staff status enabled.
- view and create records
  - permissions to manage all our objects

In the same directory as `manage.py`

run `python3 manage.py createsuperuser`

Restart the development server to test the login:

`python3 manage.py runserver`

## Logging in and using the site

Open the /admin URL and enter the superuser credentials. There is a GUI to view models and edit values for the fields.

## Advanced configuration

List views and detail views provide greater customization

Find a reference of the admin site in the docs: https://docs.djangoproject.com/en/5.0/ref/contrib/admin/

## Register a ModelAdmin class

Comment out the original registrations. Then Define a `ModelAdmin class` and register it with the model. See: https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#modeladmin-objects
