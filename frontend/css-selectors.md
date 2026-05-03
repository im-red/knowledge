# CSS Selectors Cheat Sheet

## 1. Basic Selectors
Basic selectors target elements based on their type, class, ID, or universally.

| Selector | Example | Description |
| :--- | :--- | :--- |
| **Universal** | `*` | Selects all elements. |
| **Type (Tag)** | `div` | Selects all `<div>` elements. |
| **Class** | `.card` | Selects all elements with `class="card"`. |
| **Chained (Multiple)**| `.btn.active` | Selects elements that have *both* `class="btn"` and `class="active"`. |
| **ID** | `#header` | Selects the element with `id="header"`. |
| **List (Grouping)**| `h1, h2, h3` | Selects all `<h1>`, `<h2>`, and `<h3>` elements. |

---

## 2. Combinators
Combinators target elements based on their relationship with other elements in the HTML hierarchy.

| Selector | Example | Description |
| :--- | :--- | :--- |
| **Descendant** | `div p` | Selects all `<p>` elements that are *inside* a `<div>` (at any depth). |
| **Child** | `ul > li` | Selects all `<li>` elements that are *direct children* of a `<ul>`. |
| **Adjacent Sibling**| `h1 + p` | Selects the `<p>` element placed *immediately after* an `<h1>`. |
| **General Sibling**| `h1 ~ p` | Selects all `<p>` elements that are siblings *following* an `<h1>`. |

---

## 3. Attribute Selectors
Attribute selectors target elements based on the presence or value of their attributes.

| Selector | Example | Description |
| :--- | :--- | :--- |
| **Presence** | `[target]` | Selects all elements with a `target` attribute. |
| **Exact match** | `[type="text"]` | Selects elements where the `type` attribute exactly equals `"text"`. |
| **Contains word** | `[class~="btn"]` | Selects elements whose class attribute contains the whole word `"btn"`. |
| **Starts with** | `[href^="https"]`| Selects elements whose `href` attribute starts with `"https"`. |
| **Ends with** | `[href$=".pdf"]` | Selects elements whose `href` attribute ends with `".pdf"`. |
| **Contains string**| `[href*="google"]`| Selects elements whose `href` attribute contains the substring `"google"`. |
| **Hyphenated** | `[lang|="en"]` | Selects elements with `lang` attribute equal to `"en"` or starting with `"en-"`. |

*(Note: Adding `i` before the closing bracket makes it case-insensitive, e.g., `[href$=".pdf" i]`)*

---

## 4. Pseudo-classes
Pseudo-classes target an element based on its state or position in the DOM.

### Links and User Action States
| Selector | Example | Description |
| :--- | :--- | :--- |
| **:link** | `a:link` | Selects unvisited links. |
| **:visited** | `a:visited` | Selects visited links. |
| **:hover** | `btn:hover` | Selects an element when the user's mouse is over it. |
| **:active** | `btn:active` | Selects an element while it is being clicked/pressed. |
| **:focus** | `input:focus` | Selects the element that currently has keyboard focus. |
| **:focus-within**| `form:focus-within`| Selects an element if it or any of its descendants have focus. |

### Structural & Tree-Structural
| Selector | Example | Description |
| :--- | :--- | :--- |
| **:root** | `:root` | Selects the document's root element (usually `<html>`). |
| **:first-child** | `li:first-child` | Selects an element that is the first child of its parent. |
| **:last-child** | `li:last-child` | Selects an element that is the last child of its parent. |
| **:nth-child(n)** | `tr:nth-child(even)`| Selects the *n*th child of its parent (accepts numbers, `odd`, `even`, or formulas like `3n+1`). |
| **:first-of-type**| `p:first-of-type` | Selects the first `<p>` element among its siblings. |
| **:last-of-type** | `p:last-of-type` | Selects the last `<p>` element among its siblings. |
| **:nth-of-type(n)**| `p:nth-of-type(2)` | Selects the 2nd `<p>` element among its siblings. |
| **:only-child** | `p:only-child` | Selects a `<p>` element that is the only child of its parent. |
| **:empty** | `div:empty` | Selects a `<div>` that has no children (including text nodes). |

### Form States
| Selector | Example | Description |
| :--- | :--- | :--- |
| **:checked** | `input:checked` | Selects checkboxes or radio buttons that are checked. |
| **:disabled** | `input:disabled` | Selects disabled form elements. |
| **:enabled** | `input:enabled` | Selects enabled form elements. |
| **:valid / :invalid**| `input:invalid` | Selects inputs with valid/invalid values based on their type/pattern. |
| **:required** | `input:required` | Selects inputs with the `required` attribute. |

