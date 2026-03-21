# What are Semantic HTML Elements?

Semantic HTML elements are **HTML tags that clearly describe the meaning and purpose of their content both to browsers and developers**. Unlike generic tags like `<div>` and `<span>`, which offer no information about their role, semantic elements such as `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, and `<footer>` communicate what type of content they enclose.

## What Are Semantic HTML Elements?

Semantic HTML elements define their content based on **meaning** rather than just presentation or style. For example, `<header>` clearly indicates introductory or navigational content, while `<footer>` references context like copyright or links at the end of a page or section.

## Why Use Semantic HTML Elements?

- Improves **accessibility**: Semantic tags let screen readers and assistive technologies understand and convey the content's role, enhancing user experience for everyone.
- Boosts **SEO**: Search engines recognize semantic elements and use them to better understand a site's structure, which can improve rankings.
- Encourages **collaboration**: Semantic tags make code more readable and maintainable across large teams of developers.
- Future-proofs websites for evolving web standards and technologies.

## How to Use Semantic HTML Elements

- Use `<header>` for top-of-page or section intros, logos, or navigation menus.
- Use `<nav>` for major site navigational blocks or set of internal/external links.
- Use `<main>` to wrap the primary content unique to the page.
- Use `<section>` to group related content or create thematic zones (like chapters or feature lists).
- Use `<article>` for independent, self-contained content like news items, blog posts, or user comments.
- Use `<aside>` for tangential or complimentary content, such as sidebars, hints, or advertisements.
- Use `<footer>` for contact info, authorship, and closing content for a page or section.

## When and Where to Use Semantic Elements

- Use semantic elements whenever content fits one of their roles (i.e., clearly defines navigation, headers, articles, etc.).
- Avoid using them only for styling purposes; if a section lacks a clear thematic or semantic role, `<div>` may still be appropriate.

## Tradeoffs and Edge Cases

- **Tradeoff**: Some legacy browsers or screen readers may not fully support new semantic tags (HTML5 tags), but modern environments have robust support.
- **Edge cases**: Overusing semantic tags purely for layout, or nesting them incorrectly, can confuse both users and search engines.
- **Alternatives**: For purely presentational sections where no special meaning exists, non-semantic tags like `<div>` and `<span>` are still valid.

## Comparison Table: Semantic vs. Non-Semantic Elements

| Element     | Semantic? | Meaningful Structure | Typical Role                     |
| :---------- | :-------- | :------------------- | :------------------------------- |
| `<header>`  | Yes       | Yes                  | Page/section header              |
| `<nav>`     | Yes       | Yes                  | Primary navigation block         |
| `<section>` | Yes       | Yes                  | Thematic grouping/zone           |
| `<article>` | Yes       | Yes                  | Independent, self-contained item |
| `<footer>`  | Yes       | Yes                  | Page/section footer              |
| `<div>`     | No        | No                   | Generic container                |
| `<span>`    | No        | No                   | Inline generic container         |

## Measuring Outcomes

- Semantic HTML increases accessibility scores in audits (e.g., with Lighthouse) and improves SEO performance metrics.
- Well-chosen semantic elements facilitate automated testing and monitoring, contributing to **observability** and maintainability at scale.

Overall, **semantic HTML elements** are essential for building robust, scalable, accessible, and easily maintained websites.



---

# What is srcset in HTML?

The **srcset** attribute in HTML enables developers to specify multiple image sources for an `<img>` or `<source>` tag, allowing browsers to automatically choose the most appropriate image based on the device's screen size, resolution, or pixel density. It is a core tool for creating **responsive images**, optimizing both display quality and loading performance on a wide range of devices.

## What Is srcset?

The `srcset` attribute is a comma-separated list of image file URLs, each paired with a condition such as width (`w`) or pixel density (`x`) descriptor. It tells the browser what set of images are available for different situations, letting it select the best fit given the user's device characteristics.

## Why Use srcset?

- **Responsive images:** Improves visual quality across devices with varying resolutions and screen sizes.
- **Performance:** Reduces unnecessary bandwidth usage by preventing large images from loading on small or low-resolution screens, which can boost page loading speed.
- **SEO and accessibility:** Helps deliver optimized content, which benefits user experience and search engine rankings.

## How to Use srcset

- Use the `srcset` attribute directly on an `<img>` or `<source>` element, paired with the optional `sizes` attribute for advanced layout-based selection.
- Example:

```html
<img
  src="default.jpg"
  srcset="small.jpg 480w, medium.jpg 800w, large.jpg 1200w"
  sizes="(max-width: 600px) 480px, (max-width: 900px) 800px, 1200px"
  alt="Description"
