# [Pyrebase](https://pypi.python.org/pypi/Pyrebase)

A simple python wrapper for the [Firebase API](https://www.firebase.com/docs/rest/guide/).

## Installation

pip install pyrebase

## Usage

### Initialising your Firebase

#### Admin

Basic initialisation disregards your security rules and authenticates an admin.

```
admin = pyrebase.Firebase('https://yourfirebaseurl.firebaseio.com', 'yourfirebasesecret')
```

#### User

Passing in a third parameter containing a uid will authenticate a user.

```
user = pyrebase.Firebase('https://yourfirebaseurl.firebaseio.com', 'yourfirebasesecret', 'simplelogin:1')
```


### Saving Data

#### POST

To save data with a unique, auto-generated, timestamp-based key, use the POST method.

```python
data = '{"name": "Marty Mcfly", "date": "05-11-1955"}'
ref.post("users", data)
```

Example of an auto-generated key: -JqSjGteC4SRzNJ2hH52

#### PUT

To create your own keys use the PUT method. The key in the example below is "Marty".

```python
data = '{"Marty": {"name": "Marty Mcfly", "date":"05-11-1955"}}'
ref.put("users", data)
```

#### PATCH

To update data for an existing entry use the PATCH method.

```python
data = '{"date": "26-10-1985"}'
ref.patch("users", "Marty", data)
```
Updates "05-11-1955" to "26-10-1985".

### Reading Data

#### Simple Queries

##### all

Takes a database reference, returning all reference data.

```python
all_users = ref.all("users")
```

##### one

Takes a database reference and a key, returning a single entry.

```python
one_user = ref.one("users", "Marty Mcfly")
```

##### sort_by

Takes a database reference and a property, returning all reference data sorted by property.

```python
users_by_name = ref.sort_by("users", "name")

for i in users_by_name:
    print(i["name"])
```

#### Complex Queries

##### sort_by_first

Takes a database reference, a property, a starting value, and a return limit,
returning limited reference data sorted by property.

```python
results = ref.sort_by_first("users", "age", 25, 10)
```

This query returns 10 users with an age of 25 or more.

##### sort_by_last

Takes a database reference, a property, a starting value, and a return limit,
returning limited reference data sorted by property.


```python
results = ref.sort_by_last("users", "age", 25, 10)
```

This query returns 10 users with an age of 25 or less.


### Common Errors

#### Index not defined

Indexing is [not enabled](https://www.firebase.com/docs/security/guide/indexing-data.html) for the database reference.

#### Property types don't match

Returned properties are not of the same data type.
Example: name: 0 and name: "hello".