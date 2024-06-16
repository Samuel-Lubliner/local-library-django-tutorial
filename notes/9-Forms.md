# Working with forms

## Book create

First add a URL pattern for book creation

## Form model

Django projects have a specific location forms to be used in View logic.

- Create `locallibrary/catalog/forms.py`
- Import the `forms` module
- Import the `Book` model from `models.py`

Define a new form class

- Inheriting from `forms.ModelForm`
- Override the `Meta` class to supply it with a model and fields for the form

## View

- Write a function-based view for the create view logic.
- The `form` object is an instance of the `BookForm`, which itself references the `Book` model.
- Pass the `form` object to the template using the context object

## Template

URL, Form, View, and then Template

- Created a `<form>`
- Set an action `{% url 'book-create %}` which evaluates to `/catalog/book/create`
- Give it the post method

- Add `{% csrf_token %}`
- Must be done manually for all forms in Django

- Passed in the form context object from view logic
- Iterate with `{% for field in form %}`

- The form object, and each field in it, has full access to the `Book` model
- Supplied the `BookForm` in `forms.py` was created

- For every field objects, call `field.label_tag`
- Then field to render the HTML label and input

- Access the model’s `help_text` with `field.help_text` in a conditional within the form

- Access `field.errors` and show them in a conditional within the form

- Validations are defined directly in the Book model

## Refactor form

- The form object
- was passed to the template in the context dictionary
- is an instance of the `BookForm` class, 
  - inherits from `models.ModelForm` and references the `Book` model

- Leave off the form action
- Can also just write `{{ form.as_p }}` to render all of our inputs, labels, help text, and validation messages, in paragraph tags

- Modify the `book_create` view logic so the book form redirects to the new book’s detail page after a successful book creation

## Book update

- Re-use the BookForm class new View and render the same template
- The BookForm can be passed an existing book with the argument `instance=book`
- Django is receiving an existing book object with `book = Book`objects.get(pk=pk) in the view logic
- The form will automatically pre-populate with the existing data for that book record

## Book delete

- Define a route (URL)
- Get the book from the dynamic path segment `<int:pk>` in a new function-based view
- Redirect to the list of books at the end of the logic
- Create a post form on the book detail template to give user the delete link.

## Refactor everything with class-based views

- In Django class-based views are more popular than function-based views
- Delete `forms.py`
- Create a confirmation template for book deletion
- Remove the form for book deletion
- Chang the views and URLs to use built in `generic.CreateView`, `generic.UpdateView`, and `generic.DeleteView` classes

Built in class-based views

- Know that you want a form for Book (`model=Book`)
- Expect that you have a template called `book_form.html`
- Provide fields to tell it how to build the form, and handle the processing and redirects after the actions are taken

Writing CRUD routes

- Django does not handle DELETE and PUT/PATCH HTTP requests in forms
- HTML forms only support GET and POST methods
- Django expects the POST method for form submissions
- Handle the intended action (update or delete) in view logic

## Permissions

- Define permissions on the model "class Meta" section
- Specify permissions in a tuple

