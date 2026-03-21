

# What events can we use when a website is loading? In-depth view of CRP (Critical Rendering Path)

When a website is loading, the browser goes through a precise sequence called the Critical Rendering Path (CRP), which is the process of converting HTML, CSS, and JavaScript into pixels rendered on the screen. Understanding and optimizing this sequence is essential for improving page load performance and user experience.

### Key Events and Steps in the Critical Rendering Path

1. **HTML Parsing and DOM Construction**
    - The browser receives HTML and parses it line by line.
    - It builds the Document Object Model (DOM) which is a tree-like structure representing the page structure.
    - This phase triggers the event `domInteractive` which marks when the DOM is ready to be interacted with, though subresources might still be loading.
2. **CSS Parsing and CSSOM Construction**
    - As the browser parses HTML, it encounters CSS resources.
    - CSS files are fetched and parsed into the CSS Object Model (CSSOM), which holds the CSS styles.
    - Critical: CSS parsing is render-blocking, so the browser waits for the CSSOM before proceeding.
    - When both DOM and CSSOM are ready, the event `DOMContentLoaded` typically fires, signaling that the HTML and CSS are fully parsed (unless JavaScript blocks).
3. **Render Tree Construction**
    - The browser combines the DOM and CSSOM to build the Render Tree.
    - The Render Tree contains only the nodes needed for rendering and how they are styled.
4. **Layout (Reflow)**
    - The browser calculates the size and position of every visible element on the page.
    - This stage defines how each element will be displayed spatially.
5. **Painting**
    - The final step involves rasterizing the pixels on the screen.
    - This is when users actually see the visual representation of the page.
6. **Load Events**
    - After rendering, `domComplete` occurs when all DOM processing and resource loading (images, scripts) finish.
    - The `load` event fires last, indicating the page and all its subresources have fully loaded.

### Important Browser Events During Loading Related to CRP

- `domInteractive`: DOM is parsed and interactive, but external resources may still be loading.
- `DOMContentLoaded`: DOM and CSSOM are fully parsed and ready, triggering this event (can be blocked by synchronous JavaScript).
- `domComplete`: All processing and resource loading are finished.
- `loadEvent` or `load`: All resources including images, stylesheets, and scripts are fully loaded.


### Why CRP Optimization Matters

- The time until the first paint or content is visible is crucial for user experience.
- Blocking resources (especially CSS and synchronous JavaScript) delay render.
- Progressive rendering is preferable so the browser shows something usable as early as possible without "flash of unstyled content" or layout shifts.
- Optimizing CRP focuses on reducing critical resources, asynchronous loading for non-critical scripts, minimizing CSS blocking, and efficient resource delivery to speed up rendering.


### Summary of CRP Sequence for Rendering

| Step | Description | Related Event |
| :-- | :-- | :-- |
| HTML Parsing | Build DOM | `domInteractive` |
| CSS Parsing | Build CSSOM |  |
| DOM \& CSSOM Combined | Build Render Tree | `DOMContentLoaded` |
| Layout Calculation | Calculate position and size of elements |  |
| Painting | Rasterize pixels to screen |  |
| Full Resource Loaded | Images, scripts fully loaded | `domComplete`, `load` |

Understanding these events helps you track loading progress and optimize the web page to load and display faster with less perceived delay.

This detailed view reveals how to measure and optimize the CRP, making it essential knowledge for frontend and full-stack developers aiming to build performant websites.[^1_1][^1_2][^1_3][^1_4][^1_5][^1_6]
<span style="display:none">[^1_10][^1_11][^1_7][^1_8][^1_9]</span>

<div style="text-align: center">⁂</div>

[^1_1]: https://developer.mozilla.org/en-US/docs/Web/Performance/Guides/Critical_rendering_path

[^1_2]: https://web.dev/learn/performance/understanding-the-critical-path

[^1_3]: https://web.dev/articles/critical-rendering-path/measure-crp

[^1_4]: https://www.frontendgeek.com/blogs/understanding-critical-rendering-path-crp-to-improve-web-performance

[^1_5]: https://www.linkedin.com/pulse/understanding-browser-rendering-critical-path-divyansh-singh

[^1_6]: https://dev.to/abhinavshinoy90/inside-the-critical-rendering-path-what-your-browser-is-really-doing-15mp

[^1_7]: https://angular.love/101-javascript-critical-rendering-path/

[^1_8]: https://www.geeksforgeeks.org/web-tech/critical-rendering-path-flow/

[^1_9]: https://kinsta.com/blog/critical-rendering-path/

[^1_10]: https://dev.to/vikas2426/demystifying-the-critical-rendering-path-a-deep-dive-into-web-page-loading-19ni

[^1_11]: https://nitropack.io/blog/post/critical-rendering-path-optimization


