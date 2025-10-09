

***

## Frontend and Web Concepts Overview

### 1. Webpack Build Process

**Explanation:**
Webpack is a module bundler that starts from a single entry file (like `index.js`) and builds a dependency graph of all the modules and assets your app needs.

**Process Breakdown:**

- **Entry:** Webpack looks at your entry point (e.g., `main.js`).
- **Dependency Graph:** It analyzes imports and requires to build a graph of dependencies.
- **Loaders:** For non-JS files (like CSS, images, TypeScript), webpack uses loaders to transform them into valid modules.
- **Plugins:** Used for tasks like minification, injecting variables, cleaning folders, etc.
- **Output:** Finally, all modules are bundled into one or more files.

***

### 2. Architecting an Application for Multiple Devices

**Goal:** To ensure your application works seamlessly across mobile, tablets, and desktops.

**Approach:**

- Use **responsive design** with media queries and relative units (`em`, `%`, `rem`).
- Create adaptive layouts using **CSS Grid** or **Flexbox**.
- Serve optimized images with the **`srcSet`** attribute.
- Implement **device detection** to render conditional logic or UI.
- Follow **progressive enhancement** so that core features work even without JavaScript.

***

### 3. Use of Headers in HTTP Requests

**Definition:**
HTTP headers carry metadata and control how the client and server communicate.

**Examples:**

- **Content-Type:** Specifies the data type being sent (`application/json`, `text/html`).
- **Authorization:** Sends access tokens or credentials.
- **Cache-Control:** Defines browser caching policies.
- **User-Agent:** Identifies the browser or client making the request.

***

### 4. Render-Blocking Resources

**Definition:**
Render-blocking resources are CSS or JavaScript files that must be downloaded, parsed, and executed *before* rendering the page.

**Examples:**

- Large CSS files in the `<head>` without `media` or `async` attributes.
- Inline scripts running before `DOMContentLoaded`.

**Fixes:**

- Use **`async`** or **`defer`** for JavaScript files.
- Inline **critical CSS**.
- Load fonts or styles only when necessary.

***

### 5. Event Capturing vs Delegation vs Bubbling

**Definitions:**

- **Event Capturing:** The event starts at the window/root level and travels *down* to the target.
- **Event Bubbling:** After reaching the target, the event bubbles *up* from the target to the root.
- **Event Delegation:** Instead of attaching listeners to each child, attach one listener to the parent to handle bubbling events efficiently.

**Example:**

```js
document.getElementById('parent')
  .addEventListener('click', (e) => {
    if (e.target.matches('.child')) {
      // Handle child click
    }
  });
```


***

### 6. Symbols and Generators

**Definitions:**

- **Symbols:** A primitive data type for creating unique keys in objects (avoids naming conflicts).

```js
const sym1 = Symbol('id');
const obj = { [sym1]: 123 };
```

- **Generators:** Special functions that can pause and resume execution using `yield`. Useful for async flows and custom iterators.

```js
function* counter() {
  yield 1;
  yield 2;
}

const gen = counter();
gen.next(); // { value: 1, done: false }
```


***

### 7. Web Components, Service Worker, Web Worker, and PWA

**Definitions:**

- **Web Components:** A set of APIs to create reusable custom HTML elements (`CustomElement`, `Shadow DOM`, `HTML Template`).
- **Service Worker:** A background script that intercepts network requests and enables offline caching.
- **Web Worker:** Runs JavaScript in a separate thread to handle heavy tasks without blocking the UI.
- **Progressive Web App (PWA):** Web apps that behave like native apps — work offline, can be installed on the home screen, and load fast using service workers and caching.

