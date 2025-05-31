# Python Learning Curriculum: Advanced Level (Pro)

This section covers advanced Python concepts, specialized domains, and software engineering best practices necessary for professional Python development.

## III. Advanced Level (Pro)

### A. Advanced OOP Concepts

Mastering the intricacies of Python's object model.

**1. Metaclasses:**
In Python, classes are themselves objects, and they are created by metaclasses. The default metaclass is `type`. Understanding metaclasses allows you to customize class creation itself, enabling powerful metaprogramming techniques like automatically adding methods, registering classes, or implementing custom object models (e.g., ORMs).
*   **Concept:** Classes are instances of their metaclass. `type` is the default metaclass.
*   **Custom Metaclass:** Define a class inheriting from `type` and implement `__new__` or `__init__` to control class creation.
*Example (Simple Metaclass):*
```python
class MyMeta(type):
    def __new__(cls, name, bases, dct):
        print(f"Creating class: {name}")
        # Add a custom attribute to the class being created
        dct["custom_attribute"] = 100
        return super().__new__(cls, name, bases, dct)

class MyClass(metaclass=MyMeta):
    pass

print(MyClass.custom_attribute) # Output: 100
```
*Suggested Resource:* Real Python on Metaclasses (https://realpython.com/python-metaclasses/), Fluent Python Ch. 21.

**2. Abstract Base Classes (ABCs):**
Used to define interfaces or enforce that subclasses implement specific methods. The `abc` module provides the `ABC` metaclass and the `@abstractmethod` decorator.
*   **Purpose:** Define common APIs for a set of subclasses, ensuring consistency.
*   **Usage:** Inherit from `abc.ABC` and decorate methods that must be implemented by subclasses with `@abc.abstractmethod`.
*Example:*
```python
import abc

class Vehicle(abc.ABC):
    @abc.abstractmethod
    def start_engine(self):
        pass

    @abc.abstractmethod
    def stop_engine(self):
        pass

class Car(Vehicle):
    def start_engine(self):
        print("Car engine started.")

    def stop_engine(self):
        print("Car engine stopped.")

# my_vehicle = Vehicle() # TypeError: Can't instantiate abstract class
my_car = Car()
my_car.start_engine()
```
*Suggested Resource:* Python Docs on `abc` (https://docs.python.org/3/library/abc.html)

**3. Design Patterns in Python:**
Reusable solutions to common software design problems. While Python's dynamic nature sometimes offers simpler alternatives, understanding classic patterns (Gang of Four - GoF) and their Pythonic implementations is valuable.
*   **Examples:** Singleton, Factory (Method/Abstract), Builder, Prototype, Adapter, Decorator, Facade, Proxy, Observer, Strategy, Template Method, State.
*   **Pythonic Approach:** Often leverages first-class functions, decorators, context managers, or dynamic typing instead of strict class hierarchies.
*Example (Simple Strategy Pattern):*
```python
import abc

class Exporter(abc.ABC):
    @abc.abstractmethod
    def export(self, data):
        pass

class JSONExporter(Exporter):
    def export(self, data):
        print(f"Exporting data to JSON: {data}")

class CSVExporter(Exporter):
    def export(self, data):
        print(f"Exporting data to CSV: {data}")

class DataProcessor:
    def __init__(self, exporter: Exporter):
        self._exporter = exporter

    def process_and_export(self, data):
        print(f"Processing data: {data}")
        self._exporter.export(data)

data = {"id": 1, "value": "test"}
json_processor = DataProcessor(JSONExporter())
json_processor.process_and_export(data)

csv_processor = DataProcessor(CSVExporter())
csv_processor.process_and_export(data)
```
*Suggested Resource:* "Design Patterns: Elements of Reusable Object-Oriented Software" (GoF book), Various online resources and Python-specific pattern books/articles.

### B. Concurrency and Parallelism

Executing multiple tasks seemingly simultaneously (concurrency) or actually simultaneously (parallelism) to improve performance or responsiveness.

**1. Threading (`threading` module):**
Runs multiple threads within the same process. Threads share the same memory space. Useful for I/O-bound tasks (e.g., network requests, file operations) where threads can wait for I/O without blocking the entire program.
*   **Challenge:** The Global Interpreter Lock (GIL) in CPython prevents true parallel execution of Python bytecode on multiple CPU cores for CPU-bound tasks.
*Example:*
```python
import threading
import time

def worker(name):
    print(f"Thread {name} starting")
    time.sleep(2)
    print(f"Thread {name} finishing")

threads = []
for i in range(3):
    t = threading.Thread(target=worker, args=(i,))
    threads.append(t)
    t.start()

for t in threads:
    t.join() # Wait for all threads to complete

print("All threads finished")
```

**2. Multiprocessing (`multiprocessing` module):**
Runs multiple independent processes, each with its own Python interpreter and memory space. Bypasses the GIL, allowing true parallel execution on multiple CPU cores. Ideal for CPU-bound tasks (e.g., complex calculations, data processing).
*   **Challenge:** Inter-process communication (IPC) is more complex and slower than intra-thread communication.
*Example:*
```python
import multiprocessing
import time

def cpu_task(name):
    print(f"Process {name} starting")
    # Simulate CPU-bound work
    count = 0
    for i in range(10**7):
        count += i
    print(f"Process {name} finishing")

if __name__ == "__main__": # Important guard for multiprocessing
    processes = []
    for i in range(3):
        p = multiprocessing.Process(target=cpu_task, args=(i,))
        processes.append(p)
        p.start()

    for p in processes:
        p.join()

    print("All processes finished")
```

**3. Asynchronous Programming (`asyncio`, `async`/`await`):**
A concurrency model based on event loops and coroutines (`async def` functions). Allows handling many I/O-bound operations concurrently within a single thread, efficiently managing waiting time without blocking. Uses `async` to define coroutines and `await` to pause execution until an awaited coroutine/task completes.
*   **Use Case:** High-performance network applications (servers, clients), GUIs.
*Example:*
```python
import asyncio
import time

async def say_after(delay, what):
    await asyncio.sleep(delay)
    print(what)

async def main():
    print(f"started at {time.strftime("%X")}")

    task1 = asyncio.create_task(say_after(1, "hello"))
    task2 = asyncio.create_task(say_after(2, "world"))

    await task1
    await task2

    print(f"finished at {time.strftime("%X")}")

if __name__ == "__main__":
    asyncio.run(main())
```

**4. Global Interpreter Lock (GIL):**
A mutex in CPython that protects access to Python objects, preventing multiple threads from executing Python bytecode *in parallel* within a single process. It simplifies memory management but limits CPU-bound parallelism in threads. Does not affect multiprocessing or `asyncio`'s concurrency benefits for I/O-bound tasks.

*Suggested Resources:* Real Python Concurrency Tutorials, Python Docs on `threading`, `multiprocessing`, `asyncio`, Talk Python Training courses.

### C. Web Development (Frameworks)

Building robust web applications and APIs.

**1. Deep Dive into Django or Flask:**
Mastering a chosen framework beyond the basics.
*   **Django:** ORM (Object-Relational Mapper) intricacies, advanced querying, custom model fields/managers, Class-Based Views (CBVs), middleware, context processors, form handling/validation, Django REST Framework (DRF) for APIs, Channels for WebSockets, testing strategies, deployment considerations.
*   **Flask:** Blueprints for organization, application factories, context management (`g`, `request`, `session`), advanced routing, extensions (Flask-SQLAlchemy, Flask-Migrate, Flask-RESTful/Flask-Marshmallow), testing with `pytest`, deployment with WSGI servers.

**2. Building APIs:**
Designing and implementing RESTful APIs.
*   **Frameworks:** Django REST Framework (DRF), Flask-RESTful, FastAPI (modern, high-performance, based on type hints).
*   **Concepts:** REST principles (resources, methods, representations), serialization/deserialization, authentication (Token, JWT, OAuth), authorization/permissions, versioning, documentation (Swagger/OpenAPI).

**3. WebSockets:**
Enabling full-duplex, real-time communication between client and server (e.g., chat applications, live updates).
*   **Libraries/Frameworks:** Django Channels, `websockets` library, `Socket.IO` integration.

**4. Authentication and Authorization:**
Securely managing user identity and access control.
*   **Authentication:** Verifying user identity (e.g., username/password, social login, tokens).
*   **Authorization:** Determining what an authenticated user is allowed to do (permissions).
*   **Techniques:** Session-based auth, Token-based auth (JWT - JSON Web Tokens), OAuth2 (for third-party authorization).
*   **Libraries:** Framework built-ins, `Authlib`, `python-jose`, etc.

**5. Deployment:**
Making your web application accessible on the internet.
*   **Containerization:** Packaging applications and dependencies using Docker.
*   **WSGI Servers:** Running Python web applications (e.g., Gunicorn, uWSGI).
*   **Reverse Proxy:** Handling incoming requests, load balancing, serving static files (e.g., Nginx, Apache).
*   **Cloud Platforms:** Deploying to services like AWS (EC2, Elastic Beanstalk, Lambda), Google Cloud (App Engine, Cloud Run), Azure (App Service), Heroku, DigitalOcean.
*   **Serverless:** Using platforms like AWS Lambda or Google Cloud Functions with API Gateway.

*Suggested Resources:* Official documentation for Django, Flask, DRF, FastAPI. Real Python web development tutorials. Talk Python Training courses.

### D. Data Science and Machine Learning (Specialization Path)

Leveraging Python for data analysis, visualization, and building predictive models.

**1. NumPy:**
Fundamental package for numerical computing. Provides powerful N-dimensional array objects, linear algebra, Fourier transform, and random number capabilities. Essential for performance-critical computations.
*   **Key Concepts:** `ndarray` object, vectorization (avoiding explicit loops), broadcasting, indexing/slicing.

**2. Pandas:**
High-performance, easy-to-use data structures (DataFrame, Series) and data analysis tools. Built on top of NumPy.
*   **Key Concepts:** Data loading/saving (CSV, Excel, SQL), data cleaning/wrangling (handling missing data, merging, reshaping), indexing/selection (`loc`, `iloc`), grouping (`groupby`), time series analysis.

**3. Matplotlib/Seaborn:**
*   **Matplotlib:** Foundational plotting library, provides fine-grained control over plots.
*   **Seaborn:** Built on Matplotlib, provides a high-level interface for drawing attractive and informative statistical graphics.
*   **Concepts:** Creating various plot types (line, scatter, bar, histogram, boxplot), customizing plots, subplots.

**4. Scikit-learn:**
Comprehensive library for machine learning.
*   **Features:** Classification, regression, clustering, dimensionality reduction algorithms, model selection (cross-validation, grid search), preprocessing (scaling, encoding), evaluation metrics.
*   **Workflow:** Data loading -> Preprocessing -> Model Training -> Evaluation -> Prediction.

**5. Introduction to Deep Learning Frameworks (Optional):**
*   **TensorFlow/Keras:** Google's framework for large-scale machine learning and deep neural networks. Keras provides a high-level API.
*   **PyTorch:** Facebook's framework, popular in research, known for its flexibility and Pythonic feel.
*   **Concepts:** Neural networks, layers, activation functions, loss functions, optimizers, training loops.

*Suggested Resources:* Official documentation for NumPy, Pandas, Matplotlib, Seaborn, Scikit-learn, TensorFlow, PyTorch. DataCamp/Coursera courses. Books like "Python for Data Analysis" by Wes McKinney.

### E. Advanced Python Features

Leveraging more sophisticated language features.

**1. Context Managers (`with` statement):**
Objects that manage resources (like files or network connections) by ensuring setup and teardown actions occur. Implement `__enter__` and `__exit__` methods or use the `contextlib` module (`@contextmanager` decorator).
*Example (Custom Context Manager):*
```python
import time
from contextlib import contextmanager

@contextmanager
def timer(label):
    start = time.time()
    try:
        yield
    finally:
        end = time.time()
        print(f"{label}: {end - start:.3f}s")

with timer("Block execution"):
    # Code block to time
    time.sleep(1.5)
```

**2. Type Hinting (PEP 484):**
Adding type annotations to function signatures and variables. Does not affect runtime behavior (Python remains dynamically typed) but enables static analysis tools (like MyPy) to catch type errors before runtime, improves code readability and maintainability.
*Example:*
```python
def greet(name: str) -> str:
    return f"Hello, {name}"

pi: float = 3.14159
```

**3. Advanced Decorator Patterns:**
*   **Decorators with Arguments:** Creating decorators that accept arguments themselves.
*   **Class Decorators:** Using classes to implement decorators (implementing `__call__`).
*   **Stacking Decorators:** Applying multiple decorators to a single function.

**4. Working with C extensions (Optional):**
For performance-critical code, interfacing with C/C++ libraries or writing parts of your application in C/C++ and calling them from Python.
*   **Tools:** `ctypes` (call functions in shared libraries), `cffi` (alternative interface), `Cython` (compile Python-like code to C extensions), Python C API (direct C integration).

*Suggested Resources:* Fluent Python, Python Docs, Real Python articles on specific features.

### F. Software Development Best Practices

Professional practices for writing maintainable, robust, and collaborative code.

**1. Version Control (Advanced Git):**
Beyond basic commits and pushes.
*   **Branching Strategies:** Gitflow, GitHub Flow - managing feature development, releases, hotfixes.
*   **Rebasing:** Rewriting commit history for cleaner merges.
*   **Resolving Conflicts:** Handling merge conflicts effectively.
*   **Collaboration:** Pull requests, code reviews.

**2. Advanced Testing:**
*   **Mocking:** Using `unittest.mock` or `pytest-mock` to replace external dependencies (databases, APIs) during tests, isolating the code under test.
*   **Test-Driven Development (TDD):** Writing tests *before* writing the implementation code (Red-Green-Refactor cycle).
*   **Coverage:** Measuring how much of your code is executed by tests (`coverage.py`).

**3. Continuous Integration/Continuous Deployment (CI/CD):**
Automating the build, test, and deployment process.
*   **CI:** Automatically running tests whenever code is pushed to the repository.
*   **CD:** Automatically deploying code to staging or production environments after tests pass.
*   **Tools:** Jenkins, GitLab CI/CD, GitHub Actions, CircleCI.

**4. Code Quality Tools:**
*   **Linters:** Check for style errors and potential code issues (e.g., `Flake8`, `Pylint`).
*   **Formatters:** Automatically format code to conform to a style guide (e.g., `Black`, `isort`).
*   **Static Analysis:** Detect potential bugs and type errors (e.g., `MyPy`).

**5. Logging (`logging` module):**
Recording events, errors, and diagnostic information during program execution. More flexible and configurable than `print()` statements.
*   **Levels:** DEBUG, INFO, WARNING, ERROR, CRITICAL.
*   **Handlers:** Sending logs to files, console, network sockets, etc.
*   **Formatters:** Controlling the output format of log messages.

*Suggested Resources:* Pro Git book, documentation for testing frameworks and CI/CD tools, Real Python articles on best practices.

### G. Performance Optimization

Identifying and addressing performance bottlenecks.

**1. Profiling Code:**
Measuring where your code spends its time and memory.
*   **Tools:** `cProfile` (built-in time profiler), `line_profiler` (line-by-line time profiling), memory profilers (`memory_profiler`, `objgraph`).

**2. Memory Management:**
Understanding CPython's reference counting and garbage collection. Identifying memory leaks or excessive memory usage.

**3. Using efficient data structures and algorithms:**
Choosing the right tools (e.g., sets/dicts for lookups, appropriate sorting algorithms) can drastically improve performance. Understanding Big O notation.

*Suggested Resources:* Python Docs on profiling, High Performance Python book.

### H. Advanced/Pro Projects

Demonstrating mastery by building complex, real-world applications.

**1. Complex Web Application:**
*   **Goal:** Build a full-featured web application (e.g., e-commerce site, social media clone, project management tool) using Django or Flask/FastAPI. Include user authentication, database persistence, background tasks (Celery), potentially a REST API, and deploy it.

**2. Data Analysis Pipeline:**
*   **Goal:** Create an end-to-end pipeline: ingest data from various sources (APIs, databases, files), clean and transform it using Pandas/NumPy, perform statistical analysis or apply ML models using Scikit-learn, and generate reports or an interactive visualization dashboard (using Dash, Streamlit, or Bokeh).

**3. Machine Learning Model Deployment:**
*   **Goal:** Develop and train a machine learning model (e.g., image classification, sentiment analysis) using Scikit-learn, TensorFlow, or PyTorch. Package the model and deploy it as a web service (API) using Flask/FastAPI/DRF and Docker, allowing others to make predictions.

**4. Contribute to Open Source:**
*   **Goal:** Find an established Python project on GitHub that interests you. Start by fixing small bugs, improving documentation, or adding tests. Gradually contribute more significant features. Demonstrates collaboration and real-world coding skills.

**5. Build a Custom Tool/Library:**
*   **Goal:** Identify a recurring problem or missing functionality and create a reusable Python library or command-line tool to address it. Package it properly (using `setup.py` or `pyproject.toml`) and potentially publish it to PyPI.