/>
```

Here, the browser selects the best-fit image for the device and layout.

## When and Where to Use srcset

- Whenever serving images on a site that may be accessed from devices with different screen sizes or resolutions (e.g., desktops, tablets, smartphones, retina displays).
- Particularly valuable for large hero images, product photos, thumbnails, and any visually rich site elements that must look sharp while remaining efficient across devices.

## Tradeoffs and Edge Cases

- **Tradeoff:** Preparing and managing multiple image sizes increases complexity and build times, but the result is optimized performance and appearance.
- **Edge case:** If either descriptors or sizes are mismatched or omitted, the browser may not choose the ideal image, leading to blurry or oversized images.

## Comparison: srcset vs. picture Element

| Feature               | srcset           | picture            |
| :-------------------- | :--------------- | :----------------- |
| Device adaptation     | Yes              | Yes                |
| Art direction support | No               | Yes                |
| Fallback handling     | Required (`src`) | Handled by `<img>` |
| Syntax complexity     | Simpler          | More flexible      |

The **srcset** attribute is fundamental for delivering crisp and efficient images at scale, combining adaptability, performance, and accessibility in modern frontend development.



---

# Difference between display: none and visibility: hidden

**display: none** completely removes an element from the visual layout and document flow, meaning it takes up no space on the page, while **visibility: hidden** simply makes the element invisible but still reserves its space in the layout.

## What and Why: Key Differences

- **display: none**: The element is not rendered at all – it disappears from both the screen and the page's layout, as if it never existed.
  - Useful when the goal is to completely remove content, and allow surrounding elements to fill the gap.
- **visibility: hidden**: The element remains in the layout and still takes up space, but it is not visible to the user.
  - Useful for keeping fixed layouts or placeholder elements while hiding specific content temporarily.

## How: Syntax and Usage

```css
/* Removes the element from the layout */
.example {
  display: none;
}

