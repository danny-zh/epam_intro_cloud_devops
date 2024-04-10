# Python Bootcamp

## 1. Virtual Environments

It allows the logical isolation of interpreter, libraries, packages, and binaries for different Python versions. These environments are useful when using multiple versions of Python in the same host. Some tools that allow the implementation of Python virtual environments are:

1. virtualenv
3. pyenv
4. pipenv
5. python -m venv

An example when using pyenv in shown in figure 1

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/92881d26-fadc-4e25-a47e-797444a9471f"
         alt="Figure 1" width="600" height="300"/>
  <br/>
  <em>Figure 1. Pyenv virutal env architecture</em>
</p>

## 2. Python Language Syntax
### 2.1 Loops
- While
  ``` 
  i=0
  while i <100:
    print(i)
    i++
  else:
    print("counter break") # The else will be execute if the while statement was terminated by a false condition rather than a break or return stament.
  ```
- For
  ``` 
  for item in range(100)
    print(item)
  else:
    print("counter break") # The else will be execute if the for statement was terminated by a false condition rather than a break or return stament.
  ```
- List comprehension
  ``` 
  [item for item in range(100]]
  ```

### 2.2 Strings

- Format: To format strings

  - `"this {0} is {1}".format("food", "awesome")`
  - `"this {subject} is {adjective}".format(subject="food" adjective="awesome") 'this food is awesome'`
  - `:.2f".format(number)'` # To format number to be printed wiht only two decimals value

- Raw strings: Treats back slaches in strings as literals 
  - `r"C:\User\test"`

### 2.3 Lists

List are mutable ordered data structures

- Pop: Deletes the last or the specified index of the list. Raises an error if list is empty
  - `["a","b","c","d"].pop()` # Pops last list's item
  - `["a","b","c","d"].pop(2)` # Pops item at index 2

- Remove: Delete the given object of the list if present. Raises an error is object is not found.
   - `["a","b","c","d"].remove("c")` # Removes the object "c"

### 2.4 Tuples

Tuples are inmutable ordered data structures. Tuples are like lists but it cannot be updated at a particular index or subsequence of the tuple

### 2.5 Dictionaries

Mutable object. Python's object that consists of key-value pairs. It is python's implementation of what is know as an associative array. Dictionaries only allow hashable items as keys.

- Set default: Sets the default value of a given key 
  ```
  d = {}
  d.setdefault("node", []).append("item")
  ```

- Pop: Pops the specified key and print the return value r_value

  - `d.pop(key, r_value)`

- Popitem: Pops the last item of the dict in a LIFO way
- Update: Updates d1 with the items from d2. Existing items get replaced, new items are added
  ```
  d1={1:"one", 2:"five"}
  d2={2:"two", 3:"three"}
  d1.update(d2)
  ```
### 2.6 Sets

Inmnutable unordered collection of unique items. Sets are not subscriptable. Basic uses include membership testing and eliminating duplicate entries. Set objects also support mathematical operations like union, intersection, difference, and symmetric difference.

- Pop: Pops the items in a FIFO way.
- Discard and Remove: Removes item of the set. Removes rises and error if the item does no exitst in the set, dicard does not.
- Operation between sets: 
  ```
  set1  = {1,2,3,4}
  set2 = {3,4,5,6}
  set1 & set2 # Intersection {3, 4}
  set1 | set2 # Union {1, 2, 3, 4, 5, 6}
  set1 - set2 # Difference {1, 2}
  set1 ^ set2 # Simetric Difference {1, 2, 5, 6}
  ```

## 3. Classes, Modules, Packaging

### 3.1 Scopes

Defines the scope of a variable in Python, usually whether the variable is global or local. Python uses the LEGB (Local, Enclosing, Global, Built-in) rule for variable name resolution.

- Local: Variables defined inside a function
- Enclosing: Variables defined inside a closure function and used by the inner function
- Global: Variables defined in the python script not nested inside any function. object, etc.
- Built-in: Variables that come as Python's built-in.

Some rules for working with global and local variables:
- All variables declared inside functions are local variables
- Local variables cannot be accesed from outside of the context from which were created
- Functions can read and use global variables but the content of the variable outside that function remains unchanged. Except for mutable data structures which their values can be changed directly if modified inside the function.
- If it is need that the function modifies the variable from the global scope, we need to declare the variable as **global** inside the function.

### 3.2 Lambda

Lambda functions are anonymous functions, lambda can receive several arguments but only can eval one expression. Lamdba functions are useful when you want to perform regular tasks and don´t want to define a separate function with the def keyword.

- Syntax: Lambda gets arguments and expression in the form args:expression. Arguments can be set to have default values
  - `lambda x, y, z=1 : x + y + z`

It can be used with other functions such as map and filter

  - `map(lambda x, y, z=1 : x + y + z, [x_iterable],[y_iterable],[z_iterable])`
  - `filter(lambda x, y, z=1 : x + y + z > 8, [x_iterable],[y_iterable],[z_iterable])`

### 3.3 Class

It allows to encapsulate data and methods into a single data structure. The class is most of like blueprint that is used to create objects.

