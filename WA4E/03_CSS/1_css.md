# Cascade Style Sheet (CSS)

## Introduction
CSS (Cascading Style Sheets) is a stylesheet language used to control the presentation of HTML documents. It allows web developers to separate content from design, making websites more visually appealing and easier to maintain.

## Features of CSS
- **Separation of Concerns**: Keeps HTML structure and design separate.
- **Consistency**: Enables uniform styling across multiple web pages.
- **Efficiency**: Reduces redundancy and minimizes code repetition.
- **Responsiveness**: Supports responsive web design techniques for different screen sizes.
- **Animation & Effects**: Allows animations, transitions, and visual effects.

## Types of CSS
1. **Inline CSS**: Styles applied directly to an HTML element using the `style` attribute.
   ```html
   <p style="color: blue; font-size: 20px;">This is an inline styled paragraph.</p>
   ```

2. **Internal CSS**: Styles defined within a `<style>` tag inside the HTML `<head>`.
   ```html
   <style>
       p {
           color: red;
           font-size: 18px;
       }
   </style>
   ```

3. **External CSS**: Styles written in a separate `.css` file and linked using the `<link>` tag.
   ```html
   <link rel="stylesheet" href="styles.css">
   ```

## Selectors in CSS
CSS uses selectors to target HTML elements:
- **Element Selector**: `p { color: blue; }`
- **Class Selector**: `.my-class { font-weight: bold; }`
- **ID Selector**: `#my-id { background-color: yellow; }`
- **Group Selector**: `h1, h2, p { margin: 10px; }`
- **Descendant Selector**: `div p { color: gray; }`

## Box Model in CSS
The CSS Box Model consists of the following components:
1. **Content**: The actual content inside an element.
2. **Padding**: Space between content and border.
3. **Border**: Surrounds padding and content.
4. **Margin**: Space outside the border, separating elements.

Example:
```css
.box {
    width: 200px;
    padding: 10px;
    border: 5px solid black;
    margin: 20px;
}
```

## Positioning in CSS
CSS provides different positioning properties:
- **Static** (default positioning)
- **Relative** (relative to its normal position)
- **Absolute** (relative to the nearest positioned ancestor)
- **Fixed** (fixed relative to the viewport)
- **Sticky** (toggles between relative and fixed based on scroll position)

Example:
```css
.fixed-box {
    position: fixed;
    top: 10px;
    right: 10px;
}
```

## Flexbox and Grid
- **Flexbox**: Used for flexible and responsive layouts.
  ```css
  .container {
      display: flex;
      justify-content: center;
      align-items: center;
  }
  ```
- **CSS Grid**: A powerful layout system for 2D designs.
  ```css
  .grid-container {
      display: grid;
      grid-template-columns: 1fr 1fr;
  }
  ```

## Media Queries (Responsive Design)
CSS media queries allow web pages to adapt to different screen sizes.
```css
@media (max-width: 600px) {
    body {
        background-color: lightgray;
    }
}
```

## Conclusion
CSS enhances web development by providing styling capabilities that improve website appearance, responsiveness, and user experience. Mastering CSS is essential for modern web design and front-end development.

