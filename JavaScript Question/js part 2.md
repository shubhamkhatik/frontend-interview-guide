

# What's the difference between Repaint and Reflow (often confused with Rework)?

Here’s a comprehensive breakdown of the difference between **Repaint** and **Reflow** (sometimes called Rework) in the context of the browser’s rendering engine, using the what, why, how, when, and where approach:

### What

- **Repaint:**
    - This is the process where the browser redraws elements on the screen without changing their layout. It updates the visual styling (such as color, background, visibility, or outline) but does not alter the geometry (position or size).
- **Reflow (Rework):**
    - This is the process where the browser recalculates the layout of part or all of the webpage. Any changes that affect the geometry (like DOM structure, element dimensions, or fonts) trigger a reflow.


### Why

- **Repaint:**
    - Triggered to visually display changes such as color transitions, visibility toggling (`visibility: hidden`), or simple style alterations that do not influence an element’s physical size or placement.
- **Reflow:**
    - Needed when the structure of the document or the position and size of elements changes. Examples: adding/removing DOM elements, resizing the window, changing font size, or applying CSS styles that affect the box model.


### How

- **Repaint:**
    - The browser simply redraws the affected part of the screen with the new visual style.
    - No recalculations of positions or layouts occur.
    - Minimal performance cost compared to reflow.
- **Reflow:**
    - The browser examines the entire DOM tree (or a portion of it, depending on the change) and recalculates the exact position and size of every affected element.
    - It may be localized or propagate to parent and child elements if dependencies exist.
    - This is compute-intensive and can block UI rendering, causing performance issues, especially in complex UIs or during frequent changes.


### When

- **Repaint:**
    - When a property like `background-color`, `color`, `visibility`, or `outline` changes.
- **Reflow:**
    - When layout-affecting properties change: `width`, `height`, `margin`, `padding`, `border`, `display`, `position`, or the DOM is updated (adding/removing nodes, changing font properties).


### Where

- **Repaint:**
    - Happens only in the part of the viewport where the visual change occurs (the affected element and possibly its overlapping siblings).
- **Reflow:**
    - Can start at a single element but may require relayout for parent and child nodes, up to the entire document tree, depending on the scope of the change.


### Summary Table

| Aspect | Repaint | Reflow (Rework) |
| :-- | :-- | :-- |
| What | Redrawing visual styles | Recalculating element layout and geometry |
| Why | Visual-only changes (color, visibility) | Layout changes (size, position, structure) |
| How | Painting pixels, no layout processing | Recomputing box model, layout, dependencies |
| When | Style changes NOT affecting layout | Style/DOM changes affecting layout/geometry |
| Where | Localized to changed area | May affect part or all of the DOM tree |

### Key Insight

- **Reflows are more expensive** than repaints. For optimal performance, minimize layout changes and batch DOM updates when possible.
- Both reflows and repaints are necessary for dynamic UIs, but understanding the distinction helps developers build faster, smoother web applications.

---

# If a user clicks a button multiple times to fetch data, how to cancel old API calls and use only the latest result?

To ensure only the **latest API call is effective** when a user clicks a button multiple times (e.g., rapidly fetching data), you should cancel any ongoing (older) requests before starting a new one. This pattern prevents race conditions, wasted bandwidth, and displaying out-of-date data.

Let's break this solution down:

### What

**Technique:**
Cancelling outdated or redundant API calls and only processing the most recent API response by using an abort mechanism.

### Why

- **Avoids showing stale data:** Prevents responses from earlier/faster requests overwriting newer results.
- **Saves network resources:** Reduces unnecessary network traffic.
- **Improves user experience:** Ensures the displayed data always matches the latest user action.


### How

#### Modern JavaScript/Fetch API (using AbortController)

1. **Create an AbortController instance for each request.**
2. **Store the controller reference** outside the fetch function (e.g., in component state or closure).
3. **Before making a new request, call abort() on the existing controller**—this cancels any in-flight API call.
4. **Pass the controller’s signal to your fetch**.
5. **Handle abort errors** gracefully.

**Example:**

```javascript
let currentController;

async function fetchData(url) {
  // Abort any ongoing request
  if (currentController) {
    currentController.abort();
  }

  currentController = new AbortController();
  try {
    const response = await fetch(url, { signal: currentController.signal });
    const data = await response.json();
    // Only latest result will be processed
    displayData(data);
  } catch (error) {
    if (error.name === 'AbortError') {
      // Request was canceled; do nothing or log
    } else {
      // Handle other errors
      console.error(error);
    }
  }
}
```

In a **React component** (with useEffect), do this in the effect's cleanup function:

