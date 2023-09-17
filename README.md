[Go to Back to Home Page](https://ukthanki.github.io/)

# MIT Data Engineering Professional Certification

## Books Web Application

<p align="center">
    <img width="100%" src="https://github.com/ukthanki/MIT_Books_Web_Application/assets/42117481/b9a29299-7d26-4e47-935f-eee6e297d448">
</p>

Up to this point in the course, we had learned the following topics:
1. NumPy
2. Pandas
3. SQL
4. Linear Regression
5. ETL Fundamentals
6. Flask
7. Java
8. CDC Fundamentals

In this project, we used the various skills we have learned to build website that shows a collection of books. We added images, users with usernames and passwords to access the site, and applied various roles to alter what actions users are able to perform. The project was presented to us in a way that a majority of the base code was already provided to us and we had to fill in various sections with code.

After installing the required libraries (flask-restful and flask-jwt-extended), I added two more books to the Books List object which initially contained 3 Dictionaries, each being a Book, as shown below:

```python
books = [
    {
        "id": 1,
        "author": "Eric Reis",
        "country": "USA",
        "language": "English",
        "title": "Lean Startup",
        "year": 2011,
    },
    {
        "id": 2,
        "author": "Mark Schwartz",
        "country": "USA",
        "language": "English",
        "title": "A Seat at the Table",
        "year": 2017,
    },
    {
        "id": 3,
        "author": "James Womak",
        "country": "USA",
        "language": "English",
        "title": "Lean Thinking",
        "year": 1996,
    },
        {
        "id": 4,
        "author": "J.K. Rowling",
        "country": "UK",
        "language": "English",
        "title": "Harry Potter and the Philosopher's Stone",
        "year": 1997,
    },
        {
        "id": 5,
        "author": "William Shakespeare",
        "country": "UK",
        "language": "English",
        "title": "Much Ado About Nothing",
        "year": 1623,
    },
]
```

Having added the two books, we had to ensure that the correct book cover images were added to visually identify the entries in the website. This match was achieved by adding the following html to the books.html file that connects the "id" field above with the file name that contains the number:

{% raw %}
```html
<img src="static/image{{book['id']}}.png" width = "50">
```
{% endraw %}

I also added two more users and assigned one the role of "admin" and the other the role of "reader", as shown below:

```python
users = [
 {"username": "testuser", "password": "testuser", "role": "admin"},
    {"username": "John", "password": "John", "role": "reader"},
    {"username": "Anne", "password": "Anne", "role": "admin"},
    {"username": "Umang", "password": "Umang", "role": "admin"},
    {"username": "Mike", "password": "Mike", "role": "reader"}
]
```

Finally, we added a function that allows users to add books only if they have been assigned an "admin" role; all other users should see an exception otherwise, as shown below:

```python
def admin_required(fn):
   @wraps(fn)
   def wrapper(*args, **kwargs):
        verify_jwt_in_request
        claims = get_jwt()
        if claims['role'] != 'admin':
            return jsonify(msg = 'Admins only!'), 403
        else:
            return fn(*args, **kwargs)
   return wrapper
```

With these code enhancements, logging into the website shows the following web page, presented in Figure 1:

| ![image](https://github.com/ukthanki/MIT_Books_Web_Application/assets/42117481/b264e31b-d682-496f-9b52-035672a79c1b)| 
|:--:| 
| **Figure 1.** Screenshot of the Book Web Application Home page. |

Consequently, we were also able to verify the other functionality added earlier by adding a new book with the "admin" user and seeing the expected error message when attempting to do so with the "reader" user.

This project was interesting as it allowed me to implement some basic html, a language I had never used before, and create a tangible web application that has relevant functionality.

**You can view the full Project in the "module_8.py" and "Module 8_Umang_Thanki.ipynb" files in the Repository.**

[Go to Repo](https://github.com/ukthanki/MIT_Books_Web_Application_Project)
