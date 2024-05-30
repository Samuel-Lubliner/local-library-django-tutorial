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