An example of classes implementing class variables (both puclib and private*), class methods, and inheritance is given below
```
def class_function (self):
        print("I am a class fuction")

class TestClassParent:
    class_variable_parent = 4
    __hidden_class_variable_parent = 10
    def __init__(self, *args, **kwargs):
      pass


class TestClass(TestClassParent):
    class_variable = 0
    __hidden_class_variable = 2
    f = class_function
    def __init__(self):
      super().__init__()
      TestClassParent.__init__(self)


class_object = TestClass()

print(class_object.class_variable) # 0
class_object.f() # I am a class fuction
print(class_object._TestClass__hidden_class_variable) # 2
print(class_object.class_variable_parent) #4
print(class_object._TestClassParent__hidden_class_variable_parent) #10
```

Python classes can implement something referred to as dunder or magic methods. These methods start and end with __ (double underscore). They are use to define the behaviour of the object when in presence of certain operations or function calls.For example:
 
- object2 + object2, the method **\__add__()** from the object is called. 
- print(object),  the **\__str__()** method from the object is called
- len(object),  the **\__len__()** from the object is called
- object[x], the **\__getitem__()** from the object is called
- object[x] = 10, the **\__setitem__()** from the object is called

An example of using magic methods is shown belos

```
class Vector:
    def __init__(self, a, b):
        self.a = a
        self.b = b

    def __str__(self):
        return f'Vector ({self.a}, {self.b})'

    def __add__(self,other):
        return Vector(self.a + other.a, self.b + other.b)

v1 = Vector(2,10)
v2 = Vector(5,-2)
print(v1 + v2) # print calls __str__ method implicitly to convert Vector object to string

------
Output:
Vector (7, 8)
```

### 3.4 Modules

These are python script files that contain definitions of variables, functions, classes and other Python objects that are bundled together and can be imported or use by other scripts. 

```
import module as
import module as customed_module
from module import variable, function
from module import variable as v, function as f
```

The search order when importing a module into a script is a follows:

1. Search locally where the script was runned, or if python interpreter is launch in the interactive mode, search in the current directory
2. Search in the list of directories contained in the PYTHONPATH environment variable, if it is set. 
3. Default path at /usr/lib/python/sys.path

When importing a module, its contents can be scanned with the dir function
```
import sys
dir(sys)
```

### 3.5 Files

Both plain and binary files can be opened, modified and saved with the open() function

There are two ways to work with a file:

1. Using open and close functions

```
file = open(file_path, mode, encoding)
file.read() # Loads the whole file into RAM
file.close() # Closes the file descriptor used to open the file
```

2. Using with keyword. This is the preferred way since it can close automatically the file descriptor associated with the file, once it gets done using

```
with open(file_path, mode, encoding) as file:
  file.read()
```

### 3.6 Packages

When the number of modules increases it becomes hard to manage them separaterly. Packages bundles together a collection of modules. It helps to provide a hierchical structure to access the modules, also it helps to separate the namespace of the the variables, functions, classes, etc defined in each module so they don't overlap with each other.

```
Below the example of a package structure.
└── animals
    ├──handlers
    │  ├── __init__.py
    │  ├── walk.py
    │  └── swim.py
    ├── __init__.py
    ├── crocodile.py
    └── monkey.py
```

In order for python to use a package we need to include the **\__init__.py** file somewhere in the package structure. This file is executed as part of the initialization process for the package. It can contain Python code that initializes package-level attributes, imports submodules, or performs any other setup tasks necessary for the package.


To import modules or its objects from packages we use the following dot notation
```
from animals import crocodile
from animals.monkey import Monkey
from animals.handlers import swim
from animals.handlers.walk import is_walking
```

### 3.6 Packaging

Python uses the **Disutils** module which is a standard python library that provides a framework for python package building and distribution. **Setuptools** module is another popular option that is built on top of Disutils and offers more avanced features and improvements for package building and distribution.

How it works?

1. Have ready the package you want to builld 

    ```
    zoo-example
    ├── animals
    │   ├──handlers
    │   │  ├── __init__.py
    │   │  ├── walk.py
    │   │  └── swim.py
    │   ├── __init__.py
    │   ├── crocodile.py
    │   ├── monkey.py
    │   └── zoo.py
    ├── README.md
    ```

2. Install the tools for package building

- `pip install build wheel twine`

3. Include the setup.py file in the root directory of the package. This file contains package metadata that the building tool will use to configure and build the package

    ```
    from setuptools import setup, find_packages

    setup(
        name="zoo-example",
        packages=find_packages(),
        entry_points={
            "console_scripts": [
                "zoo = animals.zoo:main",
            ],
        },
        install_requires=[
           "termcolor==1.1.0",
        ],
        version="0.1",
        author="Captain Jack",
        author_email="captain_jack@gmail.com",
        description="Example of the test application",
        license="MIT",
    )
    ```

- The entry_points are a type of metadata that can be exposed by packages on installation. They come especially handy when the package would like to provide commands to be run at the terminal.
- Install_requires is the metadata that installs dependencies for the package.

4. Build the package

