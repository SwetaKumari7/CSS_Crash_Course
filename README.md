# CSS_Crash_Course
For quick revision, just see this ~~~
# CSS Quick Revision — Fundamentals

## 1. Types of CSS

### Inline CSS
CSS is written directly inside an HTML element using the `style` attribute.

```html
<p style="color: blue;">Hello World</p>
```

### Internal CSS
CSS is written inside a `<style>` element, usually in the HTML `<head>`.

```html
<style>
  p {
    color: blue;
  }
</style>
```

### External CSS
CSS is written in a separate `.css` file and linked to the HTML document.

**index.html**
```html
<link rel="stylesheet" href="style.css">
```

**style.css**
```css
p {
  color: blue;
}
```

**Important:** External CSS is generally preferred for maintainability, reuse, and cleaner HTML. When declarations conflict, the winner depends on origin, importance, specificity, and source order—not simply whether CSS is inline, internal, or external.

## 2. Types of CSS Selectors

### ID selector
Selects an element by its ID.

```css
#myId {
  color: red;
}
```

```html
<p id="myId">Hello</p>
```

An ID should be unique within an HTML document.

### Class selector
Selects every element with the specified class.

```css
.myClass {
  color: blue;
}
```

```html
<p class="myClass">First paragraph</p>
<p class="myClass">Second paragraph</p>
```

A class can be reused across multiple elements, and an element can have multiple classes.

### Element (tag) selector
Selects all elements of a specified type.

```css
p {
  font-size: 20px;
  font-family: Arial, sans-serif;
  font-weight: bold;
  color: rgb(187, 0, 255);
}
```

### Attribute selector
Selects elements based on the presence or value of an attribute.

```css
[title] {
  color: green;
}

input[type="text"] {
  border: 1px solid black;
}
```

### Pseudo-class
Selects an element in a particular state or condition.

```css
a:hover {
  text-decoration: underline;
}

input:focus {
  border-color: blue;
}
```

Other examples: `:first-child`, `:nth-child(2)`, `:checked`, `:not()`.

### Pseudo-element
Styles a specific part of an element or inserts generated content.

```css
p::first-line {
  font-weight: bold;
}

p::before {
  content: "Note: ";
}
```

Other examples: `::after`, `::first-letter`, `::selection`.

## 3. Selector differences at a glance

| Selector | Syntax | Purpose |
|---|---|---|
| ID | `#myId` | Select by ID |
| Class | `.myClass` | Select by class |
| Element | `p` | Select by element type |
| Attribute | `[title]` | Select by attribute |
| Pseudo-class | `a:hover` | Select by state or condition |
| Pseudo-element | `p::first-line` | Select a specific part |

## 4. CSS specificity — frequently asked

Specificity determines which selector wins when competing declarations apply to the same property, after the cascade's origin and importance have been considered.

A useful simplified comparison:

| Selector | Specificity |
|---|---:|
| `p` | 0-0-1 |
| `.myClass` | 0-1-0 |
| `#myId` | 1-0-0 |
| `#myId .myClass` | 1-1-0 |
| Inline `style` attribute | Higher than normal selector rules |

Example:

```html
<p id="myId" class="myClass">Hello</p>
```

```css
p {
  color: green;
}

.myClass {
  color: blue;
}

#myId {
  color: red;
}
```

**Result: red**, because the ID selector has higher specificity than the class and element selectors.

Remember:
- `!important` changes the cascade's priority; it is not a specificity score.
- If specificity is equal, source order usually decides between otherwise equivalent declarations.
- Inheritance is not the same as specificity.
- Inline styles do not automatically defeat every `!important` declaration.

## 5. CSS font units: px, em, rem

### Pixels (`px`)
A CSS pixel is a fixed CSS length unit, not necessarily one physical screen pixel.

```css
p {
  font-size: 16px;
}
```

### `em`
For `font-size`, `em` is relative to the inherited font size of the parent. For many other properties, it is based on the element's own computed font size.