```jsx
useEffect(() => {
  const controller = new AbortController();
  fetch(url, { signal: controller.signal })
    .then(/* ... */)
    .catch(/* ... */);

  return () => controller.abort(); // this cancels previous request
}, [url]);
```


#### With Axios

Axios uses `CancelToken` (deprecated), but modern Axios supports `AbortController` as well. Create and abort controllers similarly before any new request is fired[^2_1][^2_2][^2_3][^2_4][^2_5][^2_6].

### When

- When you want the **UI to always correspond to the latest user action** and users can trigger data-fetching actions repeatedly (e.g., button clicks, typing in a search box, changing filters).
- Especially important in typeahead, auto-complete, selection lists, or pagination scenarios.


### Where

- Client-side JavaScript applications—most often with frameworks like React, Angular, Vue, but works in plain JS too.
- Modern browsers supporting ES6 and Fetch API. Axios (or similar libraries) can also be used.


### Key Summary Table

| Concept | What You Do | How/Where To Implement | Why |
| :-- | :-- | :-- | :-- |
| Cancel/Ignore old requests | Call `controller.abort()` on previous requests | Before every new fetch call (UI event handler, effect) | To prevent stale data and race conditions |
| Use only latest response | Store and check the latest controller | Anywhere you trigger network calls | To match the UI to user’s intent |

### References for Implementation Examples

- [Cancel Duplicate Fetch Requests in JavaScript][^2_1]
- [MDN: AbortController.abort()][^2_7]
- [React \& Fetch: Preventing Multiple API Calls][^2_4]
- [MDN: Using the Fetch API - Canceling a Request][^2_5]
- [DEV: Mastering Request Cancellation][^2_6]

**Tip:** Always check for `AbortError` in your `catch` block to avoid treating a canceled request as a failure.

**In Summary:**
**Abort previous requests before starting new API calls using `AbortController`** in modern browsers and fetch workflows. This ensures your UI and data are always up-to-date, even if the user triggers multiple requests in rapid succession[^2_1][^2_2][^2_3][^2_4][^2_5][^2_6].

<div style="text-align: center">⁂</div>

[^2_1]: https://austingil.com/cancel-duplicate-fetch-requests-in-javascript-enhanced-forms/

[^2_2]: https://dev.to/rigalpatel001/the-easy-way-to-cancel-fetch-requests-when-you-dont-need-them-1d3g

[^2_3]: https://dev.to/gyantocode/how-to-cancel-api-calls-in-react-with-axios-a-step-by-step-guide-41b6

[^2_4]: https://www.js-craft.io/blog/react-useeffect-multiple-api-calls-fetch-race-conditions/

[^2_5]: https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch

[^2_6]: https://dev.to/dharamgfx/mastering-request-cancellation-in-javascript-using-abortcontroller-with-axios-and-fetch-api-2589

[^2_7]: https://developer.mozilla.org/en-US/docs/Web/API/AbortController/abort

[^2_8]: https://stackoverflow.com/questions/31061838/how-do-i-cancel-an-http-fetch-request

[^2_9]: https://www.reddit.com/r/reactjs/comments/o9d0s6/how_to_cancel_all_prior_requests_and_only_display/

[^2_10]: https://www.c-sharpcorner.com/article/how-to-cancel-fetch-requests-in-javascript-using-abortcontroller/

[^2_11]: https://tr.javascript.info/fetch-abort

[^2_12]: https://javascript.plainenglish.io/stop-that-fetch-call-right-now-854fd26e78af

[^2_13]: https://www.youtube.com/watch?v=VdAlFWBUGV4

[^2_14]: https://proandroiddev.com/retrofit-cancelling-multiple-api-calls-4dc6b7dc0bbd

[^2_15]: https://www.codingdeft.com/posts/fetch-cancel-previous-request/

[^2_16]: https://blog.devgenius.io/how-to-cancel-promises-and-api-calls-in-react-and-next-with-axios-️-3cc373ae14b8

[^2_17]: https://stackoverflow.com/questions/68210088/how-to-stop-multiple-re-renders-from-doing-multiple-api-calls-useeffect

[^2_18]: https://www.reddit.com/r/reactjs/comments/14th28i/how_to_avoid_unneccesary_api_calls_whenever_react/

[^2_19]: https://wearecommunity.io/communities/india-united-ui-community/articles/3244

[^2_20]: https://dev.to/mr_mornin_star/how-to-synchronize-your-api-calls-with-your-component-lifecycle-in-react-with-a-custom-hook-82d


---

# Does React use Promise.allSettled() for parallel API calls? How does that work internally?

Let's break down the answer as per your interview guide's instructions:

### What

