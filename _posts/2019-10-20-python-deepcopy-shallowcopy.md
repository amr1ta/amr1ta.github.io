---
title: "Deep Copy vs Shallow copy in Python"
layout: post
excerpt: "What is Deep Copy and Shallow copy in Python "
tags:
  - python
---

**Prerequisite Knowledge**: [Mutable and Immutable datatypes]({% post_url 2019-10-19-python-mutable-immutable-datatypes %})

List Assignment 
```
>>> a = [1,2,3,4,5] 

>>> b = a 

>>> print(a) 
 [1,2,3,4,5]

>>> print( id(a) )
 23567790

>>> print(b, id(b)) 
 [1,2,3,4,5]  23567790

>>> print(a is b) # a and b refers to same object
 True
 ```
Note that id values are same, meaning both a and b point to same memory location. Therefore if we modify a (change a value or add a value), it should be reflected in b and vice versa. 
```
>>> a[1] = 20 

>>> b.append(6) 

>>> print(a, id(a))
 [1,20,3,4,5,6]  23567790

>>> print(b, id(b)) 
 [1,20,3,4,5,6]  23567790
 ```
So what to do if we wanted to create a totally independent copy of a list? Meaning it should have separate memory locations. For this we have a few options:

* Use the builtin **list.copy()** method (available since Python 3.3):  
```>>> new_list = old_list.copy()```  
* Use the built in **list()** function:  
```>>> new_list = list(old_list)``` 
* Use shallowcopy or deepcopy based on requirement.  
  We have to import the copy module for shallow and deep copy operations. 

**Deepcopy**:

Deepcopy creates independent copy of original object and all its nested objects.
```
>>> import copy 
>>> a = [1,2,3,4,5] 

>>> b = copy.deepcopy(a) 

>>> print(a, id(a)) 
 [1,2,3,4,5]  23567790

>>> print(b, id(b))
 [1,2,3,4,5]  45466789

>>> print (a is b) # a and b are two different objects
 False

>>> a[1]=20 

>>> b.append(6) 

>>> print(a, id(a)) 
 [1,20,3,4,5]   23567790

>>> print(b, id(b))
 [1,2,3,4,5,6]  45466789  
 ```
Notice that id values of **a** and **b** are different, meaning they are in different memory locations. So changing one does not affect the other. 

Now what if the list contained nested lists? Let's have a closer look at the values of **a** and **b**. 
```
>>> import copy 

>>> a = [[1,2], [3,4], 5] 

>>> b = copy.deepcopy(a)

>>> print(a[0], id(a[0])) 
 [1,2]  68787054

>>> print(b[0], id(b[0]))
 [1,2]   98976545

>>> print(a[0] is b[0]) # the inner lists are also different objects
False

>>> print(a[2], b[2], a[2] is b[2]) # immutable data types occupy same memory location
 5  5  True
 ```
Notice that on deep-copying even the nested lists get new memory locations. So changing one will not affect the other. However the integer value at **a[2] (i.e 5)** and **b[2]** are same and occupy same memory location. 

So does this mean if we modify **a[2], b[2]** will also be modified? 
```
>>> a[2] = 6 

>>> print(a[2], b[2], a[2] is b[2])

 6  5  False
 ```
 Absolutely not. Why? Recall that ints, strings, tuples are immutable. So once you changed **a[2]** value, it got moved to a different memory location but **b[2]** keeps referencing the old object.

 

**Shallowcopy** :

A shallow copy creates a new object which stores the reference of the original elements. The only difference between shallowcopy and deepcopy is that upon shallow copying, nested lists share the same memory location. 
```
>>> import copy 

>>> a = [[1,2], [3,4], 5] 

>>> b = copy.copy(a) # shallow copy

>>> print(a, id(a))
[[1,2], [3,4], 5]  27373873

>>> print(b, id(b))
[[1,2], [3,4], 5]  87424218 

>>> print (a is b) # a and b are different objects
False

>>> print(a[0], id(a[0]))
[1,2]  23578382

>>> print(b[0], id(b[0]))
[1,2]  23578382

>>> print(a[0] is b[0]) # inner lists share same memory location
True
```
So although **a** and **b** themselves occupy different memory locations, their nested lists still reference to the same location. So naturally if you modify the nested lists in **a**, it will be reflected in **b** and vice versa. 
```
>>> import copy 

>>> a = [[1,2], [3,4], 5] 

>>> b = copy.copy(a) # shallow copy

>>> print(a, id(a))
[[1,2], [3,4], 5]  27373873 

>>> print(b, id(b)) 
[[1,2], [3,4], 5]  87424218 

>>> print (a is b) # a and b are different objects
False

>>> a[0][1] = 20 

>>> b[1].append(100) 

>>> a.append(6) 

>>> print(a, id(a))
[[1,20], [3,4,100], 5, 6]  27373873 

>>> print(b, id(b))
[[1,20], [3,4,100], 5]  87424218
```
 Note that only modifications of nested lists are mirrored across both **a** and **b**, not the other elements because the other element is int type, immutable type. So when we will modify that it will naturally create a new object.

Therefore to summarize,

* Making a shallow copy of an object won’t clone child objects. Therefore, the copy is not fully independent of the original.
* A deep copy of an object will recursively clone child objects. The clone is fully independent of the original, but creating a deep copy is slower.


References:

<https://stackoverflow.com/questions/2612802/how-to-clone-or-copy-a-list>
<https://realpython.com/copying-python-objects/>

