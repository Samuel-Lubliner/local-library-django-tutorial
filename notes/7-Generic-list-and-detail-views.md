# Generic List and Detail Views

From learn.firstdraft and [MDN Django Generic Views](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Generic_views)

## Overview

Generic class-based views reduce the amount of code needed for common view patterns.

For detail pages:
- Extract information from URL patterns and pass it to the view.
- Use detail views for displaying individual records.
- Use generic class-based list views for listing multiple records.
- They reduce the amount of view code needed.
- When using generic class-based list views, paginate data to handle large datasets efficiently.

## Book List Page

- Extend the base template `base_generic.html`.
- Display a list of all available book records.
- URL: `catalog/books/`.
- Each book's title is a hyperlink to the associated book detail page.
- Display the author for each book record.

## URL Mapping

Set the path for `books/` in `/catalog/urls.py`.

The `path()` function:
- Defines a pattern to match the `/catalog/books/` URL.
- Calls the view function returned by `views.BookListView.as_view()` if the URL matches.
- Provides a name for this particular URL mapping.

The view function:
- Is implemented in a different format.
- Is implemented as a class, `BookListView`.
- Inherits from Django's generic class-based views.
- Is converted to a view function using the `as_view()` method.

The tutorial implementation of `/workspaces/local-library-django-tutorial/locallibrary/catalog/urls.py` looks like this:

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
    path('books/', views.BookListView.as_view(), name='books'),
]
```

I am going to diverge from the tutorial for learning purposes and then return to the MDN implementation.

Right now my updated implementation of `/workspaces/local-library-django-tutorial/locallibrary/catalog/urls.py` looks like this:

```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.index, name="index"),
    path("books/", views.books_list, name="books"),
]
```

At this point, if I run `python3 manage.py runserver`, I get an error message in the server log:

`catalog.views has no attribute book_list`

## Function-based book list view and template

Coming from a Rails background I am going to first write a function-based view, similar to a Rails controller action. 

Later, I will bridge the gap from Rails function-based views to Django class-based views.

Implement the book_list function-based view in the `catalog/views.py` file.

Create a template to render the view
Visit the live app preview.

The “/catalog/books” page shows a list of available book titles and author.

## View (class-based)

Now I am returning to the MDN article

One option is to write the book list view as a regular function
- queries the database for all books
- then call render() to pass the list to a specified template

Instead I am using a class-based generic list view (ListView)

— Inherits from an existing view
- The generic view already implements most of the functionality
- Django best-practices
    - More robust list view
    - with less code
    - less repetition
    - less maintenance

In `catalog/views.py` the generic view 
- Queries the database 
- Renders a template 
 
The template can access the list of books with a template variable.

Generic views look for templates in `/application_name/the_model_name_list.html` inside the application's `/application_name/templates/` directory.

Add attributes to change the default behavior

- Specify another template file if you need to have multiple views that use this same model
- Specify a different template variable name 
- Change/filter the subset of results that are returned

```
class BookListView(generic.ListView):
    model = Book
    context_object_name = 'book_list'   # your own name for the list as a template variable
    queryset = Book.objects.filter(title__icontains='war')[:5] # Get 5 books containing the title war
    template_name = 'books/my_arbitrary_template_name_list.html'  # Specify your own template name/location
```