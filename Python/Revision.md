- [Python Cheatsheet](#python-cheatsheet)
  - [Built-in Functions](#built-in-functions)
  - [List Operations](#list-operations)
  - [Dictionary Operations](#dictionary-operations)
  - [String Methods](#string-methods)
  - [Regex Basics](#regex-basics)
  - [Collections Module](#collections-module)
  - [File Handling](#file-handling)
  - [Error Handling](#error-handling)
  - [Context Managers](#context-managers)
  - [Decorators](#decorators)
  - [OOP Basics](#oop-basics)
    - [Encapsulation](#encapsulation)
    - [Inheritance](#inheritance)
    - [Polymorphism](#polymorphism)
    - [Abstraction](#abstraction)
  - [Datetime](#datetime)
  - [Pandas Basics](#pandas-basics)

# Python Cheatsheet

## Built-in Functions
```python
# Common built-ins
abs(-5)  # 5
all([True, True, False])  # False
any([False, False, True])  # True
bool(1)  # True
chr(65)  # 'A'
dict(a=1, b=2)  # {'a': 1, 'b': 2}
divmod(7, 3)  # (2, 1)
enumerate(['a', 'b'])  # [(0, 'a'), (1, 'b')]
filter(lambda x: x > 0, [-1, 0, 1])  # [1]
map(lambda x: x*2, [1, 2, 3])  # [2, 4, 6]
max([1, 2, 3])  # 3
min([1, 2, 3])  # 1
range(5)  # [0, 1, 2, 3, 4]
reversed([1, 2, 3])  # [3, 2, 1]
sorted([3, 1, 2])  # [1, 2, 3]
sum([1, 2, 3])  # 6
zip([1, 2], ['a', 'b'])  # [(1, 'a'), (2, 'b')]
```

## List Operations
```python
lst = [1, 2, 3]
lst.append(4)  # [1, 2, 3, 4]
lst.extend([5, 6])  # [1, 2, 3, 4, 5, 6]
lst.insert(0, 0)  # [0, 1, 2, 3, 4, 5, 6]
lst.remove(0)  # [1, 2, 3, 4, 5, 6]
lst.pop()  # 6, lst becomes [1, 2, 3, 4, 5]
lst.index(2)  # 1
lst.count(2)  # 1
lst.sort()  # [1, 2, 3, 4, 5]
lst.reverse()  # [5, 4, 3, 2, 1]
```

## Dictionary Operations
```python
d = {'a': 1, 'b': 2}
d.keys()  # dict_keys(['a', 'b'])
d.values()  # dict_values([1, 2])
d.items()  # dict_items([('a', 1), ('b', 2)])
d.get('a')  # 1
d.get('c', 0)  # 0
d.pop('a')  # 1, d becomes {'b': 2}
d.update({'c': 3})  # d becomes {'b': 2, 'c': 3}
```

## String Methods
```python
s = 'hello world'
s.split()  # ['hello', 'world']
'-'.join(['hello', 'world'])  # 'hello-world'
s.replace('l', 'L')  # 'heLLo'
'  hello  '.strip()  # 'hello'
'Hello'.lower()  # 'hello'
'hello'.upper()  # 'HELLO'
'hello'.isalpha()  # True
'123'.isdigit()  # True
```

## Regex Basics
```python
import re
re.match(r'hello', 'hello world')  # match object
re.search(r'world', 'hello world')  # match object
re.findall(r'\d', 'a1b2c3')  # ['1', '2', '3']
re.sub(r'\d', 'X', 'a1b2c3')  # 'aXbXcX'
re.split(r'\d', 'a1b2c3')  # ['a', 'b', 'c', '']
```

## Collections Module
```python
from collections import namedtuple, Counter, defaultdict

Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)  # Point(x=1, y=2)

c = Counter('hello')  # Counter({'h': 1, 'e': 1, 'l': 2, 'o': 1})

dd = defaultdict(int)
dd['a'] += 1  # defaultdict(<class 'int'>, {'a': 1})
```

## File Handling
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

## Error Handling
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

## Context Managers
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

## Decorators
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

## OOP Basics
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
### Encapsulation
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

### Inheritance
```python
class SavingsAccount(BankAccount):
    def __init__(self, owner, balance=0, interest_rate=0.01):
        super().__init__(owner, balance)
        self.interest_rate = interest_rate
```

### Polymorphism
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

### Abstraction
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

## Datetime
```python
from datetime import datetime, timedelta

now = datetime.now()
formatted = now.strftime("%Y-%m-%d %H:%M:%S")
tomorrow = now + timedelta(days=1)
```

## Pandas Basics
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
