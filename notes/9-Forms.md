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
