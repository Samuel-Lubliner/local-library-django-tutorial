# Sample Data

- Is helpful when developing
- Sample data scripts expose issues with data models

## Querying in the shell

```bash
cd locallibrary
python3 manage.py shell
```

The interactive Python environment can be used to create records and query the database.

Import all necessary modules and models for the session.

```bash
from catalog.models import Book
Book.objects.all()
```

Outputs:

```bash
<QuerySet []>`
```

An empty query set was returned.

Add a book:

```bash
book = Book(title="Python Tutorial")
book.save()
Book.objects.all()
```

Outputs:

```bash
<QuerySet [<Book: Python Tutorial>]>
```

A book was successfully created. Importing everything in the beginning of the shell is painful.


Exit the the shell with `exit()`

## Querying with shell_plus

Navigate to the same directory as `requirements.txt`.

`shell_plus` doesn’t come with Django out-of-the-box.

```bash
$ pip install django-extensions
```

Add the new package to the list of installed packages.

Run `pip freeze` from the same directory as `requirements.txt` to update the file. Otherwise, you will create another `requirements.txt`.

```bash
pip freeze > requirements.txt
```

Go to `settings.py` and register the extension in `INSTALLED_APPS`.

Then, navigate to the `locallibrary` directory where `manage.py` is located and run:

```bash
python3 manage.py shell_plus
```

Now you can query without needing to import models manually.

## Sample data script

Install the Faker library and add it to `requirements.txt`

```bash
pip install Faker
pip freeze > requirements.txt
```

Run the sample data script:

```bash
python3 create_sample_data.py
```

To start querying run `python3 manage.py shell_plus` 