---

# Difference between Prototypal and Classical Inheritance in JavaScript

Here is an in-depth comparison between Prototypal and Classical Inheritance in JavaScript:

### Classical Inheritance

- **Based on classes:** In classical inheritance (typical in languages like Java, C++, Python), you define classes as blueprints for objects.
- **Class hierarchy:** Objects (instances) inherit properties and methods from classes, which can form a parent-child (superclass-subclass) hierarchy.
- **Instantiation:** New objects are created by instantiating classes.
- **Static structure:** The inheritance structure is usually fixed at compile time.
- **Syntax (Java example):**

```java
class Animal {
  void eat() { System.out.println("This animal eats food."); }
}
class Dog extends Animal {
  void bark() { System.out.println("The dog barks."); }
}
Dog dog = new Dog();
dog.eat();  // Inherited
dog.bark(); // Own method
```


### Prototypal Inheritance in JavaScript

- **Based on objects:** There are no classes in JavaScript (before ES6). Objects inherit directly from other objects via prototypes.
- **Prototype chain:** Each object has a prototype object linked via an internal property. Objects inherit properties and methods from their prototype chain.
- **Dynamic and flexible:** You can modify objects and prototypes at runtime, add or override properties and methods dynamically.
- **Object creation:** Objects can be created from other objects using `Object.create()` or constructor functions with `.prototype`.
- **Underlying mechanism of ES6 classes:** Even after ES6 added `class` syntax, under the hood it's still prototypal inheritance.
- **Syntax (JavaScript example):**

```js
const animal = {
  eat() { console.log("This animal eats food."); }
};
const dog = Object.create(animal);
dog.bark = function() { console.log("The dog barks."); };
dog.eat();  // Inherited from animal
dog.bark(); // Own method
```


### Key Differences

| Aspect | Classical Inheritance | Prototypal Inheritance |
| :-- | :-- | :-- |
| Paradigm | Class-based inheritance | Prototype-based inheritance |
| Inheritance structure | Class hierarchies (static) | Prototype chains (dynamic, runtime) |
| Object creation | Instantiate classes | Create objects directly from other objects |
| Flexibility | Less flexible after compile time | Very flexible (modify prototypes at runtime) |
| Language example | Java, C++, Python | JavaScript, Lua, Self |
| JavaScript syntax | ES6 `class` syntax is syntactic sugar | Prototype chain with objects and constructor functions |
| Typical usage | Defines blueprints for objects (classes) | Objects inherit methods and properties directly from other objects |

### Summary

- In **classical inheritance**, classes define blueprints, and objects are instances of these classes forming a hierarchical tree.
- In **prototypal inheritance**, objects inherit directly from other objects, without a class. JavaScript’s inheritance is prototypal, even when using ES6 class syntax under the hood.
- Prototypal inheritance offers more dynamic flexibility since the prototype chain can be modified at runtime.

This understanding is crucial for JavaScript developers to grasp how inheritance and object extension truly work in the language.[^2_1][^2_2][^2_3][^2_5][^2_6][^2_7]
<span style="display:none">[^2_4][^2_8]</span>

<div style="text-align: center">⁂</div>

[^2_1]: https://stackoverflow.com/questions/19633762/classical-inheritance-vs-prototypal-inheritance-in-javascript

[^2_2]: https://dev.to/crishanks/classical-vs-prototypal-inheritance-2o5a

[^2_3]: https://www.greatfrontend.com/questions/quiz/explain-the-difference-between-classical-inheritance-and-prototypal-inheritance

[^2_4]: https://www.reddit.com/r/learnjavascript/comments/16y8b2b/inheritance_vs_classical_is_this_a_succinct/

[^2_5]: https://www.linkedin.com/pulse/javascript-prototype-chain-inheritance-explained-from-ross-baker

[^2_6]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Inheritance_and_the_prototype_chain

[^2_7]: https://codetrace.com/interview-questions/5d30565d4ccf4c0010333c36/what-is-the-difference-between-classical-and-prototypal-inheritance

[^2_8]: https://www.youtube.com/watch?v=doXpW5AD60Q


---

# How does JavaScript handle asynchronous operations? What mechanisms does it use?

JavaScript handles asynchronous operations using several key mechanisms designed to allow tasks to run without blocking the main thread, enabling efficient and responsive applications despite being single-threaded and synchronous by nature.

### How JavaScript Handles Asynchronous Operations

#### 1. **Call Stack (Synchronous Execution)**

- JavaScript runs code on a single thread and processes it synchronously using a call stack.
- When a function is called, it’s pushed onto the call stack, executed, and then popped off.
- Synchronous code blocks further execution until it completes.


#### 2. **Web APIs / Background Threads**

