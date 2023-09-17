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

Having added the two books, we had to ensure that the correct book cover images were added to visually identify the entries in the website. This match was achieved by adding the following html to the books.html file:

{% raw %}
```html
<img src="static/image{{book['id']}}.png" width = "50">
```
{% endraw %}

The function is then used in the code below to efficiently output a single Data Frame with all of the data:

```python
list_df = []
years = []
year = 2020
while year >= 1992:
    years.append(str(year))
    year -= 1

for year in years:
    add_dataframe(list_df, year)
    
df_stacked = pd.concat(list_df)
df_stacked.reset_index(inplace=True)
df_stacked.drop("index", axis=1, inplace=True)
```

I created a separate YAML file that contained the credentials of the database connection that had to be established to load the data into a designated table. The benefit of this type of file is that it stores this sensitive information in a separate file so that it is not hard-coded in the main file.

Once the connection was made, I created the table and loaded the records from the Data Frame into the table, as shown below:

```python
# Secure Connection to the Database
db = yaml.safe_load(open("db.yaml"))
config = {
    "user":     db["user"],
    "password": db["pwrd"],
    "host":     db["host"],
    "auth_plugin":  "mysql_native_password"
}
cnx = mysql.connector.connect(**config)

MyCursor=cnx.cursor()
queries = []

# Creating the database and the table `mrts`
queries.append("DROP DATABASE IF EXISTS mrtsdb")
queries.append("CREATE DATABASE mrtsdb")
queries.append("USE mrtsdb")
queries.append("""CREATE TABLE mrts (
    kind_of_business VARCHAR (300) NOT NULL,
    value FLOAT NOT NULL,
    period DATE NULL) ENGINE=InnoDB DEFAULT CHARSET=UTF8MB4 COLLATE=utf8mb4_0900_ai_ci""")

for query in queries:
    MyCursor.execute(query)

...

# Inserting each row of the DataFrame to the MySQL table and committing
for row_num in range(0, len(df_stacked)):
    row_data = df_stacked.iloc[row_num]
    value = (row_data[0], row_data[1], row_data[2])
    sql = "INSERT INTO mrts (kind_of_business, value, period) VALUES (%s, %s, %s)"
    MyCursor.execute(sql, value)

cnx.commit()

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