```css
.parent {
  font-size: 20px;
}

.child {
  font-size: 1.5em; /* 30px */
}
```

### `rem`
`rem` is relative to the root element's (`html`) font size.

```css
html {
  font-size: 16px;
}

h1 {
  font-size: 2rem; /* 32px */
}
```

**Common confusion:** Nested `em` font sizes can compound; `rem` avoids that nesting effect because it refers to the root font size.

## 6. The CSS Box Model

Every element is represented by four layers:

1. Content — the text or other content.
2. Padding — space inside the border.
3. Border — the edge surrounding padding and content.
4. Margin — space outside the border.

```css
.container {
  width: 300px;
  height: 200px;
  padding: 20px;
  border: 5px solid black;
  margin: 10px;
}
```

With the default `box-sizing: content-box`:

- Content width = 300px
- Total border-box width = 300 + 40 + 10 = **350px**
- Width including left and right margins = 350 + 20 = **370px**

For easier sizing:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

With `border-box`, the declared width includes the content, padding, and border.

**Common confusion:** Margin is outside the border; padding is inside it. Vertical margins between normal block elements can sometimes collapse.

## 7. Flexbox

Flexbox is a one-dimensional layout system. It arranges items along a main axis, which can be horizontal or vertical.

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 10px;
}
```

Important properties:

- `flex-direction` — chooses the main axis direction.
- `justify-content` — distributes items along the main axis.
- `align-items` — aligns items along the cross axis.
- `flex-wrap` — allows items to wrap onto additional lines.
- `gap` — adds spacing between items.

**Common confusion:** `justify-content` is not always horizontal, and `align-items` is not always vertical. Their directions depend on `flex-direction`.

## 8. CSS positioning

### Static
```css
.element {
  position: static;
}
```
The default position. The element follows normal document flow; `top`, `right`, `bottom`, and `left` do not reposition it.

### Relative
```css
.element {
  position: relative;
  top: 10px;
  left: 20px;
}
```
The element is offset from its normal position. Its original space remains in the layout.

### Absolute
```css
.parent {
  position: relative;
  width: 200px;
  height: 200px;
}

.child {
  position: absolute;
  top: 50px;
  left: 50px;
  width: 100px;
  height: 100px;
}
```
The child is removed from normal flow and positioned relative to its nearest qualifying containing block, often an ancestor with `position: relative`.

### Fixed
```css
.element {
  position: fixed;
  top: 0;
  right: 0;
}
```
Usually positioned relative to the viewport and stays in place while scrolling. Certain ancestor properties can establish a different containing block.

### Sticky
```css
.element {
  position: sticky;
  top: 0;
}
```
Behaves like a relatively positioned element until a scrolling threshold is reached, then sticks within its scrolling container's limits. It needs an inset such as `top: 0` for the expected effect and can be affected by overflow and container height.

**Common confusion:** `static` does not mean horizontal order; normal flow determines the layout. `relative` does not remove the element from flow, while `absolute` and `fixed` generally do.

## 9. Quick interview revision

- **Class vs ID:** Classes are reusable; IDs should be unique.
- **Padding vs margin:** Padding is inside the border; margin is outside.
- **`em` vs `rem`:** `em` depends on font-size context; `rem` depends on the root font size.
- **Relative vs absolute:** Relative preserves its original layout space; absolute is removed from normal flow.
- **Flexbox vs Grid:** Flexbox is primarily one-dimensional; Grid supports two-dimensional row-and-column layouts.
- **Pseudo-class vs pseudo-element:** A pseudo-class selects a state or condition; a pseudo-element targets a particular part.
- **`display: none` vs `visibility: hidden`:** Both hide the element visually, but `display: none` removes its layout box while `visibility: hidden` generally preserves its space.

Keep practicing these concepts with small HTML and CSS examples. Understanding why a rule works is more useful than memorizing its syntax.
