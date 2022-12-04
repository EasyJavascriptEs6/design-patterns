# Design-patterns
The patterns covered on this website and in the workshop can guide you when facing a problem other developers have encountered many times before, but are not a blunt tool for jamming into every scenario. The goal is to raise awareness to certain patterns, the problems they solve, and their implementation.

##  Module Pattern 
Split up your code into smaller, reusable pieces
----------
### Overview
ES2015 introduced built-in JavaScript modules. A module is a file containing JavaScript code and makes it easy to expose and hide certain values.

The module pattern is a great way to split a larger file into  **multiple smaller**,  **reusable pieces**. It also promotes  **code encapsulation**, since the values within modules are kept private inside the module by default, and cannot be modified. Only the values that are explicitly exported with the  `export`  keyword are accessible to other files.	  

https://user-images.githubusercontent.com/94198583/205502977-33b8f59d-9d0c-4d98-a420-d1d3298c9852.mov

The module pattern provides a way to have both public and private pieces with the `export` keyword. This protects values from leaking into the global scope or ending up in a naming collision.



https://user-images.githubusercontent.com/94198583/205503389-890ff634-53b2-4611-81fc-6e24617d494d.mov


In the above example, the `secret` variable is accessible within the module, but not outside of it. Other modules cannot import this value, as it hasn't been `export`ed.
## Implementation

There are a few ways we can use modules.

### HTML tag

When adding JavaScript to HTML files directly, you can use modules by adding the  `type="module"`  attribute to the  `script`  tags.

https://user-images.githubusercontent.com/94198583/205503304-2c7dafde-62be-457e-8afb-1fe07bea2a83.mov

### Tradeoffs
**Encapsulation**: The values within a module are scoped to that specific module. Values that aren't explicitly exported are not available outside of the module.

**Reusability**: We can reuse modules throughout our application

# Singleton Pattern

Share a single global instance throughout our application

----------

### Overview

With the Singleton Pattern, we restrict the instantiation of certain classes to one single instance. This single instance is unmodifiable, and can be accessed globally throughout the application.

For example, we can have a  `Counter`  singleton, which contains a  `getCount`,  `increment`, and  `decrement`  method.

![Singleton Pattern 1](https://user-images.githubusercontent.com/94198583/205504101-cefc6015-21e9-4aae-a3bb-213f5429f848.png)

This singleton can be shared globally across multiple files within the application. The imports of this Singleton all reference the same instance.

The  `increment`  method increments the value of  `counter`  by 1. Any time the  `increment`  method has been invoked anywhere in the application, thus changing the value of  `counter`, the change is reflected throughout the entire application.

https://user-images.githubusercontent.com/94198583/205504147-f99498fa-7bdf-43a2-848a-669f23cd2549.mov


### Implementation

In ES6, there are several ways to create Singletons.

#### Classes

Creating a singleton with an ES2015  `class`  can be done by:

```javascript
let instance;

// 1. Creating the `Counter` class, which contains a `constructor`, `getInstance`, `getCount`, `increment` and `decrement` method.
// Within the constructor, we check to make sure the class hasn't already been instantiated.
class Counter {
  constructor() {
    if (instance) {
      throw new Error("You can only create one instance!");
    }
    this.counter = counter;
    instance = this;
  }

  getCount() {
    return this.counter;
  }

  increment() {
    return ++this.counter;
  }

  decrement() {
    return --this.counter;
  }
}

// 2. Setting a variable equal to the the frozen newly instantiated object, by using the built-in `Object.freeze` method.
// This ensures that the newly created instance is not modifiable.
const singletonCounter = Object.freeze(new Counter());

// 3. Exporting the variable as the `default` value within the file to make it globally accessible.
export default singletonCounter;
```


#### Objects

We can also directly create objects without having to use a  `class`, which can lead to much simpler and cleaner code.

To create a singleton using a regular object, we have to:



```javascript
let counter = 0;

// 1. Create an object containing the `getCount`, `increment`, and `decrement` method.
const counterObject = {
  getCount: () => counter,
  increment: () => ++counter,
  decrement: () => --counter,
};

// 2. Freeze the object using the `Object.freeze` method, to ensure the object is not modifiable.
const singletonCounter = Object.freeze(counterObject);

// 3. Export the object as the `default` value to make it globally accessible.
export default singletonCounter
```

### Tradeoffs

**Memory**: Restricting the instantiation to just one instance could potentially save a lot of memory space. Instead of having to set up memory for a new instance each time, we only have to set up memory for that one instance, which is referenced throughout the application.

**Unnecessary**: ES2015 Modules are singletons by default. We no longer need to explicitly create singletons to achieve this global, non-modifiable behavior.

**Depedency Hiding**: When importing another module, it may not always be obvious that that module is importing a Singleton. This could lead to unexpected value modification within the Singleton, which would be reflected throughout the application.

**Global Scope Pollution**: The global behavior of Singletons is essentially the same as a global variable. Global Scope Pollution can end up in accidentally overwriting the value of a global variable, which can lead to a lot of unexpected behavior. Usually, certain parts of the codebase modify the values within global state, whereas others consume that data. The order of execution here is important, understanding the data flow when using a global state can get very tricky as your application grows, and dozens of components rely on each other.

**Testing**: Since we can't create new instances each time, all tests rely on the modification to the global instance of the previous test. The order of the tests matter in this case, and one small modification can lead to an entire test suite failing. After testing, we need to reset the entire instance in order to reset the modifications made by the tests.
