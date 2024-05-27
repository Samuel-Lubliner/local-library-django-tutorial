# Using Models

From: [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models)

## Models

- Python objects
- Define the data structure
- Independent from the database schema
- Facilitate communication between Django and the database via
Object-Relational Mapper

## Designing Models

Create separate models for every object. Models can also be used to represent selection-list options.

Consider the relationships between objects. Relationships include:

- one to one (OneToOneField)
- one to many (ForeignKey)
- many to many (ManyToManyField)

Multiplicities: the maximum and minimum number of each model that may be present in the relationship.

## Model definition

Models:

- Defined in `models.py`
- Implemented as subclasses of `django.db.models.Model`
- Can include fields, methods and metadata

## Fields

- A model has fields that represent columns in a database table.
- Each record (row) in the table contains values for these fields.
- Field types are assigned with classes, which determine the type of record that is used to store the data in the database.
- Field types can be used for HTML form validation.

## Field Arguments

- https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options
- `help_text`
- `verbose_name`
- `default`
- `null`
- `blank`
- `unique`
- `primary_key`

## COMMON FIELD TYPES

- https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-types
- `CharField`
- `TextField`
- `IntegerField`
- other fields for different types of numbers
- `DateField`
- `EmailField`
- `FileField`
- `ImageField`
- `AutoField`
- `ForeignKey` specifies one-to-many relationship to another database model
- `ManyToManyField`

## Metadata

- https://docs.djangoproject.com/en/5.0/ref/models/options/
- Declare model-level metadata with `class Meta`
- Useful for controlling the ordering of records returned by queries
- Other metadata options control the database used for the model and how the data is stored

## Methods

### `__str__()`
- Python class method
- Returns a human-readable string representation of the object
- The string represents individual records

### `get_absolute_url()`
- Returns a URL for displaying individual model records

## Model management

Use model classes to create, update, or delete records, and to run queries.

## Creating and modifying records

- Create a record by instantiating the model and using the model's constructor.
- Then call `save()` to save the object to the database.
- Access fields in the record using dot notation.
- Search for records using the model's objects attribute

## The full list of lookups: 
https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups

## Making queries: 
https://docs.djangoproject.com/en/5.0/topics/db/queries/

## Constraints 

https://docs.djangoproject.com/en/5.0/ref/models/constraints/

## Defining the LocalLibrary Models

At the top of `/django-locallibrary-tutorial/catalog/models.py`, the boilerplate imports the `models` module, which provides the `models.Model` base class that our models inherit from.

## Genre model

- Stores information about the book genre
- A single `CharField` field is used to describe the genre
- Limited to 200 characters 
- Has some `help_text`
- Set to`unique=True`
- One record for each genre
- `a __str__()` method returns the name of the genre
- `get_absolute_url()` method used to access a detail record 

## Book model

- Represents all the general information about an available book
- `CharField` to represent the book's title and isbn.
- unique as true ensures all books have a unique ISBN
- title is not set to be unique, because it is possible for different books to have the same name.
- `TextField` for longer summary
- `ManyToManyField` a book can have multiple genres and a genre can have many books

## In both field types:

- The related model class is declared as the first unnamed parameter
  - using either the model class
  - or a string containing the name of the related model
- Use the name of the model as a string if the associated class has not yet been defined in this file before it is referenced
- `null=True` allows the database to store a `Null` value if no author is selected
- `on_delete=models.RESTRICT` prevents the book's associated author being deleted if it is referenced by any book

## Warning:

- By default `on_delete=models.CASCADE`
- If the author was deleted, this book would be deleted too! - - Use `RESTRICT` or `PROTECT` to prevent the author being deleted while any book uses it 
- or `SET_NULL` to set the book's author to `Null` if the record is deleted
- `__str__()` uses the book's title field to represent a Book record
- `get_absolute_url()` returns a URL used to access a detail record

## BookInstance model

- Represents a specific copy of a book
- Includes information about whether the copy is available
- The date expected back
- version details
- unique id for the book in the library
- `ForeignKey` identifies the associated Book
  - each book can have many copies
  - a copy can only have one Book
- `on_delete=models.RESTRICT` ensures the Book cannot be deleted while referenced by a `BookInstance`
- `CharField` represents the specific release
- `UUIDField` for the id field to set it as the `primary_key` 
- This allocates a globally unique value for each instance 
- `DateField` for the `due_back` date
- status `CharField` defines a choice/selection list
- `__str__()` uses a combination of its unique id and title

## Re-run the database migrations

After models have now been created, re-run database migrations to add them to the database.

```BASH
python3 manage.py makemigrations
python3 manage.py migrate
```
