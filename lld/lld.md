# LLD Basics

## Reference

- [sudoCode Course](https://www.youtube.com/watch?v=5Tz9EUMHOGI&list=PLTCrU9sGybupCpY20eked6blbHI4zZ55k&index=2)
- [book recommendations](https://youtu.be/aJxx-Y42rF0?feature=shared)

### What is LLD?

Low level design refers to process of designing a small part of system that solves one or more requirements

Process of low level design

- Requirement gathering
- Use cases
- UML/Class Diagram (can use NVT noun verb technique)
- Using Object Oriented Design to model the problem
- Implement the code with design patterns, SOLID Principles
- Make it maintainable, testable

Use cases diagram helps to understand how the software should look like

---

### Programming Paradigms

Programming paradigms are ways we can organize a program to accomplish certain things. Common programming paradigms are OOPS, functional programming, declarative programming and structured programming.

---

## Object oriented thought process book

reading the book to understand object oriented programming in a conceptual way

## Chapter 1 - Introduction to Object Oriented Concepts

What is an object?

An entity that have it's own attributes and behaviors is called as an object.
Entity - It is thing that has a clear identity

What problem does object oriented programming solve?
In procedural, structural programming paradigm, data can be manipulated anywhere this leads to uncertainty of accessing and modifying the data, and also makes it difficult to test

Object oriented programming solves this problem by providing guidelines such as only the object can changes it's data (attributes)

---

What is a class?

A class is a template of an object. Attributes, accessors and behaviors are defined in classes. When a object is created, the attributes, behaviors are created based on the class.

attribute -> data
behavior -> method
accessors -> access a certain attribute or method

UML -> It is a modelling tool to assist in designing software

messages -> When object A wants to talk with Object B, Object A will only be able to access public method or property of Object B

---

Encapsulation is

- Hiding details
- Provides an interface for other units to be used

Encapsulation is not specific to OOP but more in general.
Using modules also provides Encapsulation
Following OOP provides Encapsulation in code.

Inheritance is inheriting attributes and behaviors from a class. Inheritance is beneficial when compared with a function call because it lets u create relationship between classes
Superclass - parent, sub class -child

a sub class has **"is a"** relationship with it's parent

Polymorphism

- Accomplish multiple things with a single interface, it is a more generic concept
- Say we have list of objects with a same method, each object will have different behaviour with same interface like save function on all the games

Composition is another mechanism of building objects, Composition is creating objects with objects. Composition makes **"has-a"** relationship from parent to child class

---

## Chapter 2 - How to think in terms of object

### Difference between Interface and Implementation

Interface: What's exposed from a class is the interface, Ideally an interface should expose only what the user needs, interface also specifies the parameters and it's type so that should not also change.
Question: What if new data is required?

Implementation: It means how a functionality is implemented in the class, TLDR: Changes from user side should not be required to change the implementation. Implementation and user both of them should conform to the Interface, Think of deciding an API contract and handling the communication to user but also making sure it conforms to interface as well.

### Using Abstract thinking

Advantage of OOP is having reusable classes, In general, creating abstract class than concrete classes is more beneficial for creating reusable classes.
To create re usable classes, we need to create public interfaces
Example: Creating a interface for saying play music is better than saying play "Pop music"

### Design rules

When designing a class provide as little knowledge of implementation as possible,
Followings rules can be followed to accomplish this

- Start with Exposing only what's absolutely necessary
- Better to add more interfaces later then creating more interfaces now
- Design the class from user's perspective
- Make sure to spend effort on user's requirements

### Process of creating Object

1. Identifying users

- Users are the one who will actually use the system.
- Taking an example of taxi booking system, here the users are both taxi driver and the consumer

2. Object Behaviors

- Once users are identified, think about what are the behaviors of the users
- Think it from different viewpoint, One way is look at a user from a different user perspective
- Identify the purpose of each user, how can they provide their service, what do they need to do this

3. Environmental Constraints

- Think through what issues might arise, what is not possible

4. Identify Public Interface: Think about what a particular object needs to expose, it is good practice to expose only one behavior per interface.
5. Implementation: We can use private methods to hide implementations between users, Implementation can also use different classes.

## Chapter 3 - Advanced Object oriented concepts

Concepts such as Constructors, operator overloading and multiple inheritance is for understanding lower level design and implementation but from a higher level

### Constructors

- Constructors are methods that are called during initialization of a Class, the attributes of a class constructed in safe state when constructors are called. "new" keyword is used to initialize a class.
- when constructors are initialized memory will be allocated for it and it's super classes.
- In OO languages, constructors should will have the same name as class and it will not return anything. Javascript excluded since it has a method called constructor.
- If a constructor is not given to a class, the compiler will add a default constructor and have super call to initialize the super class, it is always good to create a constructor even if it is not planned to be used from a maintenance perspective.
- Multiple constructors are possible in OO languages (JS excluded) until they are called with different parameters

#### Design things for constructors

- Thumb rule is to create constructors for classes even if it is not going to be used.
- Good practice to initialize all attributes with a value that is stable

```java

    public class ExampleClass {
        public void ExampleClass() {

        }

        public void ExampleClass(exampleParameter) {
            
        }
    }

    ExampleClass example = new ExampleClass();
```

### Error Handling

2 optimal ways are described ways described in the book to handle errors

- Checking for problems and recovering
- Throwing exception

#### Checking for problem and recovering

If the root cause of an issue is identifiable and can be recovered it will be a good idea to add some conditions to not crash the system and record it for observability.
One example is dividing by zero, since it is not possible we can override the parameter that sends it

#### Throwing an exception

Exception is a way to deal with problem where we don't expect errors, this is done through a try catch block, There will be granularity of exceptions depending upon the situation
