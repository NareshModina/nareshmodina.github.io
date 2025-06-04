---
title: Python programming roadmap
date: 2025-06-03 16:00:00 +/-TTTT0200
categories: [Programming]
tags: [python]     # TAG names should always be lowercase
author: naresh
---


# Python Learning Roadmap

## Basic

### 1. Variables
Variables are containers for storing data values.
```python
x = 5
name = "Alice"
```

### 2. Basic Data Structures
Includes lists, tuples, sets, and dictionaries.
```python
my_list = [1, 2, 3]
my_tuple = (1, 2)
my_set = {1, 2, 3}
my_dict = {"key": "value"}
```

### 3. Conditioning (Boolean)
Use boolean expressions to control logic.
```python
if x > 0:
    print("Positive")
```

### 4. Chained Conditioning
Multiple conditions combined.
```python
if x > 0 and x < 10:
    print("Between 0 and 10")
```

### 5. Operators: `+`, `*`, `%`, `/`
Used for arithmetic operations.
```python
sum = 3 + 2
product = 4 * 2
remainder = 10 % 3
division = 8 / 2
```

### 6. Control Flow (if/else)
Directs the execution of code.
```python
if x > 10:
    print("Big")
else:
    print("Small")
```

### 7. Loops
Iterate through sequences.
```python
for item in my_list:
    print(item)

while x < 5:
    x += 1
```

### 8. Functions
Reusable code blocks.
```python
def greet(name):
    return f"Hello, {name}"
```

### 9. Mutable and Immutable Data Types
Mutable (can change): list, dict. Immutable: int, str, tuple.
```python
immutable = "hello"
mutable = [1, 2]
```

### 10. Common Methods
```python
my_list.append(4)
print("hello".upper())
print("HELLO".lower())
```

### 11. I/O Operations
```python
with open("file.txt", "w") as f:
    f.write("Hello World")

with open("file.txt", "r") as f:
    content = f.read()
```

## Intermediate

### 1. OOP: Classes and Inheritance
```python
class Animal:
    def speak(self):
        return "Sound"

class Dog(Animal):
    def speak(self):
        return "Bark"
```

### 2. Exceptions
```python
try:
    1 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
```

### 3. Data Structures
Advanced ones: heapq, queue, etc.
```python
import heapq
heap = []
heapq.heappush(heap, 10)
```

### 4. Comprehensions
```python
squares = [x*x for x in range(5)]
```

### 5. Lambda Functions
```python
add = lambda x, y: x + y
```

### 6. Collections
Special containers.
```python
from collections import Counter
count = Counter("banana")
```

### 7. *args and **kwargs
```python
def func(*args, **kwargs):
    print(args, kwargs)
```

### 8. Dunder Methods
```python
class MyClass:
    def __str__(self):
        return "MyClass instance"
```

### 9. pip
Package installer for Python.
```bash
pip install requests
```

### 10. Environments
Used for isolated Python environments.
```bash
python -m venv myenv
source myenv/bin/activate
```

### 11. Code Modules
Reuse code via modules.
```python
import math
print(math.sqrt(16))
```

### 12. Asyncio
Handle async tasks.
```python
import asyncio

async def say_hello():
    print("Hello")
```

## Advanced

### 1. Decorators
Modify function behavior.
```python
def decorator(func):
    def wrapper():
        print("Before")
        func()
        print("After")
    return wrapper
```

### 2. Generators
Yield items lazily.
```python
def count_up_to(n):
    i = 0
    while i < n:
        yield i
        i += 1
```

### 3. Context Managers
Manage resources.
```python
with open("file.txt") as f:
    data = f.read()
```

### 4. Metaclasses
Class of a class.
```python
class Meta(type):
    pass
```

### 5. Concurrency / Parallelization
```python
import threading
t = threading.Thread(target=print, args=("Hello",))
t.start()
```

### 6. Testing
```python
import unittest

class Test(unittest.TestCase):
    def test_add(self):
        self.assertEqual(1 + 1, 2)
```

### 7. Build and Manipulate Packages
Use `setup.py` for packaging.

### 8. Cython
C extensions for Python for speed.

## Expert

### 1. Low-Level Concepts
Includes how Python is interpreted (CPython), memory model.

### 2. Specialized Packages
#### TensorFlow
Deep learning framework.
```python
import tensorflow as tf
```

#### PyTorch
Another DL framework.
```python
import torch
```

#### Django
Web development framework.
```bash
django-admin startproject mysite
```

Part of the content of the post is produced by AI