### Logical (Modern CSS)
| Selector | Example | Description |
| :--- | :--- | :--- |
| **:not()** | `div:not(.alert)` | Selects `<div>` elements that do *not* have the class `.alert`. |
| **:is()** | `:is(h1, h2) p` | Selects `<p>` elements inside either `<h1>` or `<h2>`. Removes repetition. |
| **:where()** | `:where(h1, h2) p`| Same as `:is()`, but always has **0 specificity**. |
| **:has()** | `article:has(img)`| *Parent selector:* Selects an `<article>` *only if* it contains an `<img>`. |

---

## 5. Pseudo-elements
Pseudo-elements style a specific *part* of an element. (Always use double colons `::` to distinguish them from pseudo-classes).

| Selector | Example | Description |
| :--- | :--- | :--- |
| **::before** | `p::before` | Inserts content *before* the content of `<p>`. Requires `content` property. |
| **::after** | `p::after` | Inserts content *after* the content of `<p>`. Requires `content` property. |
| **::first-letter** | `p::first-letter` | Selects the first letter of a `<p>` element. |
| **::first-line** | `p::first-line` | Selects the first line of text within a `<p>` element. |
| **::selection** | `::selection` | Styles the portion of an element selected (highlighted) by the user. |
| **::placeholder** | `input::placeholder`| Styles the placeholder text of a form input. |
| **::marker** | `li::marker` | Styles the bullet or number of a list item. |

---

## 6. At-Rules
At-rules encapsulate CSS rules and apply them based on specific conditions or define special behaviors.

| At-Rule | Example | Description |
| :--- | :--- | :--- |
| **@media** | `@media (max-width: 360px) { ... }` | Applies rules only if the device meets the media query criteria (e.g., screens up to 360px wide). |
| **@supports** | `@supports (display: grid) { ... }` | Applies rules only if the browser supports a specific CSS feature. |
| **@keyframes** | `@keyframes slideIn { ... }` | Defines the intermediate steps (keyframes) in a CSS animation sequence. |
| **@font-face** | `@font-face { font-family: 'Custom'; }` | Specifies a custom font to display text. |

---

## 7. Nesting (SCSS / Modern CSS)
Nesting allows you to write CSS rules inside each other, creating a visual hierarchy that mirrors the HTML/DOM structure. This is native to preprocessors like SCSS/SASS and is now part of standard Modern CSS.

| Feature | Example | Description |
| :--- | :--- | :--- |
| **Descendant Nesting**| `.mark-section-header { h4 { ... } ion-button { ... } }` | Targets `h4` and `ion-button` elements *inside* `.mark-section-header`. |
| **Parent Selector** | `.btn { &:hover { ... } }` | The `&` references the parent selector directly (compiles to `.btn:hover`). |
| **Suffixing (SCSS)** | `.card { &__title { ... } }` | Appends a suffix to the parent class (compiles to `.card__title`). |

---

## Specificity Hierarchy (Lowest to Highest)
If multiple CSS rules apply to the same element, the browser uses **specificity** to determine which one wins:

1. **Universal selectors** (`*`) and combinators (`>`, `+`, `~`) = `0`
2. **Type/Element selectors** (`div`, `p`) & **Pseudo-elements** (`::before`) = `1`
3. **Class selectors** (`.class`), **Attributes** (`[type="text"]`), & **Pseudo-classes** (`:hover`) = `10`
4. **ID selectors** (`#id`) = `100`
5. **Inline styles** (`style="..."`) = `1000`
6. **`!important`** = Overrides almost everything (use sparingly).

### Calculation Examples
Specificity is often represented as a 3-part value: `(ID, Class/Attribute/Pseudo-class, Type/Element)`. When comparing rules, evaluate the ID count first, then Classes, then Types.

| Selector | IDs | Classes | Types | Specificity Value |
| :--- | :---: | :---: | :---: | :--- |
| `h1` | 0 | 0 | 1 | **(0, 0, 1)** |
| `ul li` | 0 | 0 | 2 | **(0, 0, 2)** |
| `.btn` | 0 | 1 | 0 | **(0, 1, 0)** |
| `ul.menu li.item a` | 0 | 2 | 3 | **(0, 2, 3)** |
| `.classA.classB` | 0 | 2 | 0 | **(0, 2, 0)** |
| `a:hover` | 0 | 1 | 1 | **(0, 1, 1)** |
| `[type="text"]:focus`| 0 | 2 | 0 | **(0, 2, 0)** |
| `#header .nav-link` | 1 | 1 | 0 | **(1, 1, 0)** |
| `#main div#content .text p` | 2 | 1 | 2 | **(2, 1, 2)** |

*Note: Inline styles (e.g., `style="..."`) beat all external CSS rules by adding a higher tier `(1, 0, 0, 0)`, and `!important` overrides almost everything else.*