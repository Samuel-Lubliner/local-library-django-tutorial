# Generic List and Detail Views

From learn.firstdraft and [MDN Django Generic Views](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Generic_views)

## Overview

- Use generic class-based list views for listing multiple records, and detail views for displaying individual records.
- Extract information from URL patterns and pass it to the view using pattern matching with regular expressions.
- These views reduce the amount of view code needed, streamlining the development process.
- Implement pagination in list views to efficiently handle large datasets.
- Apply these concepts to create pages to view our books and authors, passing data from URLs to the views.

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

## View (class-based)

One option is to write the book list view as a regular function

- Function-based view are similar to a Rails controller action.
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

# Overriding methods in class-based views

- It is possible to override some of the class methods.
- More flexible than just setting the queryset attribute
- No real benefit in this case

- Possible to override `get_context_data()`
- Pass additional context variables to the template

Follow this pattern

- Get the existing context from the superclass.
- Add new context information.
- Return the new context.

See [Built-in class-based generic views Django docs](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/)

## Creating the List View template

``/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html`

- This is the default template file
- Expected by the generic class-based list view
- For a Book model in a catalog application
- Extend the base template

## Conditional execution

Template tags check whether the book_list has been defined and is not empty.

For more information about conditional operators see: [Django Docs](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if)

## For loops

The template uses template tags to loop and populate the book template variable.

## Accessing variables

- Access the fields using dot notation
- `book.field_name`

# Call functions in the model from within template

- `Book.get_absolute_url()` gets a URL to display the associated detail record.
- Note: There is no way to pass arguments
- Warning: Be aware of side effects when calling functions in templates. Be sure not to accidentally do something destructive.

## Update the base template

`/django-locallibrary-tutorial/catalog/templates/base_generic.html`

`{% url 'books' %}` enables the link in all pages.

The URL map for the book detail pages is needed to create hyperlinks to individual books.

## Book detail page

The book detail page displays information accessed using the URL `catalog/book/<id>`

## URL mapping

`/catalog/urls.py` 

The 'book-detail' path function defines a pattern, associated generic class-based detail view, and a name.

'<int:pk>' captures the book id

`pk` is short for primary key to store the book uniquely in the database.

## Passing additional options in your URL maps

You may pass a dictionary containing additional options to the view.

## View (class-based)

`catalog/views.py`

```PYTHON
Copy to Clipboard
class BookDetailView(generic.DetailView):
    model = Book
```

- Now create `/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html`
- The view will pass information for the Book record
- Access the book's details with
  - the template variable named object
  - OR book ("the_model_name")

## What happens if the record doesn't exist?

- The generic class-based detail view will raise an Http404 exception
- resource not found
- can be customized

## Creating the Detail View template

`/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html`

- Default template file name expected by the generic class-based detail view 
- Model named Book in an application named catalog

- The url template tag reverses the 'author-detail' URL (defined in the URL mapper)
- passing it the author instance for the book:

`get_absolute_url()` is preferred because  changes only need to be done in the author model.

The template

- extend our base template and override the "content" block.
- Conditional processing
- For loops to loop through lists of objects
- Access the context fields using the dot notation

The function `book.bookinstance_set.all()` is constructed by Django to return the set of `BookInstance` records associated with a particular Book.

The name of the function is constructed by

- lower-casing the model name where the ForeignKey was declared
- followed by _set

- Here we use all() to get all records
- Can't use the filter() method directly in templates because you can't specify arguments to functions.

- If you don't define an order (on your class-based view or model), there will be errors

```bash
[29/May/2017 18:37:53] "GET /catalog/books/?page=1 HTTP/1.1" 200 1637
/foo/local_library/venv/lib/python3.5/site-packages/django/views/generic/list.py:99: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <QuerySet [<Author: Ortiz, David>, <Author: H. McRaven, William>, <Author: Leigh, Melinda>]>
  allow_empty_first_page=allow_empty_first_page, **kwargs)
  ```

The paginator object expects an ORDER BY being executed on your underlying database.

## Pagination

- Django has excellent inbuilt support for pagination.
- Built into the generic class-based list views so you don't have to do very much to enable it!

## Templates

With the data is paginated, add support to the template to scroll through the results set. To paginate all list views, add this to the base template.

The page_obj gets all the information about the current page, previous pages, pages numbers, etc

`{{ request.path }}`
- Get the current page URL for creating the pagination links.
- Independent of the object that we're paginating.

## Book List Page

Create the author detail and list views.

catalog/authors/ — The list of all authors.
catalog/author/<id> — The detail view for the specific author with a primary key field named <id>

After creating the URL mapper for the author list page, update the All authors link in the base template.
After creating the URL mapper for the author detail page, update the book detail view template (/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html) so that the author link points to the new author detail page

Call `get_absolute_url()` on the author model as shown below.

## See also
Built-in class-based generic views (Django docs)
Generic display views (Django docs)
Introduction to class-based views (Django docs)
Built-in template tags and filters (Django docs)
Pagination (Django docs)
Making queries > Related objects (Django docs)