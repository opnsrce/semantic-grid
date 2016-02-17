#Semantic Grid

A semantic CSS grid that uses mixins to apply column-related styles to HTML
elements.

#Table of Contents:
  - [Prerequisites](#prerequisites)
  - [See it in Action](#see-it-in-action)
  - [Installation](#installation)
  - [Configuring the Grid](#configuring-the-grid)
      - [Max Grid Width](#max-grid-width)
      - [Number of Columns](#number-of-columns)
      - [Gutter Width](#gutter-width)
  - [Configuring the Columns](#configuring-the-columns)
      - [Width](#width)
      - [Collapse Gutters](#collapse-gutters)
      - [Force Column Position](#force-column-position)
      - ["Push" or "Pull" a Column](#push-or-pull-a-column)
  - [Frequently-Asked Questions](#frequently-asked-questions)
      - [Why Use Mixins Instead of Classes?](#why-use-mixins-instead-of-classes)

# Prerequisites
  Semantic Grid was compiled using SASS 3.4.21

# See it in Action

An example page demonstrating _almost_ every feature of the grid can be found
[here](http://opnsrce.github.io/semantic-grid/examples/index.html).

# Installation

To install the semantic grid from npm:

``npm install semantic-grid``

## Configuring the Grid

The following aspects of the grid can be dynamically configured at run-time:

### Max Grid Width

__config option__: ``max-grid-width``
__default value:__ ``960px``

This is how large the grid should get to accommodate the content. Once the
screen's width exceeds this value, the grid will stop expanding and center
itself on the page.

```sass
//// Styles.scss

@import '_grid';

$grid-config: (
  // The max-width of the grid
  max-grid-width: 960px,
  ...
);

.my-grid-class {
  @include grid();
}
...
```

### Number of Columns

__config option__: ``num-columns``
__default value:__ ``12``

This is how many columns a single "row" can contain and it's the number that
is used when calculating column widths.

```sass
//// Styles.scss

@import '_grid';

$grid-config: (
  // Override the default number of columns
  num-columns: 16,
  ...
);

.my-grid-class {
  @include grid();
}
...
```

### Gutter Width

__config option__: ``gutter-width``
__default value:__ ``12px``

This is how many pixels of padding get added to the top, left, and bottom of
each column.

```sass
//// Styles.scss

@import '_grid';

$grid-config: (
  // Override the default gutter width
  gutter-width: 8px,
  ...
);

.my-grid-class {
  @include grid();
}
...
```

## Configuring the Columns

The following aspects of a column can be configured:

### Width

This parameter specifies how many column-widths the element being styled
should span. The value can range from 1 to the number specified in
``$num-columns``.

```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
  @include grid();
}

.my-first-column {
  @column(6); // Given a 12 column grid, this will span 50% of its container
}
...
```

### Collapse Gutters

__config option__: ``collapse``
__default value__: ``none``

This config option collapses one or more gutters on a given column by removing
the padding.

Possible values: ``none``, ``all``, ``top``, ``left``, ``bottom``

``right`` is not an option because columns do not have a right gutter.

If ``none`` or ``all`` are specified, any following collapse options will
be ignored.


```sass
//// Styles.scss

@import '_grid';

.my-grid-class {
  @include grid();
}

.my-first-column {
  @column(6, (collapse: top bottom)); // Remove top and bottom padding
}
...
```

### Force Column Position

__config option__: ``force``
__default value__: ``none``

There are times where a column needs to be rendered in a way it would otherwise
not given the surrounding markup. For example to force a three-column layout
where the last two columns are aligned in the center of the page and to the
right, respectively but the sum of all the column's widths don't add up to the
number of columns in the grid. In this example, we have three columns, each
two column-widths wide that make up the parts of a company slogan:

```html
<div class = "page__company-slogan">
  <div class="slogan slogan--phrase-1">
    <span class="sample-content"></span>
  </div>
  <div class="slogan slogan--phrase-2">
    <span class="sample-content"></span>
  </div>
  <div class="slogan slogan--phrase-3">
    <span class="sample-content"></span>
  </div>
</div>
```

```sass
.page__company-slogan {
  @extend %clearfix;
  clear: both;
  display: block;
  height: 50px;
  .slogan {
    @include column(2);
    height: 100%;
    &--phrase-2 {
      @include column(2, (force: 'right'));
    }
    &--phrase-3  {
      @include column(2, (force: 'center'));
    }
  }
}
...
```

Here, the second part of the slogan will be forced to the right side of the
page, the first part will be on the left, and the third part will be on the
right.

### A Note About Source Order
The order in which the slogan phrases are shifted right and
center is intentional. Placing floated elements (those on the left and right)
before non-floated elements (centered) forces them to appear at the same
position vertically and allows the non-floated element to float _up_ and fill
the remaining space between the two floated elements. In order to center a
column between other non-centered columns, the non-centered columns need to
appear in the source code before the centered column does.

### "Push" or "Pull" a Column

__config option__: ``push`` and ``pull``
__default value__: ``0``

The ``push`` and ``pull`` config options can be used to force a column away
from the left (``push``) and right (``pull``) side of the pages a given number
of column widths without resorting to inserting empty column-related markup
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
  @column(6, (push: 6));
}
...
```

# Frequently-Asked Questions
## Why Use Mixins Instead of Classes?

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
``:last-of-type``, and ``[class*='col']`` to dynamically control styling of
columns and rows without having to add additional classes like ``.col--last``.

3. It clearly shows what classes affect what markup. It is easy to see what's
styling a block of HTML just by looking at the class list.

4. Using the ``class`` attribute makes it easy to alter the layout of the page
using JavaScript.

Unfortunately, it also has several __disadvantages__:

1. Column widths cannot easily be stored in reusable variables without using
some kind of templating engine. This makes maintenance rather tedius if there
is a lot of markup that shares the same ``col-`` classes.

2. It's not semantic. Classes like ``col-sm-5`` don't do anything to help
_describe_ the kind of data the markup represents. Semantic markup means
using CSS for styling and HTML for describing the data. Pure style-based
class names should be kept to a minimum if not eleminated all together.

3. It causes code-bloat in the HTML and the CSS. The class-based approach to
grids causes a lot of extra markup (in the form of wrapper ``<div>``s),
larger HTML files (due to the length of class names), and larger CSS files
(due to every grid-related class having to be generated up-front instead of
as-needed). When this project was converted from a class-based to a mixin-based
approach, combined file size was reduced by approximately 3KB.