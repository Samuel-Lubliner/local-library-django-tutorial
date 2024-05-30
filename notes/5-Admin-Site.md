From: https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Admin_site

# Django admin application

- Allows creating, viewing, updating, and deleting records using models.
- Simplifies testing models and data during development.
- Useful for managing data in production environments.
- Recommended only for internal data management.
- Dependencies needed: [Django Admin Documentation](https://docs.djangoproject.com/en/5.0/ref/contrib/admin/).
- Register models to add them to the admin interface.
- Configure the admin area for better display.
- Create a new "superuser."

## Registering models

Register models in `/django-locallibrary-tutorial/catalog/admin.py`. This is the simplest way of registering models with the site.

##  Creating a superuser

A superuser account has full access to the site and all needed permissions using `manage.py`.

- Log into the admin site with a user account that has Staff status enabled.
- View and create records with permissions to manage all objects.

In the same directory as `manage.py` run:

`python3 manage.py createsuperuser`

Restart the development server to test the login:

`python3 manage.py runserver`

## Logging in and using the site

Open the /admin URL and enter the superuser credentials. The GUI allows you to view models and edit field values.

## Advanced configuration

List views and detail views provide greater customization

Find a reference of the admin site in the docs: https://docs.djangoproject.com/en/5.0/ref/contrib/admin/

## Register a ModelAdmin class

Comment out the original registrations. Then Define a `ModelAdmin` class and register it with the model. See: https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#modeladmin-objects

## Configure list views

Use `list_display` to add additional fields to the view. See more at https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.list_display

## Add list filters

To filter displayed items, list fields in the `list_filter` attribute.

## Sectioning the detail view

To add sections, refer to: https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.fieldsets

## Inline editing of associated records

For inline editing of associated records, refer to: https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.ModelAdmin.inlines

Declaring inlines

- `TabularInline` (horizontal layout) https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.TabularInline

- `StackedInline` (vertical layout, default) https://docs.djangoproject.com/en/5.0/ref/contrib/admin/#django.contrib.admin.StackedInline
