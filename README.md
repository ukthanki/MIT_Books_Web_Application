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

As a result, the data was effectively loaded into the database table, as shown below in Figure 1:

| ![download](https://github.com/ukthanki/MIT_MRTS_ETL/assets/42117481/ae5ff829-5165-419a-aa87-0663c492c8b0)| 
|:--:| 
| **Figure 1.** MRTS data loaded into the *mrts* table in MySQL. |

I was then able to execute various SELECT statements through Python to visualize the data to gain various insights. For example, by executing the following code below, I was able to plot the data using Matplotlib, as shown below in Figure 2:

```python
# (5) Retail and food services sales, total Yearly Trend
query5 = """
SELECT SUM(`value`), YEAR(period) FROM mrts WHERE kind_of_business = 'Retail and food services sales, total'
GROUP BY 2 ORDER BY period
"""
MyCursor.execute(query5)
month = []
sales = []
for row in MyCursor.fetchall():
    sales.append(row[0])
    month.append(row[1])
    
plt.plot(month, sales)
plt.title("Retail and food services sales, total - Yearly")
plt.xlabel("Year")
plt.ylabel("Sales (USD, Million)")
plt.show()
```

| ![download](https://github.com/ukthanki/MIT_MRTS_ETL/assets/42117481/0ee68c9d-b29c-4251-b37e-93067cfae930)| 
|:--:| 
| **Figure 2.** Sales vs. Year for Retail and Food Services. |

This project was quite insightful because it focused heavily on ETL and how it may be done programmatically as opposed to manually. I could have produced the same plots by performing all steps in Python only, but by loading the data in MySQL, it became available to a wider audience for querying and analysis; this represents a real-world situation as a result because data must be accessible easily by multiple entities.

**You can view the full Project in the "module_8.py" and "Module 8_Umang_Thanki.ipynb" files in the Repository.**

[Go to Repo](https://github.com/ukthanki/MIT_Books_Web_Application_Project)

In this project, we will modify provided code to create a website that allows users to see a list of books as well as add more books. We will add users, define roles, and ensure that only users with an 'admin' role are allowed to add books.
