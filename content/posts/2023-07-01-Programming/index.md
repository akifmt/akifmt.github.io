---
title: "Programming"
date: 2023-07-01T00:00:00+00:00
hero: blog18_Programming.jpg
description: Programming
menu:
  sidebar:
    name: Programming
    identifier: programming
    weight: -20230701
tags: [Programming, OOP]
categories: [Programming, OOP]

author:
  name: Akif T.
---

<p style="text-align: center;">
<img src="blog18_Programming.jpg" alt="programming" title="programming"><br>
<p>

##### **Programming**

A programming language is a formal computer language designed to communicate instructions to a machine, particularly a computer. Programming languages can be used to create programs to control the behavior of a machine or to express algorithms. (Source: wikipedia)

###### **Object Oriented Programming(OOP)**

Object Oriented Programming(OOP) is a programming paradigm based on the concept of objects, which may contain data, in the form of fields, often known as attributes; and code, in the form of procedures, often known as methods.

1. Encapsulation
2. Abstraction
3. Inheritance
4. Polymorphism

There are five principles when design a class.

- SRP (The Single Responsibility Principle) A class should have one, and only one, reason to change.
- OCP (The Open Closed Principle) Should be able to extend any classes' behaviors, without modifying the classes..
- LSP (The Liskov Substitution Principle) Derived classes must be substitutable for their base classes.
- ISP (The Interface Segregation Principle) Make fine grained interfaces that are client specific.
- DIP (The Dependency Inversion Principle) Depend on abstractions, not on concretions.

###### **What is Encapsulation (or Information Hiding)?**

The encapsulation is the inclusion-within a program object-of all the resources needed for the object to function, basically, the methods and the data. In OOP the encapsulation is mainly achieved by creating classes, the classes expose public methods and properties. A class is kind of a container or capsule or a cell, which encapsulate a set of methods, attribute and properties to provide its indented functionalities to other classes. In that sense, encapsulation also allows a class to change its internal implementation without hurting the overall functioning of the system. That idea of encapsulation is to hide how a class does its business, while allowing other classes to make requests of it.

In order to modularize/ define the functionality of a one class, that class can uses functions or properties exposed by another class in many different ways. According to Object Oriented Programming there are several techniques classes can use to link with each other. Those techniques are named association, aggregation, and composition.

###### **Association**
A relationship where all objects have their own lifecycle and there is no owner.

###### **Aggregation**
A specialised form of Association where all objects have their own lifecycle, but there is ownership and child objects can not belong to another parent object.

###### **Composition**
A specialised form of Aggregation and we can call this as a “death” relationship. It is a strong type of Aggregation. Child object does not have its lifecycle and if parent object is deleted, all child objects will also be deleted.

###### **Abstraction**
Abstraction is an emphasis on the idea, qualities and properties rather than the particulars (a suppression of detail). The importance of abstraction is derived from its ability to hide irrelevant details and from the use of names to reference objects. Abstraction is essential in the construction of programs. It places the emphasis on what an object is or does rather than how it is represented or how it works. Thus, it is the primary means of managing complexity in large programs.

###### **Generalization**
Generalization is the broadening of application to encompass a larger domain of objects of the same or different type. Programming languages provide generalization through variables, parameterization, generics and polymorphism. It places the emphasis on the similarities between objects. Thus, it helps to manage complexity by collecting individuals into groups and providing a representative which can be used to specify any individual of the group.

*Abstraction and generalization are often used together. Abstracts are generalized through parameterization to provide greater utility. In parameterization, one or more parts of an entity are replaced with a name which is new to the entity. The name is used as a parameter. When the parameterized abstract is invoked, it is invoked with a binding of the parameter to an argument.*