- Browser environments and Node.js provide APIs separate from the JavaScript engine to handle asynchronous tasks, such as timers (`setTimeout`), HTTP requests (`fetch`), DOM events, and file I/O.
- These APIs operate in the background without blocking the JS main thread.


#### 3. **Callback Queue / Task Queue**

- When asynchronous APIs complete their operation, callbacks are pushed into the callback queue.
- Callbacks wait here until the call stack is empty so they can be executed.


#### 4. **Event Loop**

- The event loop continuously watches the call stack and the callback queue.
- When the call stack is empty, it takes the first callback from the queue and pushes it onto the call stack for execution.
- This enables asynchronous code to run without blocking other operations.


### Asynchronous Patterns and Tools in JavaScript

#### **Callbacks**

- Functions passed as arguments to be executed after an async operation finishes.
- Oldest pattern but can lead to "callback hell" with complex nested callbacks.


#### **Promises**

- Objects representing future completion or failure of asynchronous operations.
- Provide `.then()`, `.catch()`, and `.finally()` for handling async results and errors more cleanly than callbacks.
- Improve readability and error handling.


#### **Async/Await (ES2017+)**

- Syntactic sugar built on promises.
- Allows asynchronous code to be written in a style that looks synchronous using `async` functions and `await` keyword.
- Improves readability and flow of asynchronous code.


### Summary of JavaScript Asynchronous Mechanisms

| Mechanism | Description |
| :-- | :-- |
| Call Stack | Executes JavaScript code synchronously, one frame at a time. |
| Web APIs | Browser/Node APIs that process asynchronous tasks outside JS engine (timers, HTTP, events). |
| Callback Queue | Queues callbacks from asynchronous operations waiting execution. |
| Event Loop | Monitors call stack and callback queue to manage execution order, ensuring non-blocking async processing. |
| Callbacks | Basic async handling by passing functions to be executed later. |
| Promises | Represent future values with cleaner async handling and chaining capabilities. |
| Async/Await | Modern syntax for writing asynchronous code more like synchronous style. |

This model lets JavaScript run potentially long tasks (e.g., fetching data, timers) in the background while keeping the UI and main code thread responsive.[^3_1][^3_3][^3_4][^3_5][^3_7][^3_8][^3_9]
<span style="display:none">[^3_2][^3_6]</span>

<div style="text-align: center">⁂</div>

[^3_1]: https://www.freecodecamp.org/news/asynchronous-programming-in-javascript/

[^3_2]: https://www.w3schools.com/js/js_asynchronous.asp

[^3_3]: https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS/Introducing

[^3_4]: https://www.geeksforgeeks.org/javascript/asynchronous-javascript/

[^3_5]: https://bugfender.com/blog/asynchronous-javascript/

[^3_6]: https://www.linkedin.com/pulse/mastering-javascript-asynchronous-programming-comprehensive-guide-vzwbf

[^3_7]: https://www.freecodecamp.org/news/asynchronous-javascript/

[^3_8]: https://eloquentjavascript.net/11_async.html

[^3_9]: https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Async_JS


---

# What are the SOLID Principles?

The SOLID principles are a set of five foundational design principles for writing maintainable, scalable, and robust object-oriented software. These principles help developers create code that is easier to understand, test, extend, and modify without introducing bugs or excessive complexity. The acronym SOLID stands for:

### 1. Single Responsibility Principle (SRP)

- **Definition:** A class should have only **one reason to change**—it should have a single responsibility or job.
- **Benefit:** Makes code easier to understand, maintain, and test by keeping each module focused on one task.


### 2. Open-Closed Principle (OCP)

- **Definition:** Software entities (classes, modules, functions) should be **open for extension** but **closed for modification**.
- **Benefit:** You can extend behavior without changing existing code, reducing the risk of bugs and making the system more robust and scalable.


### 3. Liskov Substitution Principle (LSP)

- **Definition:** Objects of a superclass should be replaceable with objects of a subclass without affecting the correctness of the program.
- **Benefit:** Ensures derived classes extend base classes without altering expected behavior, promoting polymorphism and safer inheritance.


### 4. Interface Segregation Principle (ISP)

- **Definition:** Clients should not be forced to depend on interfaces they do not use. Instead of one fat interface, make many small, specific interfaces.
- **Benefit:** Reduces side effects and unintended dependencies by keeping interfaces focused and client-specific.


### 5. Dependency Inversion Principle (DIP)

- **Definition:** High-level modules should not depend on low-level modules. Both should depend on abstractions (e.g., interfaces). Abstractions should not depend on details; details should depend on abstractions.
- **Benefit:** Promotes loose coupling, making systems more modular, flexible, and easier to refactor or extend.

***

### Why Use SOLID Principles?