- **Promise.allSettled()** is a JavaScript method that allows running multiple promises (like API calls) in **parallel**, waiting for each to finish—regardless of whether they succeed or fail. The result is an array, with each element describing if that promise was “fulfilled” or “rejected,” alongside its value or reason[^3_1][^3_2][^3_3][^3_4][^3_5].
- **React itself does not use Promise.allSettled() internally** for parallel API calls. However, it’s a tool frequently used in React apps when you want to make multiple API requests at once and handle all their outcomes, regardless of individual success or failure.


### Why

- Use **Promise.allSettled()** when you want to:
    - Run several async calls at once (parallel).
    - Get the outcome of *every single call*—even if some fail.
    - Gracefully process API errors without breaking your whole app's data fetching logic.
- This is important in React apps, such as dashboards, where data for independent components should load regardless of other failures.


### How

**How does Promise.allSettled() work?**

1. Supply it with an array of promises (`[promise1, promise2, ...]`).
2. It starts all promises at once.
3. Once *all* promises have either resolved or rejected, it returns a results array, like:

```js
[
  { status: "fulfilled", value: ... },
  { status: "rejected", reason: ... }
]
```

4. You can then check each result’s status, using values from fulfilled ones, and gracefully handle/recover from failures[^3_1][^3_2][^3_3][^3_4][^3_6][^3_5].

**React Example:**

```javascript
useEffect(() => {
  Promise.allSettled([
    fetch("/api/one"),
    fetch("/api/two"),
    fetch("/api/three")
  ])
  .then(results => {
    results.forEach(result => {
      if (result.status === 'fulfilled') {
        // Use result.value
      } else {
        // Handle error: result.reason
      }
    });
  });
}, []);
```

**Internals:**
Promise.allSettled() is part of the JavaScript language, not React. It creates a wrapper promise that keeps track of all your input promises, collects the final status for each, and always resolves with all outcomes[^3_1][^3_2][^3_4][^3_6][^3_5]. It does **not** short-circuit if one fails (unlike Promise.all()).

### When

- Use Promise.allSettled() when:
    - You have multiple independent API calls and want to wait for *every* response, even if some fail (e.g., load widgets that work with partial data).
    - Showing loading spinners that should disappear only after *everything* is attempted.


### Where

- Use it in *React components* when you fire several unrelated async data fetches.
- Also useful in Node.js or any modern JavaScript environment with asynchronous needs.


### Key comparison (all vs allSettled)

|  | Promise.all() | Promise.allSettled() |
| :-- | :-- | :-- |
| Fails fast | Yes — rejects when any promise fails | No — always waits for all |
| Result | Array of resolved values (if all succeed) | Array: status/value or reason |
| Use-case | All-or-nothing batch loading | Partial results, resilient UI |

**Summary:**
React doesn’t use Promise.allSettled() internally, but you can—and should—leverage it for robust, parallel API calls when you want resilient data fetching without one failure spoiling all results. It gives you granular control over handling successes and errors for each individual request[^3_1][^3_2][^3_3][^3_4][^3_6][^3_5].

<div style="text-align: center">⁂</div>

[^3_1]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/allSettled

[^3_2]: https://dev.to/deep1144/handling-multiple-api-calls-with-promise-allsettled-1f80

[^3_3]: https://www.freecodecamp.org/news/how-to-use-promise-allsettled-in-javascript/

[^3_4]: https://dev.to/shameel/javascript-promise-all-vs-allsettled-and-race-vs-any-3foj

[^3_5]: https://www.geeksforgeeks.org/javascript/javascript-promise-allsettled-method/

[^3_6]: https://stackoverflow.com/questions/30362733/handling-errors-in-promise-all

[^3_7]: https://stackoverflow.com/questions/59285046/concurrent-api-calls-using-promise-allsettled-with-progress-notification

[^3_8]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise/all

[^3_9]: https://www.linkedin.com/pulse/promise-apis-all-allsettled-race-any-interview-questions-tejas-musale-kesif

[^3_10]: https://javascript.plainenglish.io/how-to-make-parallel-api-calls-in-javascript-4d429254a620

[^3_11]: https://dev.to/shameel/serial-vs-parallel-api-calling-in-javascript-3h3j

[^3_12]: https://stackoverflow.com/questions/74526251/react-promise-all-multiple-apis-different-error-handling

[^3_13]: https://namastedev.com/blog/javascript-promise-apis-explained-when-to-use-all-allsettled-race-and-any/

[^3_14]: https://mikebifulco.com/posts/solve-all-your-problems-with-promise-allsettled

[^3_15]: https://wesbos.com/javascript/12-advanced-flow-control/68-promises-error-handling

