---
toc: true
comments: true
layout: posts
title: SASS hacks
description: for importing in css from sass build
categories: [C1.0]
courses: { csa: {week: 14} }
type: hacks
---

## Hacks

Create a grid layout that automatically adjusts the number of columns based on the screen size, using SASS variables and functions.

ALSO OTHER HACK:

Define a custom SASS function that uses a for loop in order to slightly decrease the saturation and increase the brightness of a color of your choosing and fill in those increasingly more white colors into a 3x3 array of equal height and width.


<html>

<head>
  <link rel="stylesheet" href="/stud/sass/output.css">
  <title>Responsive Grid Layout test</title>
</head>
<body>
  <div class="grid-container">
    <div class="grid-item">1</div>
    <div class="grid-item">2</div>
    <div class="grid-item">3</div>
    <div class="grid-item">4</div>
</div>
    <div class="color-square">
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
    <div></div>
  </div>
</body>
</html>

HACK: Try changing the primary color to an invalid value (e.g., 'red') and observe the @error message. Then, correct it to a valid color.

RESULT:

`Error: Invalid color provided: red. Please provide a valid color.
        on line 66 of test.scss, in `validate-color'
        from line 72 of test.scss
  Use --trace for backtrace.`

changing it to just red without the quotes works 

HACK: Modify the base font size and observe the @debug message. Try different font sizes and see how it affects the calculated line height.

debug message: `DEBUG: Calculating line height for font size: 16pxpx`


### Fill in the blanks

In the design phase of any project, maintaining uniformity is extremely important for creating a polished look. SASS allows for this by allowing the use of variables to store and reuse colors, fonts, and other design elements.

This makes it so that there is a _consistent_ theme applied throughout the entire project. SASS allows for visual cohesion.

This also allows for feedback and ___iterative refinement____. People can make adjustments to the visual elements without complexity of functional code, and it makes sure that all requirements are met. 

These visual concepts also play a role in planning the _responsive_ design. The team members can visualize how layouts and styles are adapted for different screen__ ____sizes_____, so that all users can have a great visual experience across all types of devices.




## Partials and Modular Styling with SASS

### Understanding SASS Partials:

SASS partials are separate files containing any specific style or component. They allow for better organization and modularization of styles. They play a very important role in organizing and modularizing styles. 

Partials are named with a leading underscore (e.g., `_variables.sass`) to indicate that they are meant to be ___imported___ into another stylesheet.

### Benefits of Using Partials:

1. **Modular Organization:**
   - Partials break down stylesheets into smaller files, each focusing on a specific aspect (e.g., variables, typography, layout).
   - This modular approach improves code organization, making it easier to maintain and scale.

2. **Code Reusability:**
   - Partials enable the reuse of styles across multiple files. For example, a `_variables.sass` partial may store color schemes and fonts, allowing for greater consistency.

3. **Readability and Collaboration:**
   - Smaller files enhance code readability. Developers can quickly locate and understand specific styles.
   - Supports ____concurrent____ development, allowing different team members to work on different partials simultaneously.

### Importing Partials into a Main SASS File:

To use SASS partials, import them into a main SCSS file using the `@import` directive. The main file (e.g., `main.sass`) serves as the entry point for compiling styles.


SASS variables provide a way to store information for later use in a stylesheet. They offer several advantages, including enhanced maintainability and consistency, by allowing you to define values in ___one___ location.