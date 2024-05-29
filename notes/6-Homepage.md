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

