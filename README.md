#Semantic Grid

A semantic CSS grid that uses mixins to apply column-related styles to HTML elements.

#Table of Contents:
  - [Semantic Grid](#)
  - [Features](#)
	  - [Configure the Grid](#)
	  - [The Configure the Columns](#)
  - [Frequently-Asked Questions](#)
	  - [Why Use @mixin Instead of classes like .col-lg-1 and .col-md-2?](#)

# Features
The Semantic Grid is highly customizable to meet your needs when developing a
layout for your pages.

## Configure the Grid
You can dynamically configure the following aspects of the grid at run-time:
  - Number of columns
  - Gutter width
  - Grid Width

## The Configure the Columns
  You can dynamically configure the following aspects of a column _each time you use the mixin_:
  - Width
  - Dynamically alter padding
  - Force a column to be right-aligned or centered
  - "Push" or "Pull" a column a number of column-widths

# Frequently-Asked Questions
## Why Use ``@mixin`` Instead of classes like ``.col-lg-1`` and ``.col-md-2``?

A lot of well-known frameworks (Foundation5 and Bootstrap come to mind) use classes to denote column widths:

```
  <div class="col col-lg-3 col-md-6 col-sm-12">
      <div class="thumbnail">
          <div class="img skyline beijing"></div>
          <div class="desc">
              Neque porro quisquam est qui dolorem ipsum quia
              dolor sit amet, consectetur, adipisci velit.
              Neque porro quisquam est qui dolorem ipsum quia
              dolor sit amet, consectetur, adipisci velit.
          </div>
      </div>
    </div>
```

This approach several __advantages__:

1. It allows you to control how an element will look at different screen widths just by adding a series of size-related classes.

2. It lets you leverage CSS pseudo-selectors like ``:last-of-type``, and ``[class*='col']`` to dynamically control styling of columns and rows without having to add additional classes like ``.col--last``.

3. It clearly shows what classes affect what markup. You can easily see what's styling a block of HTML just by looking at the class list.

4. Using the ``class`` attribute gives you the ability to dynamically alter the layout of the page using JavaScript.

Unfortunately, it also has several __disadvantages__:

1. You can't easily store column widths in variables without using some kind of templating engine. This makes maintenance rather tedius if you have a lot of markup that shares the same ``col-`` classes.

2. It's not semantic. Classes like ``col-sm-5`` don't do anything to help _describe_ the kind of data the markup represents. Semantic markup means using CSS for styling and HTML for describing the data. Pure style-based class names should be kept to a minimum if not eleminated all together.

3. It causes code-bloat in the HTML and the CSS. The class-based approach to grids causes a lot of extra markup (in the form of wrapper ``<div>``s), larger HTML files (due to the length of class names), and larger CSS files (due to every grid-related class having to be generated up-front instead of as-needed). When I converted this project from a class-based to a mixin-based approach, I saved 2KB in the CSS and 1KB in the HTML.
