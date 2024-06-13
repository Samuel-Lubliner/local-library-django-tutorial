# Generic List and Detail Views

From learn.firstdraft and [MDN Django Generic Views](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Generic_views)

## Overview

- Use generic class-based list views for listing multiple records, and detail views for displaying individual records.
- Extract information from URL patterns and pass it to the view.
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
- Is implemented as a class, `BookListView`.
- Inherits from Django's generic class-based views.
- Is converted to a view function using the `as_view()` method.

## View (class-based)

Instead of writing the book list view as a regular function-based view (similar to a Rails controller action):
- Queries the database for all books.
- Calls `render()` to pass the list to a specified template.

Using a class-based generic list view (ListView) is preferable:
- Inherits from an existing view.
- The generic view already implements most of the functionality.
- Aligns with Django best practices for a more robust list view with less code, repetition, and maintenance.

In `catalog/views.py`, the generic view:
- Queries the database.
- Renders a template.

The template can access the list of books using a template variable. By default, generic views look for templates in the `/application_name/templates/application_name/the_model_name_list.html` path.

To modify the default behavior:
- Specify a different template file if you need to use multiple views for the same model.
- Use a different template variable name if needed.
- Customize the queryset to filter or change the subset of results returned.

## Overriding methods in class-based views

- It is possible to override some of the class methods, offering more flexibility than just setting the queryset attribute.
- For instance, override `get_context_data()` to pass additional context variables to the template.

Follow this pattern:
- Get the existing context from the superclass.
- Add new context information.
- Return the new context.

See [Built-in class-based generic views Django docs](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/).

## Creating the List View Template

The generic class-based list view for a Book model in a catalog application expects the template file to be located at `/django-locallibrary-tutorial/catalog/templates/catalog/book_list.html`. This template should extend the base template.

## Conditional execution

Template tags check whether `book_list` has been defined and is not empty.

For more information about conditional operators, see: [Django Docs](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/#if).

## For loops

The template uses template tags to loop and populate the book template variable.

## Accessing variables

- Access the fields using dot notation: `book.field_name`.

## Call functions in the model from within template

- `Book.get_absolute_url()` gets a URL to display the associated detail record.
- Note: There is no way to pass arguments.
- Warning: Be aware of side effects when calling functions in templates. Be sure not to accidentally do something destructive.

## Update the Base Template

To enable the link on all pages, update `/django-locallibrary-tutorial/catalog/templates/base_generic.html` to use `{% url 'books' %}`. The URL mapping for the book detail pages is needed to create hyperlinks to individual books.

## Book detail page

The book detail page displays information accessed using the URL `catalog/book/<id>`.

## URL Mapping

In `/catalog/urls.py`, the 'book-detail' path function defines a URL pattern, associates it with a generic class-based detail view, and assigns it a name. The `<int:pk>` part captures the book ID, where `pk` stands for primary key, uniquely identifying the book in the database.

## Passing additional options in your URL maps

You may pass a dictionary containing additional options to the view.

## View (class-based)

In `catalog/views.py`:

```python
class BookDetailView(generic.DetailView):
    model = Book
```

- Create the template at `/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html`
- The view will pass the Book record's information to the template.
- Access the book's details in the template using the variable named `object` or `book` (the model's name).

## If the record doesn't exist ...

- Generic class-based detail view raises an exception
- Http404
- Resource not found
- This behavior can be customized

## Creating the Detail View Template

For a Book model in an catalog application, the generic class-based detail view expects the template file to be located at `/django-locallibrary-tutorial/catalog/templates/catalog/book_detail.html`.

To create this template:
- The `url` template tag can reverse the 'author-detail' URL and pass it the author instance for the book. Using `get_absolute_url()` is preferred because any necessary changes need to be made only in the author model.

The template should:
- Extend the base template
- Override the content block.
- Use conditional processing and for loops to iterate through lists of objects.
- Access the context fields using dot notation.

Retrieve the set of `BookInstance` records associated with a particular Book, with the `book.bookinstance_set.all()` function.

Django constructs the function
- by lower-casing the ForeignKey model
- followed by `_set`
- Use `all()` to get all records

You cannot use the `filter()` method directly in templates because you cannot specify arguments to functions.

If you don't define an order (on your class-based view or model), there will be errors:

```bash
[date-time] "GET /catalog/books/?page=1 HTTP/1.1" 200 1637
/foo/local_library/venv/lib/python3.5/site-packages/django/views/generic/list.py:99: UnorderedObjectListWarning: Pagination may yield inconsistent results with an unordered object_list: <QuerySet [<Author: Lubiner, Samuel>, <Author: Van Rossum, Guido>, <Author: Torvalds, Linus>]>
  allow_empty_first_page=allow_empty_first_page, **kwargs)
```

The paginator object expects an `ORDER BY` clause executed on your underlying database.

## Pagination

- Django has built-in support for pagination
- Incorporated into the generic class-based list views

## Templates

With paginated data, add support to the template to scroll through the results set. To paginate all list views, add this to the base template. The `page_obj` gets all the information about the current page, previous pages, page numbers, etc.

`{{ request.path }}` gets the current page URL for creating the pagination links, independent of the object that we're paginating.

## Book List Page

Create the author detail and list views.

Author list: `catalog/authors/`
Detail view for the author with a primary key field: `catalog/author/<id>`

- After creating the URL mapper for the author list page, update the "All authors" link in the base template.
- After creating the URL mapper for the author detail page, update the book detail view template so the author link points to the new author detail page.
- Call `get_absolute_url()` on the author model.

## See also

- [Built-in class-based generic views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/generic-display/)
- [Generic display views](https://docs.djangoproject.com/en/5.0/ref/class-based-views/generic-display/)
- [Introduction to class-based views](https://docs.djangoproject.com/en/5.0/topics/class-based-views/intro/)
- [Built-in template tags and filters](https://docs.djangoproject.com/en/5.0/ref/templates/builtins/)
- [Pagination](https://docs.djangoproject.com/en/5.0/topics/pagination/)
- [Making queries > Related objects](https://docs.djangoproject.com/en/5.0/topics/db/queries/#related-objects)
