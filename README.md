## Practitioner Python Course
check
### Standardized Error Handling
Cleaner way of dealing with errors:
```python
import os
import errno

try:
    with open("file.txt", "r") as file:
        pass
except FileNotFoundError as err:
    print(f"ERNO: {err.errno}; {os.strerror(err.errno)}")
```
> ```cmd
> ERNO: 2; No such file or directory
> ```
* Documentation
	* [os .strerror REF](https://docs.python.org/3/library/os.html)
	* [errno REF](https://docs.python.org/es/3/library/errno.html)
### Name Mangling
[theory](https://www.geeksforgeeks.org/name-mangling-in-python/)
### Hiding module entities 

```python 
from utils import *
```

In order to avoid importing everything from a module when using `*`, at the beginning of a script we should specify the allowed functions, variables, classes, . . . that will be imported

```python 
__all__ = ["var_1", "function_1"]
```

It is also possible to define variables, functions, etc. as pseudo-private using the underscore notation.

`my_module.py`
```python 
_private_var = 10
```

```python 
from my_module.py import *
_private_var += 1
```
>```python
> NameError _private_var is not defined
> ```
### Lambda Functions
Single expression anonymous function. They show their utility when use together with collection functions like filter, map, reduced, sort, . . . 
One powerful use is the `list.sort()` method thanks to the `key` and `reverse` arguments: 
```python
to_be_sort_list = [obj1, obj2, obj3, ...]
to_be_sort_list.sort(key=lambda obj: F(obj), reverse=True/False)
```
or, in case there is an existing method like `str.__len__` or `str.lower`, 
```python
to_be_sort_list = [obj1, obj2, obj3, ...]
to_be_sort_list.sort(key=obj.obj_method, reverse=True/False)
```
Analogously to list method `.sort()` there is the built in method `sorted` 
```python 
sorted_list = sorted(iterable, key, reverse)
```
### Closures 
Nested defined functions can be really useful since they can access nonlocal variables defined in the outer function, 
```python
def outer():
    "Local scope -> a, name and inner"
    a = 25
    name = "python"

    def inner(prefix):
	    "name doesn't belong to inner local scope"
        print(prefix, name)
        
    return inner
```
> ```python
> my_func = outer()
> my_func("Maxi")
> ```
> > \>\> Maxi python

In order for `inner` to access `outer`'s variable scope we must remember that  since `inner` is defined in `outer`'s local space `outer`'s scope can be access using the built-in keyword `nonlocal`
```python 
def counter(start):
    def inc(step):
        nonlocal start
        start += step
        print(start)
    return inc
```

> ```python
> inc_counter = counter(10)
> inc_counter(1)
> ```
> > \>\> 11
> ```python
> inc_counter(1)
> ```
> >\>\> 12
### Conditional Expressions
One-liner pythonic style.
>```python 
CONDITION = False
my_var = "This is" if CONDITION else "awesome."
print("Python is ", my_var)
>```
>>\>\> "Python is awesome."

```python
name = "Maximo"
vowels = ["a", "e", "i", "o", "u"]

for letter in name:
    print(f"{letter} is a {'vowel' if letter in vowels else 'consonant'}.")
```
## Python Random Info

### 1. Unicode encoding (UTF)

Most symbols and icons can be added to strings as

```python
utf_encoding = "Hi my name is Maximo \u2674"
```

### 2. while-else & for-else

This extra clause will execute every time our loops finishes successfully (without reaching a break instruction)

```python
count = 1
while count < 3:
    print(count)
    count += 1
else: 
    print("While loop completed")
```

This might look cumbersome but it real potential comes when we are trying to save some computational power (this applies for both while and for)

```python
important_list = [elem1, elem2, elem3, elem4, ...]
# Look for something in the list
for elem in important_list:
    if elem == elemY:
        print("elemY was found")
        break
else:
    print("elemY not present in important_list")
    # Useful for error raising
```

Here the else clause will only be executed if the break command is NOT reached which can lead to some potential error raising.

### 3. Function Recursion

Recursion is some nice looking syntactic sugar but in terms of computing efficiency and infinite loop issues it must be used rather carefully.

```python
def fibonacci(N):
    """Return Nth element of the Fibonacci series."""
    if n == 1:
        return 1
    elif n == 2:
        return 1
          
    return fibonacci(n-2) + fibonacci(n-1)
    
```

### Python Scopes

1. Conditional and loops do not defined their own scope, therefore
   
   > ```python
   > if 1 < 2:
   >     x = 2
   > 
   > print(f"x is: {x}")
   > ```
   > 
   > > \>> x is 2

2. Functions and Classes DO defined their own scope
   
   > ```python
   > x = 5
   > 
   > def some_fnc(y):
   >     x = y
   > 
   > some_fnc(42)
   > 
   > print(f"x is still {x}")
   > ```
   > 
   > > \>> x is stil 5

3. Global keyword: Access some outer defined variable from some inner scope (class, function, ...)
   
   > ```python
   > x = 5
   > print(f"initial x: {x}")
   > 
   > def some_fnc(y):
   >     global x, a
   >     x = y
   >     a = 23
   > 
   > some_fnc(42)
   > 
   > print(f"x is now {x}")
   > print(f"a global value defined from inside function: {a}")
   > ```
   > 
   > > \>> initial x: 5
   > > 
   > > \>> x is now 42
   > > 
   > > \>> a global value defined from inside function: 23
