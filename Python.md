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

- Pop: Deletes the last or the specified index of the array. Raises an error it list is empty
  - `["a","b","c","d"].pop()` # Pops last list's item
  - `["a","b","c","d"].pop(2)` # Pops item at index 2

- Remove: Delete the given object of the list if present. Raise an error is object is not found.
   - `["a","b","c","d"].remove("c")` # Removes the object "c"

### 2.4 Tuples

Tuples are inmutalbe ordered data structures

### 2.5 Dictionaries
