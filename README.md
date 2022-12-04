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
