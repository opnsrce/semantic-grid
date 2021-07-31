# Semantic Grid

A semantic CSS grid that uses mixins to apply column-related styles to HTML
elements.

# Table of Contents:

- [Semantic Grid](#semantic-grid)
- [Prerequisites](#prerequisites)
- [See it in Action](#see-it-in-action)
- [Installation](#installation)
  * [Configuring the Grid](#configuring-the-grid)
    + [The Number of Columns](#the-number-of-columns)
    + [Column Gutter size](#column-gutter-size)
    + [Row Gutter size](#row-gutter-size)
  * [Configuring the Columns](#configuring-the-columns)
    + [Span](#span)
    + [Start](#start)
    + ["Push" or "Pull" a Column](#-push--or--pull--a-column)
  * [Supported Browsers](#supported-browsers)
  * [Frequently-Asked Questions](#frequently-asked-questions)
    + [Why Use Mixins Instead of Classes?](#why-use-mixins-instead-of-classes-)

# Prerequisites

Semantic Grid was compiled using SASS 1.37.0 compiled with dart2js 2.13.4

# See it in Action

An example page demonstrating _almost_ every feature of the grid can be found
[here](http://opnsrce.github.io/semantic-grid/examples/index.html).

# Installation

To install the semantic grid from npm:

``npm install semantic-grid``

## Configuring the Grid

The following aspects of the grid can be dynamically configured at dev-time:

### The Number of Columns

This is how many columns per row the grid should contain (default: 12).

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
    // Create a 6 column grid
    @include grid(6); 
}
...
```

### Column Gutter size

This is how wide the gap should be between the columns of the grid (default: 0).

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
    // Create a 6 column grid with 12px between the columns
    @include grid(6, 12px); 
}
...
```

### Row Gutter size

This is how wide the gap should be between the rows of the grid (default: 0).

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
    // Create a 6 column grid with 12px between the columns and 12px between 
    // the rows
    @include grid(6, 12px, 12px); 
}
...
```

## Configuring the Columns

The following aspects of a column can be configured:

### Span

This parameter specifies how many columns of the grid it should span (default:
1).

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
  @include grid();
}

.my-first-column {
  // Given a 12 column grid, this will span 50% of its container
  @column(6); 
}
...
```

### Start

Where the column should start within the grid. Use this param to "push" or
"pull" a column. Postive integers start counting from the left most column.
Negative integers start counting from the right most column (default: null).

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
  @include grid();
}

.my-first-column {
    // Given a 12 column grid, this will span 50% of its container starting on 
    // the second column
  @column(6, 2); 
}
...
```

### "Push" or "Pull" a Column

The `$start` param in the `column` mixin can be used to force a column away
from the left (`push`) and right (`pull`) side of the pages a given number
of columns without resorting to inserting empty column-related markup
before or after the affected column.

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
  @include grid();
}

.my-pushed-column {
  // This column will be 50% of its container width (in a 12 column grid) and
  // 50% from the left of its container.
  @column(6, 6);
}
...
```

## Supported Browsers

See [Can I Use](https://caniuse.com/css-grid) for an up-to-date list of
supported browsers.

## Frequently-Asked Questions
### Why Use Mixins Instead of Classes?

A lot of well-known frameworks (Foundation5 and Bootstrap come to mind) use
classes to denote column widths:

```html
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

1. It allows control over how an element will look at different screen widths
just by adding a series of size-related classes.

2. It allows for control over styling using CSS pseudo-selectors like
`:last-of-type`, and `[class*='col']` to dynamically control styling of
columns and rows without having to add additional classes like `.col--last`.

3. It clearly shows what classes affect what markup. It is easy to see what's
styling a block of HTML just by looking at the class list.

4. Using the `class` attribute makes it easy to alter the layout of the page
using JavaScript.

Unfortunately, it also has several __disadvantages__:

1. Column widths cannot easily be stored in reusable variables without using
some kind of templating engine. This makes maintenance rather tedius if there
is a lot of markup that shares the same `col-` classes.

2. It's not semantic. Classes like `col-sm-5` don't do anything to help
_describe_ the kind of data the markup represents. Semantic markup means
using CSS for styling and HTML for describing the data. Pure style-based
class names should be kept to a minimum if not eleminated all together.

3. It causes code-bloat in the HTML and the CSS. The class-based approach to
grids causes a lot of extra markup (in the form of wrapper `<div>`s),
larger HTML files (due to the length of class names), and larger CSS files
(due to every grid-related class having to be generated up-front instead of
as-needed). When this project was converted from a class-based to a mixin-based
approach, combined file size was reduced by approximately 3KB.