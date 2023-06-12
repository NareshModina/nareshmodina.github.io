---
title: Python object oriented programming - classes
date: 2023-06-10 17:00:00 +/-TTTT0200
categories: [Python Classes]
tags: [python classes, methods, oop]     # TAG names should always be lowercase
author: naresh
---

Classes are usefull in logically grouping data (attributes) and functions (methods) that are easy to build up on and reuse.

1. Defining a class:
    ``` python 
    class Team_member:
        pass
    ```
A class can not be empty, use 'pass' to skip it, otherwise it produces an error.

Instances of a class:

``` python
member_1 = Team_member()
member_2 = Team_member()
```

member_1, member_2 are instance variables and they are unique.

You can directly assign attributes to an instance as follows:

``` Python
member_1.name = "your name"
member_1.role = "researcher"
member_1.id = "xyz"

member_2.name = "ur name"
member_2.role = "engineer"
member_2.id = "zyx"
```
Both the instances have unique attributes.

This is a manual process of assigning attributes and it is not useful as one has to write many lines of code and it can be prone to errors(typos). 

A smarter way to instantiate a class is the following:

``` python
class Team_member:
    def __init__(self, name, role, domain):
        self.name = name
        self.role = role
        self.domain = domain
        self.email = first + '.' + last + '@outlook.com'
```
In this the init method (function inside a class) can be seen as an initialization function which by default takes 'self' as an instance (it can be named any other, but self is a convention). Then an instance can be generated as follows:

``` python
 member_1 = Team_member('your name', 'researcher', 'computer science')
 member_1 = Team_member('ur name', 'engineer', 'electronics')
```

Here, member_1 is sent as an instance automatically through self.

It is often useful to define methods: in the following example email(self) is a method where it takes self as an instance.

``` python
class Team_member:
    def __init__(self, name, role, domain):
        self.name = name
        self.role = role
        self.domain = domain
    def email(self)
        return self.name + '@outlook.com'
member_1 = Team_member('your name', 'researcher', 'computer science')
 member_1 = Team_member('ur name', 'engineer', 'electronics')
```
After that it can easily used to generate the email for as many users as possible. This can be done in two ways:

``` python
member_1.email()
Team_member.email(member_2)
```

Both return the email for the corresponding isntance but the syntax is slightly different.