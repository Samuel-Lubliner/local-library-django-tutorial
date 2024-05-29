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
