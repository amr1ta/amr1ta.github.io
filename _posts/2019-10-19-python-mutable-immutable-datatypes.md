---
title: "Mutable and Immutable data types in Python"
layout: post
excerpt: "What are Mutable and Immutable data types in Python "
tags:
  - python
---

Every variable in python holds an instance of an object. There are two types of objects in python i.e. **Mutable and Immutable objects**. Whenever an object is instantiated, it is assigned a unique object id.

We can use the **id()** function to validate mutability. This function returns an integer which is guaranteed to be unique and constant for an object during its lifetime. This is calculated from the object's memory address.

**Immutable object** can’t be changed after it is created.

e.g: string, tuple, int, float

Example:

```
n = 13
id(n) = 87325325
n = 31
```
You may think you've changed the object's value from 13 to 31, but you haven't. All you've done is made n refer to a new object. You can confirm this by calling the id() function again.

```
id(n) = 82326421
```
You can compare the identities using the **is** operator. It is shorthand for **id(x) == id(y).**
```
x = 12
y = 12

id(x) = 1452653
id(y) = 3546534

>>> x == y  # x and y have same value ?
True

>>> id(x) == id(y)  # x and y are same objects ?
False

>>> x is y
False
```


**Mutable objects** can be changed after it is created.

e.g: list, dictionary, set
```
c = ["red", "blue", "green"]

id(c) = 6643318

c.append("yellow")
```
Although we have modified the list by appending a new value to it, we can verify that the original list object was updated by using the id() function.
```
c = ["red", "blue", "green", "yellow"]

id(c) = 6643318  # the identity value is same as original object
```

