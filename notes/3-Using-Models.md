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

Consider the relationships between objects.

Relationships include:

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
- Field types are assigned with classes, which determine the type of record that is used to store the data in the database,
- Field types can be used for HTML form validation

# Field Arguments

- https://docs.djangoproject.com/en/5.0/ref/models/fields/#field-options
- `help_text`
- `verbose_name`
- `default`
- `null`
- `blank`
- `unique`
- `primary_key`

# COMMON FIELD TYPES

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

`__str__()`
- Python class method
- Returns a human-readable string representation of the object
- The string represents individual records

`get_absolute_url()`
- Returns a URL for displaying individual model records

## Model management

Use model classes to create, update, or delete records, and to run queries.

# Creating and modifying records

- Create a record by instantiating the model and using the model's constructor.
- Then call `save()` to save the object to the database.
- Access fields in the record using dot notation.
- Search for records using the model's objects attribute

The full list of lookups: https://docs.djangoproject.com/en/5.0/ref/models/querysets/#field-lookups

Making queries: https://docs.djangoproject.com/en/5.0/topics/db/queries/