- They **reduce dependencies** so changes in one part don’t ripple through the entire system.
- They improve **code readability, maintainability, and testability**.
- They help build **flexible and scalable software** that can evolve without breaking existing functionality.
- Though applying SOLID may increase initial design complexity, it saves time and cost in the long term by reducing bugs and refactoring needs.

***

These principles were introduced by Robert C. Martin (Uncle Bob) around 2000 and remain fundamental for good object-oriented software design across many programming languages.[^4_1][^4_2][^4_3][^4_5][^4_8]
<span style="display:none">[^4_4][^4_6][^4_7]</span>

<div style="text-align: center">⁂</div>

[^4_1]: https://www.bmc.com/blogs/solid-design-principles/

[^4_2]: https://www.digitalocean.com/community/conceptual-articles/s-o-l-i-d-the-first-five-principles-of-object-oriented-design

[^4_3]: https://www.freecodecamp.org/news/solid-design-principles-in-software-development/

[^4_4]: https://en.wikipedia.org/wiki/SOLID

[^4_5]: https://codefinity.com/blog/The-SOLID-Principles-in-Software-Development

[^4_6]: https://scalastic.io/en/solid-dry-kiss/

[^4_7]: https://stackify.com/solid-design-principles/

[^4_8]: https://www.geeksforgeeks.org/system-design/solid-principle-in-programming-understand-with-real-life-examples/


---

# How do we use OOP in JavaScript?

JavaScript uses Object-Oriented Programming (OOP) by leveraging objects and prototypes to model real-world entities with data (properties) and behavior (methods). Although JavaScript is prototype-based rather than class-based, it supports OOP concepts like encapsulation, inheritance, and polymorphism through various mechanisms.

### How to Use OOP in JavaScript

#### 1. **Using Constructor Functions (Pre-ES6)**

- Constructor functions act like classes to create multiple similar objects.
- Use the `new` keyword to instantiate objects.
- Methods can be added to the constructor’s `.prototype` to be shared.

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.greet = function() {
  console.log(`Hello, my name is ${this.name}`);
};

const john = new Person("John", 30);
john.greet();  // Hello, my name is John
```


#### 2. **Using ES6 Classes (Syntactic Sugar)**

- ES6 introduced `class` syntax which is clearer and more similar to classical OOP languages.
- Behind the scenes, ES6 classes use prototypes.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    console.log(`Hello, my name is ${this.name}`);
  }
}

const jane = new Person("Jane", 25);
jane.greet();  // Hello, my name is Jane
```


#### 3. **Prototypal Inheritance**

- Objects can inherit directly from other objects using prototypes.
- This allows method sharing and reuse.
- Methods like `Object.create()` help link prototypes easily.

```js
const animal = {
  speak() {
    console.log("Animal speaks");
  }
};

const dog = Object.create(animal);
dog.bark = function() {
  console.log("Dog barks");
};

dog.speak(); // Animal speaks
dog.bark();  // Dog barks
```


#### 4. **Encapsulation with Objects**

- Data and methods are grouped within objects.
- Closure and Symbols can be used for private properties.

***

### Core OOP Concepts Supported in JavaScript:

| OOP Principle | How JavaScript Supports It |
| :-- | :-- |
| Encapsulation | Objects group data and functions together |
| Inheritance | Prototypes and `class` inheritance allow reuse |
| Polymorphism | Methods can be overridden in derived objects |
| Abstraction | Objects expose only necessary properties/methods |


***

### Why Use OOP in JavaScript?

- Encapsulates code for better organization and maintainability.
- Promotes code reuse with prototype-based inheritance.
- Makes complex applications easier to manage by modeling real-world entities.

JavaScript’s flexible OOP support lets developers use classical style (classes) or prototypal object-based style depending on their needs.[^5_1][^5_2][^5_3][^5_4]
<span style="display:none">[^5_5][^5_6][^5_7][^5_8][^5_9]</span>

<div style="text-align: center">⁂</div>

[^5_1]: https://www.geeksforgeeks.org/javascript/introduction-object-oriented-programming-javascript/

[^5_2]: https://developer.mozilla.org/en-US/docs/Learn_web_development/Extensions/Advanced_JavaScript_objects/Object-oriented_programming

[^5_3]: https://www.freecodecamp.org/news/object-oriented-programming-javascript/

[^5_4]: https://www.honeybadger.io/blog/javascript-oop/

[^5_5]: https://www.youtube.com/watch?v=GEuS0tfLfEY

[^5_6]: https://www.freecodecamp.org/news/object-oriented-javascript-for-beginners/

[^5_7]: https://birdeatsbug.com/blog/object-oriented-programming-in-typescript

[^5_8]: https://www.w3schools.com/java/java_oop.asp

[^5_9]: https://www.reddit.com/r/javascript/comments/1bd1elv/askjs_is_object_oriented_programming_pointless/

