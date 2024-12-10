# Nestjs Basics

## TOC

### Design pattern and principles

- Inversion of Control
- Dependency injection
- Decorator pattern
- How decorators work in typescript?

### NestJs Specific concepts

- Basic layout of a Nestjs project - Controllers, Providers, Module s
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

---

## Decorator in Typescript

- Javascript has proposals to introduce decorators and decorator metadata, Right now Typescript supports these functionalities and reflect-metadata is used for storing metadata.
- Decorators in typescript are declarations that can be attached to class, method, property and parameters which are executed in runtime
  
Example of a property decorator

```typescript
  
// property decorator
export const addPrefix = (prefix) => {
  return (target, propertyKey) => {
    let value = `${prefix} ${target[propertyKey]}`;

    // use getter and setter to send and get the values
    Object.defineProperty(target, propertyKey, {
      get: () => value,
      set: (newValue: string) => {
        value = `${prefix} ${newValue}`;
      },
    });
  };
};


```

---

Example of a adding metadata to a decorator

```typescript
// method decorator with metadata

export const ContainsBusinessLogic = () => {
  return (target: object) => {
    Reflect.defineMetadata('role', 'business-logic', target);
  };
};

export const doesObjectHaveBusinessLogic = (target: object) => {
  const metadata = Reflect.getMetadata('role', target);
  console.log('metadata', metadata);
  return metadata;
};

```

Ref:

- [Docs](https://www.typescriptlang.org/docs/handbook/decorators.html)
- [nestjs-decorators](https://github.dev/nestjs/nest/blob/master/packages/core/router/request/request-providers.ts)
- [Reflect-metadata](https://www.wolksoftware.com/blog/decorators-metadata-reflection-in-typescript-from-novice-to-expert-part-4)
- [Reflect](https://stackoverflow.com/questions/25421903/what-does-the-reflect-object-do-in-javascript)

---

## NestJS Basics

NestJS is a framework that provides an architecture to work from the start, it uses libraries like express to make it possible, it is allow flexible enough to make direct use of other libraries.

Common Files in a nestjs project:

Controller - Controller contains the logic for receiving the request and sending the response. Validation for requests can be handled here as well. In nestjs, @Controller decorator is used to inform

Service - Service is a layer where business logic should be written, A service is identified by adding Injectable decorator in class. This will inform nest to register it as a dependency

Module - Modules are used by Nest to organize the application structure into scopes. Controllers and Providers are scoped by the module they are declared in.

```typescript
  @Module({
    controllers: [AppController],
    providers: [AppService],
  })
  class AppModule {}

```

---
