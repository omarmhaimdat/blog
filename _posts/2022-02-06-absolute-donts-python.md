---
title: "Why Every Python Developer Needs to Read This"
categories:
  - Blog
  - Tutorials
tags:
  - python
  - data science
  - tutorial
---

Python's syntax is very permissive, so it's very easy to write bad code with Python. To standardize code writing, the Python developer community recommends a number of rules so that code is readable, readable by you but also by others. We will see in these article how you can up you code with simple code snippets of **don't** and **dos**.

Try re-reading some code you wrote “quickly” a month, six months, or a year ago. If the code is just a few lines long, you might find your way around, but if it's tens or even hundreds of lines long, it might be different. The creator of Python, Guido van Rossum, puts it simply:

> "Code is read much more often than it is written"

The code is more often read than written, see, the computer can understand almost anything, but you have to remember that code is written and read by humans. Let's see how you can improve your Python code with simple and "Pythonic" practices. Several things are necessary to write a readable code: the syntax, the organization of the code, and the understanding of the language specificity.

## String concatenation

Coming from languages such as Java or C/C++, developers tend to use and apply the same logic without taking into account that each language has it's "best" way of doing things, take this example:

### Don't

```python
text_a = "Python is "
text_b = "awesome"

text_c = text_a + text_b
```

### Better

```python
text_a = "Python is "
text_b = "awesome"

text_c = "{text_a}{text_b}".format(text_a=text_a, text_b=text_b)
```

### Best

```python
text_a = "Python is "
text_b = "awesome"

text_c = f"{text_a}{text_b}"
```

<div class="notice">
  {{ notice-2 | markdownify }}
  Python is awesome
</div>

**F-String** are by far the best and fastest way to concatenate two or multiple string, because there is no operation (+) involved in the process. *str.format()* is better than the first method but still too wordy if you have multiple strings. Furthermore, with *str.format()* you can't insert an expression or call a function while concatenating the string, example:

```python
text_a = "Python is "
text_b = "awesome"

text_c = f"{text_a}{text_b.upper()}"
```

## List comprehensions

A List Comprehension in Python is a mechanism introduced in version 2.7 and present in all subsequent versions. Its purpose is to quickly generate a list from an iterable object. This is useful when you want to filter a list or perform an operation on a list. Take this example:

### Don't

```python
list_a = [9, 4, 5, 12456, 655, 2, 4, 5]
list_b = []

for element in list_a:
    if element < 10:
        list_b.append(element)
print(list_b)
```

<div class="notice">
  {{ notice-2 | markdownify }}
  [9, 4, 5, 2, 4, 5]
</div>

So you might ask what's wrong with this code...nothing, but imagine you have a very long list and you want to optimize it, you need to thing in terms of operations, here the most important one is the *append* one, list comprehensions are here for you:

### Best

```python
list_a = [9, 4, 5, 12456, 655, 2, 4, 5]
list_b = [element for element in list_a if element < 10]
print(list_b)
```

<div class="notice">
  {{ notice-2 | markdownify }}
  [9, 4, 5, 2, 4, 5]
</div>

For one, it's shorter and most importantly it's **faster**, again sometimes if the logic is too complex you may want to still use the first technique because it can be more readable, but in most cases a list comprehension will do the trick.

## Dictionary key access

A dictionary is an unordered, modifiable and indexed collection. In Python, dictionaries are written with braces, and they have keys and values.

Take this dictionary for example:

```python
person = {"first_name": "Omar", "age": 30}
```

Let's say we want to access and do a condition on the age, most common way would be to access the key `age` and perform a condition on it like so:

### Don't

You can access items in a dictionary by referring to its key name, enclosed in square brackets and test on that condition:

```python
person = {"first_name": "Omar", "age": 30}
if person["age"] > 20:
    print("Getting old...")
```

<div class="notice">
  {{ notice-2 | markdownify }}
  Getting old...
</div>

### Best

There is also a method called get() which will give you the same result:

```python
person = {"first_name": "Omar", "age": 30}
if person.get("age") > 20:
    print("Getting old...")
```

The second method is better for multiple reasons, let's say the key `age` is not present in the dictionary, the first method will raise a `KeyError` exception, but the second one will not, because the method `get()` has a default value of `None` if the key doesn't exist. Let's see if we can improve both code examples and compare:

* **Method 1**:

Check if the key is in the list of keys, then perform the comparison

```python
person = {"first_name": "Omar", "age": 30}
if "age" in person.keys() and person["age"] > 20:
    print("Getting old...")
```

* **Method 2**:

Access the key, if the key doesn't exist, the value will be **-1** (default value) and then perform the comparison

```python
person = {"first_name": "Omar", "age": 30}
if person.get("age", -1) > 20:
    print("Getting old...")
```

## Reading and writing files

One way to store data permanently is to store it in files. To edit a file in python we use the `open` function. This function takes as first parameter the path of the file (relative or absolute) and as second parameter the [type of opening](https://www.programiz.com/python-programming/file-operation):

### Don't

```python
my_file = open('file.txt', mode='r').read()
```

This will give you the content of the file without any issue, but the problem is that we never closed the file, see Python will probably close it for you, but you are losing control in you program and the garbage collector might sweep any change you made or sometimes the changes will only take into effect if the file is closed. The best way to avoid these issues is to use a context manager that will make sure the file is closed when all your operation are finished:

### Best

```python
with open('file.txt', mode='r', encoding='utf8') as my_file:
    ## Do whatever you need to do
```

## Inconsistent return type

For someone accustomed to Java, Swift or C/C++, the function return type is checked at compile time, so you don't have to worry about it, but with Python it's another story, you can't return any type even though it's inconsistent with the function purpose, example:

## Don't

```python
def get_max_min(my_list):
    if my_list:
        return max(my_list), min(my_list)
    return None
```

If the argument `my_list` is not `None` then return a `Tuple` of maximum and minimum value in the `List`, `None` otherwise. The problem with this function is the inconsistent return type, so you either always return a `Tuple` or you raise an exception if the `List` is `None`, or you can return a `Tuple` of `None` like so:

## Better

```python
def get_max_min(my_list):
    if my_list:
        return max(my_list), min(my_list)
    return None, None
```

You can also improve this function by adding type hints:

## Best

```python
from typing import List, Optional, Tuple

def get_max_min(my_list: List[int]) -> Optional[Tuple[int, int]]:
    if my_list:
        return max(my_list), min(my_list)
    return None, None
```

