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
- Revives URLs starting with `catalog/`
- URLConf module `catalog.urls` processes the substring

The import function `django.urls.include()` splits the URL string and sends the substring to the URLconf module.

`/catalog/urls.py` is a placeholder for the URLConf module

`path()`

- Defines a URL pattern
- A view function
- The `name` parameter is a unique identifier for URL mapping
- Reverse the URL mapper to dynamically generate a URL that directs to a resource.
- Reversed URL mapping is more robust than hard coded links
- Use the name parameter to create links

## View

A view function
- processes an HTTP request
- fetches the required data from the database
- renders the data in an HTML page using a template
- returns the HTML in an HTTP response 

The first line in `catalog/views.py` imports `render()` to generate an HTML file.

The next line imports the model classes used to access data in views.

The view function 'index'

- Fetches the number of records using `objects.all()` on the model classes.
- Gets a list of `BookInstance` objects that have the status field value of 'a' for available
- Calls the `render()` function to create an HTML page a return the page as a response.

The `context` variable is a dictionary, containing the placeholder data

## Template

- A template defines the structure a file
- Such as an HTML page
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

`/django-locallibrary-tutorial/catalog/templates/index.html` extends the base template on the first line, and then replaces the default content block for the template.

Template variables

- Placeholders for the information from the view
- Variables are enclosed with double brace handlebars

Variables are named with the keys passed into the context dictionary in the `render()` function of the view. When the template is rendered, variables will be replaced with values.

## Referencing static files in templates

https://docs.djangoproject.com/en/5.0/howto/static-files/

Static resources include JavaScript, CSS, and images. You can specify the location in your templates relative to the `STATIC_URL` global setting. The default value of `STATIC_URL` is set to `'/static/'`. You can change it and host resources on a content delivery network.

In a template, first call the load template tag specifying "static" to add the template library. Then use the static template tag and specify the relative URL to the required file.

## Linking to URLs

The base template uses the url template tag.

This tag

- accepts the name of a `path()` function called in `urls.py`
- accepts the values for arguments the associated view will receive
- returns a URL used to link to the resource

## Configuring where to find the templates

https://docs.djangoproject.com/en/5.0/topics/templates/

Django searches for templates specified in the `TEMPLATES` object in the settings

In `settings.py` `'APP_DIRS': True` tells Django to search for templates in a subdirectory of each application in the project, named "templates".

Specify specific locations for Django to search for directories using `'DIRS': []`.

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
