# Python Learning Curriculum: Intermediate Level

Building upon the beginner foundations, this section delves into more complex Python features and programming paradigms, preparing learners for more sophisticated tasks.

## II. Intermediate Level

### A. Advanced Data Structures

Beyond the basics, Python offers more specialized and efficient ways to handle data collections.

**1. List Comprehensions:**
A concise and readable way to create lists. They often replace multi-line `for` loops used for generating lists, making code shorter and sometimes faster.
*   **Syntax:** `[expression for item in iterable if condition]` (the `if` part is optional).
*Example:*
```python
# Traditional way
squares = []
for i in range(10):
    if i % 2 == 0:
        squares.append(i * i)

# Using list comprehension
even_squares = [x*x for x in range(10) if x % 2 == 0]
print(even_squares) # Output: [0, 4, 16, 36, 64]
```
*Suggested Resource:* Real Python on List Comprehensions (https://realpython.com/list-comprehension-python/)

**2. Dictionary Comprehensions:**
Similar to list comprehensions, but used to create dictionaries concisely.
*   **Syntax:** `{key_expression: value_expression for item in iterable if condition}`
*Example:*
```python
numbers = [1, 2, 3, 4, 5]
square_dict = {num: num*num for num in numbers}
print(square_dict) # Output: {1: 1, 2: 4, 3: 9, 4: 16, 5: 25}
```
*Suggested Resource:* Python Docs on Comprehensions (https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions)

**3. Generators and Iterators:**
*   **Iterators:** Objects representing a stream of data. They use the `__iter__()` and `__next__()` methods. `for` loops implicitly use iterators.
*   **Generators:** A simpler way to create iterators using functions with the `yield` keyword. `yield` pauses function execution and returns a value, resuming from the same point on the next call. Generators are memory-efficient as they produce items one at a time, only when needed, instead of storing the entire sequence in memory.
*Example (Generator Function):*
```python
def count_up_to(n):
    i = 1
    while i <= n:
        yield i
        i += 1

counter = count_up_to(5)
print(next(counter)) # Output: 1
print(next(counter)) # Output: 2
for number in counter: # Continues from where next() left off
    print(number) # Output: 3, 4, 5

# Generator Expression (similar syntax to list comprehension)
gen_exp = (x*x for x in range(5))
print(list(gen_exp)) # Output: [0, 1, 4, 9, 16]
```
*Suggested Resource:* Real Python on Generators (https://realpython.com/introduction-to-python-generators/)

**4. Collections Module:**
Provides specialized container datatypes beyond the built-in list, dict, set, and tuple.
*   **`Counter`:** A dict subclass for counting hashable objects. Useful for frequency analysis.
*   **`defaultdict`:** A dict subclass that calls a factory function to supply missing values. Avoids `KeyError`.
*   **`namedtuple`:** Factory function for creating tuple subclasses with named fields. Improves code readability.
*   **`deque`:** A list-like container with fast appends and pops from both ends. Useful for queues and stacks.
*Example (`Counter`, `defaultdict`):*
```python
from collections import Counter, defaultdict

# Counter
word_counts = Counter("mississippi")
print(word_counts) # Output: Counter({"i": 4, "s": 4, "p": 2, "m": 1})
print(word_counts["s"]) # Output: 4

# defaultdict
default_factory = lambda: "Unknown"
d = defaultdict(default_factory, {"name": "Alice"})
print(d["name"]) # Output: Alice
print(d["city"]) # Output: Unknown (key "city" was added)
```
*Suggested Resource:* Python Docs on `collections` (https://docs.python.org/3/library/collections.html)

### B. Object-Oriented Programming (OOP)

OOP is a programming paradigm based on the concept of "objects", which can contain data (attributes) and code (methods). It helps structure larger programs, promotes code reuse, and makes complex systems easier to manage.

**1. Classes and Objects:**
A class is a blueprint for creating objects. An object (or instance) is a specific realization of a class.
*   **Defining a Class:** Use the `class` keyword.
*   **Creating an Object (Instantiation):** Call the class name like a function.
*Example:*
```python
class Dog:
    # Class attribute (shared by all instances)
    species = "Canis familiaris"

    # Initializer / Constructor method
    def __init__(self, name, breed):
        # Instance attributes (unique to each instance)
        self.name = name
        self.breed = breed

# Creating Dog objects (instances)
dog1 = Dog("Buddy", "Golden Retriever")
dog2 = Dog("Lucy", "Poodle")

print(f"{dog1.name} is a {dog1.breed}") # Output: Buddy is a Golden Retriever
print(f"All dogs are {Dog.species}") # Accessing class attribute
```

**2. Attributes and Methods:**
*   **Attributes:** Variables associated with a class or an instance (store data).
*   **Methods:** Functions defined inside a class that operate on objects (define behavior). The first parameter of an instance method is conventionally named `self`, referring to the instance itself.
*Example (adding a method):*
```python
class Dog:
    # ... (previous code) ...

    # Instance method
    def bark(self):
        print(f"{self.name} says Woof!")

dog1.bark() # Output: Buddy says Woof!
```

**3. Inheritance:**
Allows a new class (subclass or child class) to inherit attributes and methods from an existing class (superclass or parent class). Promotes code reuse and establishes an "is-a" relationship (e.g., a Poodle *is a* Dog).
*Example:*
```python
class Poodle(Dog): # Poodle inherits from Dog
    def __init__(self, name, size):
        # Call the parent class's __init__ method
        super().__init__(name, breed="Poodle")
        self.size = size

    # Override the bark method
    def bark(self):
        print(f"{self.name} says Yap!")

    # New method specific to Poodle
    def groom(self):
        print(f"Grooming {self.name} the {self.size} Poodle.")

my_poodle = Poodle("Fifi", "Toy")
my_poodle.bark() # Output: Fifi says Yap! (overridden method)
my_poodle.groom() # Output: Grooming Fifi the Toy Poodle.
print(my_poodle.species) # Output: Canis familiaris (inherited attribute)
```

**4. Polymorphism:**
Literally "many forms". Allows objects of different classes to respond to the same method call in their own specific ways (as seen with the overridden `bark` method above). This enables writing more generic code that can work with different object types.

**5. Encapsulation:**
The bundling of data (attributes) and methods that operate on the data within a single unit (class). It often involves restricting direct access to some of an object's components, which can prevent accidental modification of data. Python uses naming conventions for this:
*   `_single_underscore`: Convention indicating an attribute is intended for internal use (protected).
*   `__double_underscore`: Triggers name mangling, making it harder (but not impossible) to access directly from outside the class (pseudo-private).

**6. Special Methods (Dunder methods):**
Methods with double underscores at the beginning and end (e.g., `__init__`, `__str__`). They allow you to define how objects behave with built-in Python operations.
*   `__init__(self, ...)`: Constructor, called when an object is created.
*   `__str__(self)`: Returns a user-friendly string representation (used by `print()`).
*   `__repr__(self)`: Returns an unambiguous, developer-focused string representation.
*   `__len__(self)`: Returns the length (used by `len()`).
*   `__add__(self, other)`: Defines behavior for the `+` operator.
*Example (`__str__`):*
```python
class Dog:
    # ... (__init__ etc.) ...
    def __str__(self):
        return f"Dog(name={self.name}, breed={self.breed})"

print(dog1) # Output: Dog(name=Buddy, breed=Golden Retriever)
```
*Suggested Resource:* Real Python OOP Tutorials (https://realpython.com/python3-object-oriented-programming/), Python Docs on Classes (https://docs.python.org/3/tutorial/classes.html)

### C. Advanced Functions

Exploring more powerful ways to define and use functions.

**1. Decorators:**
A way to modify or enhance functions or methods. Decorators are functions that take another function as input, add some functionality, and return the modified function. Often used for logging, access control, instrumentation, etc. The `@decorator_name` syntax is syntactic sugar.
*Example (Simple Decorator):*
```python
def my_decorator(func):
    def wrapper():
        print("Something is happening before the function is called.")
        func()
        print("Something is happening after the function is called.")
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")

say_hello()
# Output:
# Something is happening before the function is called.
# Hello!
# Something is happening after the function is called.
```
*Suggested Resource:* Real Python Primer on Decorators (https://realpython.com/primer-on-python-decorators/)

**2. Closures:**
A closure occurs when a nested function remembers and has access to variables from its containing (enclosing) function's scope, even after the outer function has finished executing.
*Example:*
```python
def outer_function(text):
    def inner_function():
        print(text) # inner_function remembers 'text'
    return inner_function

my_func = outer_function("Hi there!")
my_func() # Output: Hi there!
```

**3. `*args` and `**kwargs`:**
Allow functions to accept an arbitrary number of arguments.
*   `*args`: Collects extra positional arguments into a tuple.
*   `**kwargs`: Collects extra keyword arguments into a dictionary.
*Example:*
```python
def process_data(required_arg, *args, **kwargs):
    print("Required:", required_arg)
    print("Positional args (*args):", args)
    print("Keyword args (**kwargs):", kwargs)

process_data(100, 1, 2, 3, name="Alice", city="London")
# Output:
# Required: 100
# Positional args (*args): (1, 2, 3)
# Keyword args (**kwargs): {'name': 'Alice', 'city': 'London'}
```

**4. Recursion:**
A function calling itself to solve a problem by breaking it down into smaller, self-similar subproblems. Requires a base case to stop the recursion.
*Example (Factorial):*
```python
def factorial(n):
    if n == 0 or n == 1: # Base case
        return 1
    else: # Recursive step
        return n * factorial(n - 1)

print(factorial(5)) # Output: 120
```
*Caution:* Deep recursion can lead to stack overflow errors. Iterative solutions are often preferred for performance and stability.

*(Continue drafting remaining Intermediate sections: D, E, F, G, H)*



### D. Modules and Packages (Deeper Dive)

Organizing larger Python projects effectively.

**1. Creating Your Own Modules:**
Any Python file (`.py`) can be a module. You can import functions, classes, or variables defined in one Python file into another using the `import` statement. This promotes code reuse and organization.
*Example (`utils.py`):*
```python
# utils.py
def format_name(first, last):
    return f"{last.upper()}, {first.capitalize()}"

PI = 3.14159
```
*Example (`main.py` in the same directory):*
```python
# main.py
import utils
# from utils import format_name, PI # Alternative import

print(utils.format_name("alice", "smith")) # Output: SMITH, Alice
print(utils.PI)
```

**2. Structuring Larger Projects (Packages):**
A package is a collection of modules organized in a directory hierarchy. To make a directory a package, it must contain a special file named `__init__.py` (which can be empty). This file tells Python that the directory should be treated as a package, allowing you to import modules from it using dot notation (e.g., `import my_package.my_module`).
*Example Structure:*
```
my_project/
├── main.py
└── my_package/
    ├── __init__.py
    ├── module1.py
    └── sub_package/
        ├── __init__.py
        └── module2.py
```
*Example (`main.py`):*
```python
from my_package import module1
from my_package.sub_package import module2

module1.function1()
module2.function2()
```
*Suggested Resource:* Python Docs on Packages (https://docs.python.org/3/tutorial/modules.html#packages)

**3. Virtual Environments:**
Essential for managing project dependencies. A virtual environment creates an isolated Python environment for a specific project, allowing you to install packages for that project without affecting your global Python installation or other projects. This prevents version conflicts between packages required by different projects.
*   **Tools:** `venv` (built-in standard library module) or `conda` (popular alternative, especially in data science).
*   **Creating (`venv`):** `python -m venv my_env_name` (creates a directory `my_env_name`)
*   **Activating:**
    *   macOS/Linux: `source my_env_name/bin/activate`
    *   Windows: `my_env_name\Scripts\activate`
*   **Deactivating:** `deactivate` (run in the terminal)
*   **Usage:** Once activated, `pip install` installs packages only within that environment.
*Suggested Resource:* Python Docs on `venv` (https://docs.python.org/3/library/venv.html), Real Python on Virtual Environments (https://realpython.com/python-virtual-environments-a-primer/)

### E. Working with Data

Handling common data formats and interacting with databases.

**1. Regular Expressions (`re` module):**
Regular expressions (regex) are powerful mini-languages for defining search patterns in text. Python's `re` module provides tools for matching patterns, searching, splitting, and replacing text based on these patterns.
*   **Common Functions:** `re.search()`, `re.match()`, `re.findall()`, `re.sub()`, `re.split()`.
*   **Syntax:** Involves special characters (e.g., `.`, `*`, `+`, `?`, `[]`, `\d`, `\w`, `^`, `$`).
*Example:*
```python
import re

text = "The quick brown fox jumps over the lazy dog. Phone: 123-456-7890"

# Find all words starting with 'f'
matches = re.findall(r'\bf\w+', text, re.IGNORECASE)
print(matches) # Output: ['fox']

# Find a phone number pattern
phone_match = re.search(r'\d{3}-\d{3}-\d{4}', text)
if phone_match:
    print(phone_match.group()) # Output: 123-456-7890
```
*Suggested Resource:* Python Docs on `re` (https://docs.python.org/3/library/re.html), Real Python Regex Guide (https://realpython.com/regex-python/)

**2. Working with JSON Data (`json` module):**
JSON (JavaScript Object Notation) is a lightweight, human-readable data interchange format, commonly used in web APIs. Python's `json` module makes it easy to parse JSON strings into Python dictionaries/lists and serialize Python objects into JSON strings.
*   **`json.loads(json_string)`:** Parses a JSON string into a Python object.
*   **`json.load(file_pointer)`:** Reads JSON data from a file.
*   **`json.dumps(python_object, indent=4)`:** Serializes a Python object into a JSON formatted string (indent for pretty-printing).
*   **`json.dump(python_object, file_pointer, indent=4)`:** Writes JSON data to a file.
*Example:*
```python
import json

# Python dictionary
data = {"name": "Eve", "age": 28, "city": "New York"}

# Serialize to JSON string
json_string = json.dumps(data, indent=2)
print(json_string)

# Parse JSON string back to Python object
parsed_data = json.loads(json_string)
print(parsed_data["city"]) # Output: New York
```
*Suggested Resource:* Real Python on JSON (https://realpython.com/python-json/)

**3. Working with CSV Data (`csv` module):**
CSV (Comma Separated Values) is a common format for tabular data, often used in spreadsheets and databases. The `csv` module helps read and write CSV files.
*   **`csv.reader(file_pointer)`:** Creates an reader object to iterate over rows in the CSV.
*   **`csv.writer(file_pointer)`:** Creates a writer object to write rows to the CSV.
*   **`csv.DictReader` / `csv.DictWriter`:** Work with rows as dictionaries (using header row as keys).
*Example (Reading):*
```python
import csv

try:
    with open('data.csv', mode='r', newline='') as file:
        reader = csv.reader(file)
        header = next(reader) # Read the header row
        print("Header:", header)
        for row in reader:
            print(row) # Each row is a list of strings
except FileNotFoundError:
    print("data.csv not found")
```
*Suggested Resource:* Real Python on CSV (https://realpython.com/python-csv/)

**4. Introduction to Databases (`sqlite3` module):**
Basic interaction with relational databases. SQLite is a lightweight, file-based database engine included in Python's standard library (`sqlite3`), making it great for learning SQL and database concepts without needing a separate database server.
*   **Steps:** Connect to database, create cursor, execute SQL queries (CREATE TABLE, INSERT, SELECT, UPDATE, DELETE), fetch results, commit changes, close connection.
*   **SQL Basics:** Understanding basic SQL commands is necessary.
*Example:*
```python
import sqlite3

conn = sqlite3.connect('my_database.db') # Creates file if not exists
cursor = conn.cursor()

# Create table (if not exists)
cursor.execute('''
CREATE TABLE IF NOT EXISTS users (
    id INTEGER PRIMARY KEY, 
    name TEXT, 
    email TEXT
)
''')

# Insert data (use placeholders to prevent SQL injection)
cursor.execute("INSERT INTO users (name, email) VALUES (?, ?)", ("Alice", "alice@example.com"))

# Query data
cursor.execute("SELECT * FROM users WHERE name = ?", ("Alice",))
user = cursor.fetchone()
print(user) # Output: (1, 'Alice', 'alice@example.com')

conn.commit() # Save changes
conn.close() # Close connection
```
*Suggested Resource:* Python Docs on `sqlite3` (https://docs.python.org/3/library/sqlite3.html)

### F. Web Scraping (Basics)

Extracting data from websites automatically.

**1. Using `requests` library:**
A popular third-party library (`pip install requests`) for making HTTP requests (GET, POST, etc.) to fetch web page content (HTML) or interact with web APIs.
*Example:*
```python
import requests

try:
    response = requests.get('https://www.python.org')
    response.raise_for_status() # Raise an exception for bad status codes (4xx or 5xx)
    print(f"Status Code: {response.status_code}")
    # print(response.text[:500]) # Print first 500 characters of HTML
except requests.exceptions.RequestException as e:
    print(f"Error fetching URL: {e}")
```

**2. Introduction to `BeautifulSoup`:**
A library (`pip install beautifulsoup4`) for parsing HTML and XML documents. It creates a parse tree from the page source code that can be used to navigate, search, and modify the tree, making it easy to extract specific data.
*Example (requires `requests` and `beautifulsoup4` installed):*
```python
import requests
from bs4 import BeautifulSoup

url = 'https://news.ycombinator.com'
try:
    response = requests.get(url)
    response.raise_for_status()

    soup = BeautifulSoup(response.text, 'html.parser')

    # Find all story links (example selector, might change)
    story_links = soup.select('.titleline > a') 

    for link in story_links:
        print(link.get_text(), "->", link['href'])

except requests.exceptions.RequestException as e:
    print(f"Error fetching URL: {e}")
except Exception as e:
    print(f"Error parsing HTML: {e}")
```
*Note:* Web scraping should be done responsibly and ethically, respecting website `robots.txt` files and terms of service. Websites change structure frequently, breaking scrapers.
*Suggested Resource:* Real Python on Web Scraping (https://realpython.com/python-web-scraping-practical-introduction/), BeautifulSoup Docs (https://www.crummy.com/software/BeautifulSoup/bs4/doc/)

### G. Introduction to Testing

Writing automated tests to ensure your code works correctly and continues to work as you make changes.

**1. Why Test?**
Testing catches bugs early, prevents regressions (reintroducing old bugs), improves code design (testable code is often better designed), provides documentation (tests show how code is intended to be used), and gives confidence when refactoring or adding features.

**2. `unittest` or `pytest` basics:**
*   **`unittest`:** Python's built-in testing framework, based on xUnit style. Uses classes and methods starting with `test_`.
*   **`pytest`:** A popular third-party framework (`pip install pytest`) known for its simpler syntax (uses plain functions starting with `test_`), powerful features (fixtures, parametrization), and better reporting.
*Example (`pytest`):*
```python
# test_calculator.py (requires pytest installed)
# Assume calculator.py exists with an add function
# from calculator import add 

def add(a, b):
    return a + b

def test_add_positive():
    assert add(2, 3) == 5

def test_add_negative():
    assert add(-1, -1) == -2

def test_add_mixed():
    assert add(5, -3) == 2

# Run from terminal: pytest
```

**3. Assertions:**
Statements that check if a condition is true. If the condition is false, the test fails. `pytest` uses the standard `assert` statement. `unittest` uses specific assertion methods like `assertEqual()`, `assertTrue()`, `assertRaises()`.

*Suggested Resource:* `unittest` Docs (https://docs.python.org/3/library/unittest.html), `pytest` Docs (https://docs.pytest.org/), Real Python on Testing (https://realpython.com/python-testing/)

### H. Intermediate Projects

Applying intermediate concepts to build more substantial applications.

**1. Web Scraper:**
*   **Goal:** Choose a website (e.g., a news site, e-commerce product page, weather site) and extract specific pieces of information (headlines, prices, temperatures, links). Store the extracted data in a structured format (e.g., CSV, JSON).
*   **Concepts Used:** `requests`, `BeautifulSoup`, file handling (CSV/JSON), error handling, potentially basic data structures (lists, dictionaries).

**2. Simple Blog Application (using Flask or Django):**
*   **Goal:** Build a basic web application where users can view blog posts, and an admin can create/edit/delete posts. Focus on understanding the basics of the chosen web framework.
*   **Concepts Used:** Introduction to Flask or Django (routing, templates, basic database interaction/ORM), HTML basics, virtual environments, potentially basic OOP for models.
*   **Resources:** Flask Docs (https://flask.palletsprojects.com/), Django Docs (https://docs.djangoproject.com/), associated tutorials.

**3. Data Analysis Project:**
*   **Goal:** Find a dataset (e.g., from Kaggle, government sites) in CSV format. Use libraries like Pandas to load the data, clean it (handle missing values), perform basic analysis (calculate means, medians, correlations, group data), and optionally create simple visualizations using Matplotlib or Seaborn.
*   **Concepts Used:** `pandas` for data manipulation, `matplotlib`/`seaborn` for plotting, file handling (CSV), data structures (DataFrames, Series), basic statistics.
*   **Resources:** Pandas Docs (https://pandas.pydata.org/docs/), Matplotlib Docs (https://matplotlib.org/stable/contents.html), Seaborn Docs (https://seaborn.pydata.org/api.html).