/* Hides the element but keeps its space */
.example {
  visibility: hidden;
}
```

## When and Where to Use

- **Use display: none** when content should be fully removed, such as toggling menus, modals, or dynamically adding/removing elements.
- **Use visibility: hidden** for preserving layouts, temporarily hiding elements in tables, or indicating placeholder status without rearrangement.

## Tradeoffs and Edge Cases

- **display: none** impacts all descendant (child) elements—they're all hidden and excluded from layout, events, and accessibility trees.
- **visibility: hidden** only hides the element itself, potentially leaving children visible or accessible, and the element is still part of the accessibility tree.
- Certain browser quirks can affect event handling or accessibility depending on which property is used.

## Comparison Table

| Property           | Visible? | Space taken? | In document flow? | Descendants hidden?          |
| :----------------- | :------- | :----------- | :---------------- | :--------------------------- |
| display: none      | No       | No           | No                | Yes                          |
| visibility: hidden | No       | Yes          | Yes               | No (direct child could show) |

Choosing between these properties depends on **scalability** (how layouts adapt), **observability** (debugging what's rendered), and **security** (hiding sensitive info fully versus just visually).


---

# Basic performance-related common questions.

## What and Why: Examples of Performance Questions

- What are some common frontend performance bottlenecks and how do you resolve them?
- How do you optimize website loading speed?
- What tools do you use to measure and monitor web performance?
- How does lazy loading improve performance and when would you use it?
- What is critical rendering path optimization, and why is it important?
- How do you reduce the number of HTTP requests on a page?

## How and Tradeoffs: Sample Answers & Approaches

- Use **code splitting** and **lazy loading** for JS and images to load resources only when necessary, improving initial render time and overall resource consumption.
- **Minify** CSS and JavaScript, compress images, and utilize browser caching to reduce asset sizes and repeated data transfers.
- Employ **performance analysis tools** like Chrome DevTools, Lighthouse, or WebPageTest for measurement and diagnosis.
- **Reduce DOM manipulation**, avoid layout thrashing, and use virtual DOM strategies in frameworks like React for efficient updates.

## When and Where: Common Interview Contexts

- When discussing app or page slowness, incomplete paints, or user drop-offs tied to speed.
- Asked during system design, bug-fixing, or "how would you improve this product" interviews.

## Edge Cases and Observability

- **Tradeoff**: Over-optimizing for performance might reduce code readability or maintainability.
- **Edge cases**: Lazy loading may hurt UX if not combined with proper placeholders; excessive minification can complicate debugging.
- **Practice**: Always use **data-driven metrics** and repeat optimizations across devices and browsers.

## Table: Basic Performance Questions & What They Assess

| Question                                     | What It Tests                             |
| :------------------------------------------- | :---------------------------------------- |
| Common bottlenecks and solutions?            | Root cause analysis, optimization         |
| Tools for performance measurement?           | Familiarity with dev tools, observability |
| Code-splitting or lazy loading strategies?   | Resource management, advanced JS          |
| Experience with image or asset optimization? | Media efficiency and delivery             |
| Dealing with render-blocking resources?      | Critical path and parallelization         |

a) Loading Time Optimization

How do you reduce initial page load?
Use lazy loading for images
Defer non-critical JS
Minify and bundle CSS/JS
Use CDNs for static assets
Enable caching

b) Runtime Performance

How do you reduce layout thrashing?
Avoid frequent DOM reads/writes inside loops
Use requestAnimationFrame() for animations
How do you avoid memory leaks?
Clean up event listeners
Cancel timers and API calls when not needed

c) Rendering Performance

How do you improve rendering speed?
Avoid deep DOM trees
Minimize reflows and repaints
Use will-change CSS for expensive animations

d) Network & Asset Performance

What's the impact of large images or unoptimized assets?
Longer load times
Higher bounce rates
How do you handle responsive image loading?
Use srcset, picture, lazy loading

e) Monitoring Tools

Chrome DevTools (Performance, Lighthouse)
WebPageTest, PageSpeed Insights

---

# What is the use of the new operator in JavaScript?

The **new operator** in JavaScript is used to create an instance of a user-defined object type or one of the built-in object types that has a constructor function.

## What does new do?

When a constructor function is called with `new`, it performs these steps:

- Creates a new empty object.
- Sets the new object's internal [[Prototype]] to the constructor function's prototype property.
- Binds `this` inside the constructor to the new object, so properties and methods assigned to `this` become part of the object.
- Executes the constructor function with the given arguments.
- Returns the newly created object, unless the constructor explicitly returns a non-primitive object.

## Why use new?

- To **create multiple similar objects** easily with shared methods via the prototype chain.
- Implements **reusable object creation code** rather than constructing objects manually.
- Supports object-oriented programming patterns in JavaScript.

## Syntax example

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}
let person1 = new Person("Alice", 30);
console.log(person1.name); // Alice
```

Here, `new Person(...)` creates a new `Person` instance with its own properties initialized by the constructor.

## Summary

The new operator is essential in JavaScript for:

- Creating objects from constructor functions.
- Setting up prototype inheritance.
- Binding the context (`this`) to the new object during construction.

It provides a structured way to instantiate objects with shared behavior and state.


