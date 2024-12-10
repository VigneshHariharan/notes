# Nestjs Basics

## TOC

### Design pattern and principles

- Inversion of Control
- Dependency injection
- Decorator pattern
- How decorators work in typescript?

### NestJs Specific concepts

- Basic layout of a Nestjs project - Controllers, Providers, Module - where it originated from, what it helps solving
- Controllers, Providers
- Writing a basic API in Nestjs

---

## Inversion of Control

Inversion of Control is a design principle where you keep the business logic clean by externalizing all the configurations, lifecycle management and any system interaction (storing to a file, any low level api).
Complex tasks should be handled by an Inversion of control framework

### Example 1

Event handlers in JS, using addEventListener to execute business logic but the listener is executed by the browser

### Example 2

Difference can by seen by writing an API in express and write an api in Nestjs

In express, you have to define route and you need to link the routes, method required and then you call it when required
In Nestjs, you simply tell what API it is whether get, post and define route, internally it will use express which can be replaced, the routes defined will be called by the browser

Ref:

- [IOC](https://youtu.be/37eHZza5aBk?feature=shared)
- [Wiki IOC](https://en.wikipedia.org/wiki/Inversion_of_control)

---

## Dependency Injection

Dependency: You have 2 classes (water, bottle), water has a method called drink which uses a method from bottle class, here bottle is a dependency of water.

Dependency injection means before water object is created, you create bottle object and pass it to water object. This way you can use multiple types of bottles for water object.

Dependency Injection is a specific way of implementing Inversion of control principle where an object's dependencies are injected into it.

This provides flexibility and ease of testing code.

---

### Example 1

creating a log on console

Without dependency Injection

```javascript
    const makeALog = () => {
        console.log("Log in console")
        console.log("Log in console 2")
    }

    makeALog();

    // If i need to change console log to another library I have to change the function

```

With dependency Injection

```javascript
    const makeALog = (logger) => {
        logger("Log in console")
        logger("Log in console 2")
    }

    makeALog(console.log);

    const addExtraTextWithLog = (logTextPassed) => console.log("Extra: " + logTextPassed)

    makeALog(addExtraTextWithLog)

```

---

### Example 2

Without Dependency Injection

```javascript
// S3Storage implementation
class S3Storage {
  save(fileName, data) {
    console.log(`Saving ${fileName} to S3...`);
    // Logic for saving to S3
  }
}

// LocalStorage implementation
class LocalStorage {
  save(fileName, data) {
    console.log(`Saving ${fileName} to Local Storage...`);
    // Logic for saving to LocalStorage
  }
}

// FileSaver tightly coupled with S3Storage
class FileSaver {
  constructor() {
    this.storage = new S3Storage(); // Dependency is hardcoded
  }

  saveFile(fileName, data) {
    this.storage.save(fileName, data);
  }
}

// Usage
const fileSaver = new FileSaver();
fileSaver.saveFile('example.txt', 'File data');

```

---

With Dependency Injection

```javascript
// S3Storage implementation
class S3Storage {
  save(fileName, data) {
    console.log(`Saving ${fileName} to S3...`);
    // Logic for saving to S3
  }
}

// LocalStorage implementation
class LocalStorage {
  save(fileName, data) {
    console.log(`Saving ${fileName} to Local Storage...`);
    // Logic for saving to LocalStorage
  }
}

// FileSaver is now decoupled from the storage implementation
class FileSaver {
  constructor(storage) {
    this.storage = storage; // Dependency is injected
  }

  saveFile(fileName, data) {
    this.storage.save(fileName, data);
  }
}

// Usage with S3Storage
const s3Storage = new S3Storage();
const fileSaverWithS3 = new FileSaver(s3Storage);
fileSaverWithS3.saveFile('example.txt', 'File data');

// Usage with LocalStorage
const localStorage = new LocalStorage();
const fileSaverWithLocal = new FileSaver(localStorage);
fileSaverWithLocal.saveFile('example.txt', 'File data');
```

---

## Decorator pattern

- Add extra functionality to a class without making any changes to the class which follows Open Closed Principle.

Open Closed Principle - A class should be open for extension but closed for modification
Relies on Object Composition
Implementing Decorator, how typescript decorator works?
Injectable, Controller In nestjs.

What is Composition?
Difference between Inheritance & Composition

### Example of decorator pattern

```javascript
    class Coffee {
        cost() {
            return 60
        }
    }

    class WithSugar {
        constructor(coffee) {
            this.coffee = coffee
        }
        cost() {
            return this.coffee.cost() + 10
        }
    }

    class WithMilk {
        constructor(coffee) {
            this.coffee = coffee
        }
    cost() {
            return this.coffee.cost() + 40
        }
    }

    let coffee = new Coffee();
    coffee = new WithSugar(coffee);
    coffee = new WithMilk(coffee);

    console.log('total cost of items:', coffee.cost())

```
