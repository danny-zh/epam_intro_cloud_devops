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

### 3.1 Classes