[^3_16]: https://hackmamba.io/blog/2020/12/aggregate-multiple-api-requests-with-promise-all/

[^3_17]: https://www.youtube.com/watch?v=qqybka4RSJ0

[^3_18]: https://www.geeksforgeeks.org/explain-promise-allsettled-with-async-await-in-javascript/

[^3_19]: https://dev.to/irreverentmike/solve-all-your-problems-with-promise-allsettled-444i


---

# What algorithm does Array.prototype.sort() use? What's the output of [1, null, 5, 2, undefined]?

Let’s break down your query step-by-step as requested:

### What algorithm does `Array.prototype.sort()` use?

**Array.prototype.sort()** uses a sorting algorithm that can **vary based on the JavaScript engine and environment**. The ECMAScript specification does **not mandate a specific algorithm**, so browsers choose optimized implementations:

- **V8 (Chrome, Node.js):** Uses a **hybrid of QuickSort, MergeSort, and InsertionSort**. For large, mostly unsorted arrays, it uses QuickSort or MergeSort (for stability); for small arrays or nearly sorted data, it uses InsertionSort[^4_1][^4_2].
- **Other engines (e.g., SpiderMonkey, JavaScriptCore):** May use different algorithms.

**Important:**

- *The output remains consistent: the array is sorted “in place”, according to the rules defined by the spec, but performance and stability may vary between engines.*


### How does `.sort()` decide ordering by default?

By default (without a comparator), **array elements are converted to strings** and compared in ascending order by Unicode code points, *not* numerically. This can lead to surprising results[^4_3][^4_4][^4_5][^4_6].

### What’s the output of `[1, null, 5, 2, undefined].sort()`?

#### Step-by-step:

1. **Default sort:**
    - Elements are converted to strings: `"1"`, `"null"`, `"5"`, `"2"`, `"undefined"`.
    - Sorted lexicographically: `"1"`, `"2"`, `"5"`, `"null"`, `"undefined"`.
2. **Actual output:**

```js
[1, 2, 5, null, undefined]
```

    - Numbers come first in string comparison: `"1"`, `"2"`, `"5"`, followed by `"null"` and `"undefined"`.

- If you used a numeric comparator (`(a, b) => a - b`), `null` is converted to `0`, `undefined` is converted to `NaN`. All valid numbers (including `null` as `0`) come first, and `undefined` values go last:

```js
[null, 1, 2, 5, undefined]
```

(as demonstrated in some engines and references[^4_7][^4_8][^4_9])


### Why and when does this matter?

- **Why:** Sorting sparse arrays or those with holes, `null`, or `undefined` can yield unexpected results.
- **Where:** Always check your environment for stability and implementation specifics if exact ordering is important.


### How to control the sorting of `null`/`undefined`?

- Use a custom comparator for predictable behavior; e.g., move `null`/`undefined` to the end:

```js
arr.sort((a, b) => {
  if (a == null) return 1;  // null or undefined at the end
  if (b == null) return -1;
  return a - b;             // numeric ascending
});
// Output: [1, 2, 5, null, undefined]
```


### Summary Table

| Engine/Scenario | Algorithm Used | `[1, null, 5, 2, undefined].sort()` Output |
| :-- | :-- | :-- |
| Default (no compare) | Lexical Sort | `[1, 2, 5, null, undefined]` |
| Numeric Comparator | Numeric + Null/Undefined handling | `[null, 1, 2, 5, undefined]` |
| Custom Comparator | User-defined | Depends on your logic |

**In short:**

- **Algorithm**: Varies by engine but typically QuickSort, MergeSort, or InsertionSort hybrids[^4_1][^4_2].
- **Default output**: `[1, 2, 5, null, undefined]`—numbers first, `null` and `undefined` last (as per Unicode string comparison).
- **Custom comparators**: Give you predictable, fine-grained control for sorting with special values[^4_3][^4_7][^4_9].

<div style="text-align: center">⁂</div>

[^4_1]: https://stackoverflow.com/questions/234683/javascript-array-sort-implementation

[^4_2]: https://v8.dev/blog/array-sort

[^4_3]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort

[^4_4]: https://www.w3schools.com/js/js_array_sort.asp

[^4_5]: https://www.lullabot.com/articles/sorting-data-javascript

[^4_6]: https://dev.to/akabikov/jsts-custom-sorting-order-or-how-to-sort-weird-things-3dfn

[^4_7]: https://philna.sh/blog/2019/08/26/how-not-to-sort-an-array-in-javascript/

[^4_8]: https://stackoverflow.com/questions/56312968/javascript-sort-object-array-by-number-properties-which-include-undefined