There are different build formats: sdist, bdist_wheel and bdist_egg. Sdist format builds the package including the python source code.

- `$ python setup.py bdist_egg`
- `$ python setup.py bdist_wheel`
- `$ python setup.py bdist_wheel --universal`
- `$ python setup.py sdist`


5. Upload to PyPI

Need to have account in PyPI

- `$ twine upload /home/user/zoo-example/dist/*`

6. Install/Unistall the package

- If the  package was uploaded to PyPI

  `$ pip install -U package_name` Upgrades the packet if already exists

- If package was downloaded as a file

  `$ pip install /home/user/package_name`

-  Unistall package

    `$ pip uninstall package_name`


## 4. Debugging, Jinja2 templates, Flask

### 4.1 Exceptions

Exceptions are errors that occur during the program execution. When an exeption is raised, the program execution in terminated. There are several types of built-in exceptions provide by Python as show in figure 2.

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/828eb4c0-03bb-4c62-bb83-28e500097d5b"
         alt="Figure 2" width="600" height="300"/>
  <br/>
  <em>Figure 2. Exception Tree</em>
</p>

Exeception handling is the costruct that allows to manage encountered exceptions. When a exception is meet, the program save the execution state, then runs the function or code for exception handling and finally resumes the program execution, in this way the exception is handled.

```
try:
   # You do your operations here;
   ......................
except (Exception1, [Exception2[,... Exception]]):
   # If there is Exception1, then execute this block.
   ......................
else:
   # If there is no exception then execute this block. 
finally:
   # This would always be executed.
```
### 4.2 Debugging in Python

### 4.1 Logging

When logging useful data from the right places, you can not only debug errors easily but also use the data to analyze the performance of the application to plan for scaling or look at usage patterns to plan for marketing. Some logging tools are:

1. Print
    ```
    print("This is a log message")
    ```
2. Syslog
    ```
    from syslog import syslog
    syslog('This is a debug message.')
    ```
    To see the logs we can do:

    `$ tail /var/log/syslog`

3. Logging: Ready to use and powerful module used by most third-party Python libraries
    ```
    import logging

    logging.basicConfig(filename='app.log', filemode='w',
                    format='%(name)s - %(levelname)s - %(message)s')

    logging.debug('This is a debug message')
    logging.info('This is an info message')
    logging.warning('This is a warning message')
    logging.error('This is an error message')
    logging.critical('This is a critical message')
    ```
    
    Complex logging configurations are achieve by providing a configuration file, normally in YAML format.

    ```
    import logging
    import logging.config
    import yaml

    with open('config.yaml', 'r') as f:
      config = yaml.safe_load(f.read())
      logging.config.dictConfig(config)

    logger = logging.getLogger("simpleExample")

    logger.debug('This is a debug message')
    ```

### 4.2 Debugging

Some debuggers for python are:

1. python -m pdb: Interactive CLI Built-in debugger for python, useful when no GUI is avaiable such as in a docker container

    `$ python -m pdb myscript.py`
2. pdb.set_trace(): 

    ```
    import pdb
    a = "aaa"
    pdb.set_trace() #When the program reaches this point, it will enter the pdb mode for debugging
    b = "bbb"
    c = "ccc"
    final = a + b + c
    print(final)
    ```

### 4.2 Jinja2 Templates

Jinja is a templating engine for Python. It is used to generate dynamic content, such as HTML pages or emails, by blending static template files with dynamic data. For it to uses the packages needed are Jinja2 and pyyaml

Sintax:

- {{ }} : Used to replace tags
- {{% %}} : Used for control statements such as if, for, while, etc.
- {{# #}} : Used for comments

```
import yaml
from jinja2 import Template

# Load data
with open('data.yml') as data_file:
    config_data = yaml.load(data_file, Loader=yaml.FullLoader)

# Load template
with open('vhosts.j2') as template_file: 
    template_html = template_file.read()

# Render template
template = Template(template_html) 
vhosts_conf = template.render(config_data)

with open('vhosts.conf', 'w') as vhosts_file:
    vhosts_file.write(vhosts_conf)
```

### 4.3 Python App Architecture

Python is widely used for web developement, specially in the backend. The architecture of a python-based web application is show in figure 3. 

<p align="center">
  <img src="https://github.com/userforpyhon47/epam_intro_cloud_devops/assets/134888524/4901c2cf-e412-43d9-92fa-6ff91e213144"
         alt="Figure 3" width="600" height="300"/>
  <br/>
  <em>Figure 3. Python Web App Architecture</em>
</p>

1. Web Server: It provides static content resources to the clients such as html, css, json, xml and others. Popular options for web servers are:
    - Apache
    - Nginx
2. WSGI: Web Server Gateway Inferface decouples web server and python-based application framework. It provides a standard interface for web server to communicate with application running python code. Popular options for WGSI are:
    - Gunicorn
    - uWGSI
3. Application Framework: A framework implements common issues for a web application (such as template system, ORM (Object Relational Mapping) systems, user authentication, content administration, site map. Popular options are:
    - Django
    - Flask
    - FastAPI 
