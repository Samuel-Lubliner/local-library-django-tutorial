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


