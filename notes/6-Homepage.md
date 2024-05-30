# Creating a homepage

from: https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Home_page

## Data flow and handling HTTP requests and responses

- URL mappers direct URLs and information to view functions
- View functions retrieve data from models and render HTML
- Templates: Define HTML for displaying data.

## Creating the index page

`catalog/`

- index page
- includes counts of records in the database

## URL mapping

`locallibrary/urls.py`
- Processes URLs starting with `catalog/`
- `catalog.urls` processes the substring

The import function `django.urls.include()` splits the URL string and sends the substring to the URLconf module.

`/catalog/urls.py` is a placeholder for the URLConf module

`path()`

- Defines a URL pattern and associates it with a view function.
- The `name` parameter is a unique identifier for URL mapping.
- Use the `name` parameter to reverse the URL mapper and dynamically generate a URL that directs to a resource.
- Reversed URL mapping is more robust than hard-coded links.
- Use the name parameter to create links

## View

A view function

- Handles an HTTP request
- Retrieves necessary data from the database
- Generates an HTML page using a template to display the data

The first line in `catalog/views.py` imports `render()` to generate an HTML file.

The next line imports the model classes used to access data in views.

The view function 'index'

- Fetches the number of records using `objects.all()` on the model classes.
- Retrieves a collection of `BookInstance` objects where the status field is set to 'a' for available
- Calls the `render()` function to create an HTML page a return the page as a response.

The `context` variable is a dictionary, containing the placeholder data.

## Template

- A template defines the structure a file, such as an HTML page
- Use placeholders for content

In the index view from 'locallibrary/catalog/views.py', the `render()` function will expect to find `/django-locallibrary-tutorial/catalog/templates/index.html` or return an error.

## Extending templates

Declare a base template and extend it to replace different parts for each specific page.

Template tags
- loop through lists
- conditional operations

Template syntax
- Reference variables from the view
- Use filters to format variables

When defining a template specify the base template using the extends template tag. Then declare sections from the template to be replaced.

Our base template is `/django-locallibrary-tutorial/catalog/templates/base_generic.html`

The base template references `/django-locallibrary-tutorial/catalog/static/css/styles.css` that provides additional styling.

## The index template

`/django-locallibrary-tutorial/catalog/templates/index.html` starts by extending the base template and then customizes the default content block specific to the template's needs.

Template variables

- Placeholders for the information from the view
- Variables are enclosed with double brace handlebars

The names of the variables correspond to the keys provided in the context dictionary within the view's `render()` function.

## Referencing static files in templates

https://docs.djangoproject.com/en/5.0/howto/static-files/

Static resources include JavaScript, CSS, and images. You can specify the location in your templates relative to the `STATIC_URL` global setting. The default value of `STATIC_URL` is set to `'/static/'`. You can change it and host resources on a content delivery network.

To reference static files in a template, initially load the static template tag to include the template library. After that, use the static tag to provide the relative path to the desired file.

## Linking to URLs

The base template uses the url template tag.

This tag

- Takes the name of a `path()` function called in `urls.py`
- Accepts the values for arguments the associated view will receive
- Returns a URL used to link to the resource

## Configuring where to find the templates

https://docs.djangoproject.com/en/5.0/topics/templates/

Django searches for templates specified in the `TEMPLATES` object in the settings

In `settings.py` `'APP_DIRS': True` instructs Django to look for templates within a "templates" subdirectory inside each app of the project. You can also specify particular directories for Django to search by setting `'DIRS': []`.

## Challenge

1. The LocalLibrary base template includes a title block. Override this block in the index template and create a new title for the page.

2. Modify the view to generate counts for genres and books that contain a particular word (case insensitive), and pass the results to the context. You accomplish this in a similar way to creating and using num_books and num_instances_available. Then update the index template to include these variables.

See:

[Writing your first Django app, part 3: Views and Templates](https://docs.djangoproject.com/en/5.0/intro/tutorial03/)
[URL dispatcher](https://docs.djangoproject.com/en/5.0/topics/http/urls/)
[View functions](https://docs.djangoproject.com/en/5.0/topics/http/views/)
[Templates](https://docs.djangoproject.com/en/5.0/topics/templates/)
[Managing static files](https://docs.djangoproject.com/en/5.0/howto/static-files/)
[Django shortcut functions](https://docs.djangoproject.com/en/5.0/topics/http/shortcuts/#django.shortcuts.render)
