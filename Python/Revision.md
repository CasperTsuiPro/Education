- [Python Cheatsheet](#python-cheatsheet)
- [Built-in Functions](#built-in-functions)
- [Shorthand If Else](#shorthand-if-else)
- [List Operations](#list-operations)
  - [List Comprehensions](#list-comprehensions)
- [Dictionary Operations](#dictionary-operations)
  - [Dictionary Comprehensions](#dictionary-comprehensions)
- [String Methods](#string-methods)
- [Function](#function)
  - [Default Return Type is None or Tuple](#default-return-type-is-none-or-tuple)
  - [\*args](#args)
  - [\*\*kwargs](#kwargs)
- [Decorators](#decorators)
- [Itertools](#itertools)
- [Regex](#regex)
  - [Backward and Forward Lookahead and Lookbehind](#backward-and-forward-lookahead-and-lookbehind)
- [Collections](#collections)
- [File Handling](#file-handling)
- [Error Handling](#error-handling)
- [Context Managers](#context-managers)
- [OOP](#oop)
  - [Encapsulation](#encapsulation)
  - [Inheritance](#inheritance)
  - [Polymorphism](#polymorphism)
  - [Abstraction](#abstraction)
- [Datetime](#datetime)
- [Pandas Basics](#pandas-basics)

# Python Cheatsheet

# Built-in Functions

```python
# Common built-ins
abs(-5)                             # 5
all([True, True, False])            # False
any([False, False, True])           # True
bool(1)                             # True
chr(65)                             # 'A'
dict(a=1, b=2)                      # {'a': 1, 'b': 2}
divmod(7, 3)                        # (2, 1)
enumerate(['a', 'b'])               # [(0, 'a'), (1, 'b')]
filter(lambda x: x > 0, [-1, 0, 1]) # [1]
map(lambda x: x*2, [1, 2, 3])       # [2, 4, 6]
max([1, 2, 3])                      # 3
min([1, 2, 3])                      # 1
range(5)                            # [0, 1, 2, 3, 4]
reversed([1, 2, 3])                 # [3, 2, 1]
sorted([3, 1, 2])                   # [1, 2, 3]
sum([1, 2, 3])                      # 6
zip([1, 2], ['a', 'b'])             # [(1, 'a'), (2, 'b')]
```

# Shorthand If Else

```python
x = 10
y = 20
z = x if x > y else y # Assign x to z if x is greater than y, otherwise assign y to z
```

# List Operations

```python
lst = [1, 2, 3]
lst.append(4)       # [1, 2, 3, 4]
lst.extend([5, 6])  # [1, 2, 3, 4, 5, 6]
lst.insert(0, 0)    # [0, 1, 2, 3, 4, 5, 6]
lst.remove(0)       # [1, 2, 3, 4, 5, 6]
lst.pop()           # 6, lst becomes [1, 2, 3, 4, 5]
lst.index(2)        # 1
lst.count(2)        # 1
lst.sort()          # [1, 2, 3, 4, 5]
lst.reverse()       # [5, 4, 3, 2, 1]
```

## List Comprehensions

```python
[ x**2 for x in range(10) ]           # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
[ x for x in range(10) if x % 2 == 0 ] # [0, 2, 4, 6, 8]

# Nested List Comprehensions
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
[ [ row[i] for row in matrix ] for i in range(3) ] # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

# Dictionary Comprehensions
{ x: x**2 for x in range(10) } # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}

# Set Comprehensions
{ x for x in range(10) if x % 2 == 0 } # {0, 2, 4, 6, 8}

# Generator Expressions
( x**2 for x in range(10) ) # <generator object <genexpr> at 0x1006a3040>
list(x**2 for x in range(10)) # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

# Comprehensions with Multiple Iterables (Loop y and then x)
[ (x, y) for x in range(3) for y in range(3) ] # [(0, 0), (0, 1), (0, 2), (1, 0), (1, 1), (1, 2), (2, 0), (2, 1), (2, 2)]

# Comprehensions with Conditional Logic
[ x if x % 2 == 0 else -x for x in range(10) ] # [0, -1, 2, -3, 4, -5, 6, -7, 8, -9]

# Comprehensions with Multiple Conditions
[ (x, y) for x in range(3) for y in range(3) if x != y ] # [(0, 1), (0, 2), (1, 0), (1, 2), (2, 0), (2, 1)]

# Comprehensions with Nested Iterables
matrix = [ [1, 2, 3], [4, 5, 6], [7, 8, 9] ]
[ [ matrix[j][i] for j in range(len(matrix)) ] for i in range(len(matrix[0])) ] # [[1, 4, 7], [2, 5, 8], [3, 6, 9]]

# Comprehensions with Nested Iterables and Conditional Logic
matrix = [ [1, 2, 3], [4, 5, 6], [7, 8, 9] ]
[ [ matrix[j][i] for j in range(len(matrix)) if matrix[j][i] % 2 == 0 ] for i in range(len(matrix[0])) ] # [[2], [4, 6], [8]]
```

# Dictionary Operations

```python
d = {'a': 1, 'b': 2, 'c': 3}
for key in d: print(key) # a b c ⚠️ Default is keys only
print(*d)                # a b c ⚠️ Default is keys only
d.keys()                 # dict_keys(['a', 'b', 'c'])
d.values()               # dict_values([1, 2, 3])
d.items()                # dict_items([('a', 1), ('b', 2), ('c', 3)])
d.get('a')               # 1
d.get('c', 0)            # 3
d.pop('a')               # 1, d becomes {'b': 2, 'c': 3}
d.update({'c': 3})       # d becomes {'b': 2, 'c': 3}
```

## Dictionary Comprehensions

```python
{x: x**2 for x in range(10)}                                   # {0: 0, 1: 1, 2: 4, 3: 9, 4: 16, 5: 25, 6: 36, 7: 49, 8: 64, 9: 81}

# Dictionary Comprehensions with Conditional Logic
{x: 'even' if x % 2 == 0 else 'odd' for x in range(10)}        # {0: 'even', 1: 'odd', 2: 'even', 3: 'odd', 4: 'even', 5: 'odd', 6: 'even', 7: 'odd', 8: 'even', 9: 'odd'}

# Dictionary Comprehensions with Multiple Iterables
{x: y for x in range(3) for y in range(3)}                     # {0: 0, 0: 1, 0: 2, 1: 0, 1: 1, 1: 2, 2: 0, 2: 1, 2: 2}

# Dictionary Comprehensions with Nested Iterables
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
{i: [row[i] for row in matrix] for i in range(len(matrix[0]))} # {0: [1, 4, 7], 1: [2, 5, 8], 2: [3, 6, 9]}

# Dictionary Comprehensions with Nested Iterables and Conditional Logic
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
{i: [row[i] for row in matrix if row[i] % 2 == 0] for i in range(len(matrix[0]))} # {0: [2, 4, 6, 8], 1: [2, 4, 6, 8], 2: [2, 4, 6, 8]}
```

# String Methods

```python
'hello world'.count('l')              # 3
'hello world'.split()                 # ['hello', 'world']
'-'.join(['hello', 'world'])          # 'hello-world'
'hello world'.replace('l', 'L')       # 'heLLo'
'hello world'.sub(r'l', lambda x: x.group().upper()) # 'heLLo world'.  ⚠️ Support regex and function for replacement
'  hello  '.strip()                   # 'hello'
'Hello'.lower()                       # 'hello'
'hello'.upper()                       # 'HELLO'
'hello'.isalpha()                     # False
'123'.isdigit()                       # True
'hello world'.startswith('h')         # True
'hello world'.endswith('o')           # False
'hello world'.find('o')               # 4
'hello world'.index('o')              # 4
'hello world'.rfind('o')              # 7
'hello world'.rindex('o')             # 7
'hello world'.ljust(20, '*')          # 'hello world********'
'hello world'.rjust(20, '*')          # '********hello world'
'hello world'.center(20, '*')         # '****hello world****'
'hello world'.zfill(20)               # '00000000000000000hello world'
```

# Function
⚠️ Always return a value.
## Default Return Type is None or Tuple
```python
def my_function(): return
print(my_function()) # None

def my_function(): return 1, 2, 3
print(my_function()) # (1, 2, 3)
```

## \*args
*args is used to pass a variable number of Positional arguments to a function.
args can be any name, but it is conventional to use args.

```python
def my_function(*args):
    print(args)

my_function(1, 2, 3) # (1, 2, 3)
```

## **kwargs
**kwargs is used to pass a variable number of Keyword arguments to a function.
kwargs can be any name, but it is conventional to use kwargs.

```python
def my_function(**kwargs):
    print(kwargs)

my_function(a=1, b=2, c=3) # {'a': 1, 'b': 2, 'c': 3}
```

# Decorators

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before")
        result = func(*args, **kwargs)
        print("After")
        return result
    return wrapper

@my_decorator
def say_hello():
    print("Hello!")
```

# Itertools

```python
from itertools import *

count(start=1, step=2)                        # 1, 3, 5, 7, 9, ...
cycle('ABC')                                  # A, B, C, A, B, C, ...
repeat('hello', times=3)                      # hello, hello, hello
chain('ABC', 'DEF')                           # A, B, C, D, E, F
accumulate([1, 2, 3, 4, 5])                   # 1, 3, 6, 10, 15
groupby([1, 2, 3, 4, 5], key=lambda x: x % 2) # {0: [2, 4], 1: [1, 3, 5]}
product([1, 2, 3], [4, 5, 6])                 # (1, 4), (1, 5), (1, 6), (2, 4), (2, 5), (2, 6), (3, 4), (3, 5), (3, 6)
permutations([1, 2, 3])                       # (1, 2, 3), (1, 3, 2), (2, 1, 3), (2, 3, 1), (3, 1, 2), (3, 2, 1)
combinations([1, 2, 3], 2)                    # (1, 2), (1, 3), (2, 3)
combinations_with_replacement([1, 2, 3], 2)   # (1, 1), (1, 2), (1, 3), (2, 2), (2, 3), (3, 3)
```

# Regex

```python
from re import *
match(r'hello', 'hello world')   # <re.Match object; span=(0, 5), match='hello'>
search(r'world', 'hello world')  # <re.Match object; span=(6, 11), match='world'>
findall(r'\d', 'a1b2c3')  # ['1', '2', '3']
sub(r'\d', 'X', 'a1b2c3')  # 'aXbXcX'
split(r'\d', 'a1b2c3')  # ['a', 'b', 'c', ''] ⚠️ str.split() does not support regex
```

## Backward and Forward Lookahead and Lookbehind
Remind that lookahead and lookbehind are zero-width assertions.
```python
import re

# Backward Lookahead
# Replace 'world' with 'everyone' only if it is preceded by 'hello'
re.sub(r'(?<=hello )world', 'everyone', 'hello world')  # 'hello everyone'

# Forward Lookahead
# Replace 'hello' with 'hi' only if it is followed by 'world'
re.sub(r'hello(?= world)', 'hi', 'hello world')  # 'hi world'

# Negative Lookahead
# Replace 'world' with 'everyone' only if it is not preceded by 'hello'
re.sub(r'world(?! hello)', 'everyone', 'hello world')  # 'hello everyone'

# Negative Lookbehind
# Replace 'world' with 'everyone' only if it is not preceded by 'hello'
re.sub(r'(?<!hello) world', 'everyone', 'hello world')  # 'hello everyone'
```

# Collections

```python
from collections import namedtuple, Counter, defaultdict, deque, ChainMap

# Namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)  # Point(x=1, y=2)

# Counter
c = Counter('hello')  # Counter({'h': 1, 'e': 1, 'l': 2, 'o': 1})

# Defaultdict
dd = defaultdict(int)
dd['a'] += 1  # defaultdict(<class 'int'>, {'a': 1})

# Deque
d = deque([1, 2, 3])
d.append(4)  # deque([1, 2, 3, 4])
d.appendleft(0)  # deque([0, 1, 2, 3, 4])

# ChainMap
d1 = {'a': 1, 'b': 2}
d2 = {'c': 3, 'd': 4}
ChainMap(d1, d2)  # ChainMap({'a': 1, 'b': 2}, {'c': 3, 'd': 4})
```

# File Handling

```python
# Reading
with open('file.txt', 'r') as f:
    content = f.read()

# Writing
with open('file.txt', 'w') as f:
    f.write('Hello')

# Appending
with open('file.txt', 'a') as f:
    f.write('\nWorld')
```

# Error Handling

```python
try:
    result = 10 / 0
except ZeroDivisionError:
    print("Cannot divide by zero")
except Exception as e:
    print(f"Error: {e}")
finally:
    print("Cleanup")
```

# Context Managers

```python
class MyContext:
    def __enter__(self):
        print("Enter")
        return self
    def __exit__(self, exc_type, exc_val, exc_tb):
        print("Exit")

with MyContext():
    print("Inside")
```

# OOP

```python
class Animal:
    def __init__(self, name):
        self.name = name
  
    def speak(self):
        return "Sound"

class Dog(Animal):
    def speak(self):
        return "Woof!"

# Abstract Base Class
from abc import ABC, abstractmethod
class Shape(ABC):
    @abstractmethod
    def area(self):
        pass
```

## Encapsulation

```python
class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.__balance = balance  # Private attribute

    def deposit(self, amount):
        self.__balance += amount

    def withdraw(self, amount):
        if amount > self.__balance:
            print("Insufficient funds")
        else:
            self.__balance -= amount
          
    def get_balance(self):
        return self.__balance
```

## Inheritance

```python
class SavingsAccount(BankAccount):
    def __init__(self, owner, balance=0, interest_rate=0.01):
        super().__init__(owner, balance)
        self.interest_rate = interest_rate
```

## Polymorphism

```python
class Animal:
    def speak(self):
        return "Sound"

class Dog(Animal):
    def speak(self):
        return "Woof!"

dog = Dog()
print(dog.speak())  # Output: Woof!

animals = [Dog(), Cat()]
for animal in animals:
    print(animal.speak())  # Output: Woof! and Meow!
```

## Abstraction

```python
class Animal:
    def speak(self):
        pass

class Dog(Animal):
    def speak(self):
        return "Woof!"  

class Cat(Animal):
    def speak(self):
        return "Meow!"

dog = Dog()
cat = Cat()

print(dog.speak())  # Output: Woof!
print(cat.speak())  # Output: Meow!
```

# Datetime

```python
from datetime import datetime, timedelta

now = datetime.now()
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
tomorrow = now + timedelta(days=1)
```

# Pandas Basics

```python
import pandas as pd

# Create DataFrame
df = pd.DataFrame({'Name': ['John', 'Anna'], 'Age': [28, 24]})

# Read CSV
df = pd.read_csv('file.csv')

# Filter
adults = df[df['Age'] > 25]

# Group
grouped = df.groupby('Name').mean()
```
