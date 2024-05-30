# Using Models

From: [MDN Web Docs](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models)

## Models

- Python objects
- Define the data structure
- Independent from the database schema
- Facilitate communication between Django and the database via Object-Relational Mapper

## Designing Models

Create separate models for every object. Models can also be used to represent selection-list options.

Consider the relationships between objects. Relationships include:

- one to one (OneToOneField)
- one to many (ForeignKey)
- many to many (ManyToManyField)

Multiplicities define the maximum and minimum number of each model that may be present in the relationship.

## Model definition

Models:

- Defined in `models.py`
- Extend `django.db.models.Model` class
- Can include fields, methods and metadata

## Fields

- Fields in a model represent columns in a database table.
- Each record (row) in the table contains values for these fields.
- Field types are designated using specific classes that define the type of data stored.
- Field types can also be used for HTML form validation.

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
- Provides a readable string representation of the object
- The string represents individual records

### `get_absolute_url()`
- Generates a URL to view individual records of the model

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
- `CharField` is used for the book's title and isbn.
- `unique` as `true` ensures all books have a unique ISBN
- `title` is not set to be unique, because it is possible for different books to have the same name.
- `TextField` for longer summary
- `ManyToManyField` a book can have multiple genres and a genre can have many books

## In both field types:

- The first unnamed parameter should specify the related model class
  - either directly by the model class
  - or as a string with the name of the related model
- If the associated class is not yet defined, use the model's name as a string in this file
- Setting `null=True` permits the database to store `Null` if no author is selected
- Using `on_delete=models.RESTRICT` prevents the deletion of the book's author if it is referenced by any book

## Warning:

- The default behavior is `on_delete=models.CASCADE`
- This means that if the author is deleted, the book would also be deleted
- Use `RESTRICT` or `PROTECT` to avoid the author being deleted while it is referenced by any book
- Alternatively, use `SET_NULL` to set the book's author to `Null` if the author record is deleted
- The `__str__()` method represents a Book record by its title field
- The `get_absolute_url()` method provides a URL to access a detailed record

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