[^4_9]: https://www.geeksforgeeks.org/typescript/how-to-move-null-values-at-the-end-regardless-of-the-sorting-direction-in-typescript/

[^4_10]: https://blog.bitsrc.io/lets-implement-our-own-array-map-sort-methods-e89c9d5e2dc8

[^4_11]: https://dev.to/alexmercedcoder/introduction-to-sorting-algorithms-in-javascript-b60

[^4_12]: https://www.geeksforgeeks.org/typescript/how-to-sort-an-array-of-objects-with-null-properties-in-typescript/

[^4_13]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray/sort

[^4_14]: https://javascripttoday.com/blog/sorting-algorithms-with-javascript/

[^4_15]: https://stackoverflow.com/questions/9592740/how-can-you-sort-an-array-without-mutating-the-original-array

[^4_16]: https://www.geeksforgeeks.org/javascript/sorting-algorithms-in-javascript/

[^4_17]: https://forum.freecodecamp.org/t/what-are-the-types-of-array-sorting-algorithm/263436

[^4_18]: https://dev.to/christinamcmahon/common-sorting-algorithms-in-javascript-58a7

[^4_19]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/toSorted

[^4_20]: https://stackoverflow.com/questions/29829205/sort-an-array-so-that-null-values-always-come-last/58748962


---

# What happens when we hit a URL in the browser? What is CRP (Critical Rendering Path)?

Let’s systematically break down both concepts: **what happens when you hit a URL in the browser** and **what is the Critical Rendering Path (CRP)**, using the what, why, how, when, and where framework.

## 1. What happens when we hit a URL in the browser?

### What

- The browser initiates a series of steps to fetch and display the requested resource (usually a web page).


### Why

- This process retrieves and presents content to users, transforming addresses (URLs) into interactive pages.


### How

**Key steps:**

1. **URL Parsing:**
The browser parses the input URL to extract the protocol (https), hostname, path, query params, etc.
2. **DNS Lookup:**
The browser or system resolves the domain name to an IP address via DNS.
3. **Establish TCP Connection:**
The browser establishes a connection to the server (often with TLS/SSL for https).
4. **HTTP Request:**
The browser sends an HTTP request (usually GET) to the server at the specified path.
5. **Server Response:**
The server processes the request and sends back an HTTP response containing status, headers, and the web resource (HTML, data, etc.).
6. **Resource Fetching:**
The browser parses the HTML and starts discovering other resources (like CSS, JS, images) needed to render the page.
7. **Rendering (see CRP below):**
The browser processes, constructs, and paints the page on the screen.

### When

- Every time a user enters a URL, clicks a link, or reloads a page.


### Where

- This process happens between your browser, your operating system, your network infrastructure, and the web server.


## 2. What is CRP (Critical Rendering Path)?

### What

- **Critical Rendering Path** is the sequence of steps the browser takes to convert HTML, CSS, and JS resources into pixels rendered on the screen.


### Why

- Understanding CRP is essential for optimizing performance. Bottlenecks in this path delay the time it takes for a page to become usable.


### How

**CRP main steps:**

1. **HTML Parsing → DOM Construction:**
The browser parses HTML and builds the DOM (Document Object Model) tree.
2. **CSS Parsing → CSSOM Construction:**
Stylesheets are parsed into the CSSOM (CSS Object Model).
3. **Render Tree Construction:**
The browser combines the DOM and CSSOM to create the render tree, representing all visible nodes.
4. **Layout:**
The browser calculates the position and size for each node in the render tree (reflow).
5. **Painting:**
The browser paints the content to the screen (repaint).
6. **JavaScript Execution:**
JS can block or modify any earlier steps, so it’s fetched and executed as needed.

### When

- This happens every time a new page is loaded, or when major DOM/CSS changes occur in a SPA (Single Page Application).


### Where

- The CRP occurs inside the browser’s rendering engine (like Blink, WebKit, or Gecko).


## Quick Reference Table

| Process / Term | What | Why | How | When | Where |
| :-- | :-- | :-- | :-- | :-- | :-- |
| Browser Navigation | Turning URL into page | User content retrieval | DNS → HTTP → Render | On page visit/change | Browser + Network |
| Critical Rendering Path (CRP) | Steps to display pixels on screen from HTML/CSS/JS | Optimize speed \& experience | DOM → CSSOM → Render Tree → Layout → Paint | On every new/dynamic page load | Browser Rendering Engine |

**Key point:**

- Optimizing the Critical Rendering Path—by reducing resource sizes, minimizing render-blocking JS/CSS, and delaying non-critical resources—directly speeds up the time to first contentful paint and improves perceived performance.

