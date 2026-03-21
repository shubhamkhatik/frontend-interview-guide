



***

### **Question 1: Can we bind `this` in an arrow function? What happens if we use the `new` operator with an arrow function?**

**Answer:**
Arrow functions do not have their own `this`. Instead, they *inherit* `this` from their lexical (surrounding) scope.
Trying to use `bind`, `call`, or `apply` on an arrow function does **not** affect its `this`.

```js
const obj = {
  value: 42,
  method: () => {
    console.log(this.value); // `this` is not `obj`
  }
};

obj.method(); // undefined
```

Even if you try to bind it:

```js
const fn = () => this;
const bound = fn.bind({ name: 'Test' });

console.log(bound()); // Still refers to original `this`, not { name: 'Test' }
```

**Using `new` with arrow functions:**
You cannot use arrow functions as constructors because they lack the `[[Construct]]` internal method.

```js
function Person() {} 
const p = new Person(); // Works fine

const ArrowPerson = () => {};
const ap = new ArrowPerson(); // ❌ TypeError: ArrowPerson is not a constructor
```

**Takeaway:**
Arrow functions are ideal for short, context-bound callbacks — not for object instantiation.

***

### **Question 2: Difference between Map and Object in JavaScript**

| Feature | Map | Object |
| :-- | :-- | :-- |
| Key Types | Can be any value (object, function, primitive) | Keys are always strings or symbols |
| Order of Keys | Maintains insertion order | Key order is not guaranteed (in older JS versions) |
| Iteration | Easy with `map.forEach()` or `for...of` | Must use `Object.keys()`, `Object.entries()` |
| Performance | Better for frequent additions/removals | Slower with dynamic key addition/removal |
| Size Retrieval | `.size` property | Must use `Object.keys(obj).length` |
| Use Case | When you need key-value pairs with non-string keys or frequent updates | When you need simple key-value storage with string keys |

**Use Map when:**

- You need **non-string keys**
- You want **guaranteed key order**
- You **frequently add/remove** keys

***

### **Question 3: Closure, Event Loop, Hoisting, and Currying**

#### **Closure**

A **closure** is a function that remembers its outer scope even after the outer function has finished executing.

```js
function outer() {
  let count = 0;
  return function inner() {
    return ++count;
  };
}

const inc = outer();
console.log(inc()); // 1
console.log(inc()); // 2
```

**Uses:** Private variables, event handlers, functional programming.

***

#### **Event Loop**

The **event loop** allows JavaScript (which is single-threaded) to handle asynchronous operations.

**Main Components:**

- **Call Stack:** Executes synchronous code
- **Task Queue:** Handles `setTimeout`, DOM events, etc.
- **Microtask Queue:** Handles promises (`.then()` callbacks)

The event loop constantly checks if the stack is empty and processes pending tasks accordingly.

***

#### **Hoisting**

**Hoisting** means variable and function declarations are moved to the top of their scope before execution.

```js
console.log(a); // undefined
var a = 5;

console.log(b); // ReferenceError
let b = 10;
```

With `let` and `const`, variables enter a *Temporal Dead Zone (TDZ)* until declared.

***

#### **Currying**

**Currying** transforms a function with multiple arguments into a series of functions, each taking one argument.

```js
function add(a) {
  return function (b) {
    return a + b;
  };
}

const add5 = add(5);
console.log(add5(3)); // 8
```

**Benefits:** Enables *partial application* and *function composition*.

***

### **Question 4: What are Web Core Vitals? How to improve them?**

**Core Web Vitals** are metrics defined by Google to measure real-world user experience.


| Metric | Meaning | Ideal Value | How to Improve |
| :-- | :-- | :-- | :-- |
| **LCP (Largest Contentful Paint)** | Loading performance | ≤ 2.5s | Optimize images, use CDN, reduce render-blocking CSS/JS |
| **FID (First Input Delay)** | Interactivity | ≤ 100ms | Defer non-critical JS, split large bundles, minimize long tasks |
| **CLS (Cumulative Layout Shift)** | Visual stability | ≤ 0.1 | Set image dimensions, avoid layout shifts from dynamic content |

**Tools:**

- Chrome DevTools
- Lighthouse
- PageSpeed Insights

***

### **Question 5: Web Performance Metrics**

**Key Metrics:**


| Metric | Description |
| :-- | :-- |
| **TTFB (Time to First Byte)** | Time taken for the browser to receive the first byte from the server |
| **FCP (First Contentful Paint)** | When the browser renders the first DOM content |
| **LCP (Largest Contentful Paint)** | When the main content (e.g., hero image, heading) is visible |
| **FID (First Input Delay)** | Delay between user’s first interaction and browser response |
| **TBT (Total Blocking Time)** | Total time the main thread is blocked (complements FID) |
| **CLS (Cumulative Layout Shift)** | Measures unexpected layout shifts |
| **INP (Interaction to Next Paint)** | Newer metric replacing FID, measures overall responsiveness |

**Optimization Tips:**

- Use **lazy loading** for images and videos
- Serve **compressed, next-gen image formats** (WebP, AVIF)
- **Minify and split** JS/CSS bundles
- Use a **CDN** for static assets
- Reduce **third-party script overhead**
- Implement **SSR or SSG** (Server-Side Rendering / Static Site Generation)

***



