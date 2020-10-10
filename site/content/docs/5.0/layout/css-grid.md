---
layout: docs
title: CSS Grid
description: Learn how to enable, use, and customize our alternate layout system built on CSS Grid with examples and code snippets.
status: experimental
group: layout
toc: true
---

{{< callout info >}}
- How to generate or create dynamic col classes above default of 12? Sass only?
- Subgrid?
- Shorthand for 2x2, 3x3, etc grids? .grid-2x12 (2 rows 12 cols)?
- Wish we could do `cols="6"` and `[cols] { --columns: attr(cols number); }` but it's not supported.
{{< /callout >}}

## Overview

Bootstrap's default grid system represents the culmination of over a decade of CSS layout techniques, tried and tested by millions of people. But, it was also created without so many of the modern CSS features and techniques that we're seeing in browsers—features like the new CSS Grid.

With Bootstrap 5, we've added the option to enable a separate grid system that's built on CSS Grid, but with a Bootstrap twist. You still get classes you can apply on a whim to build responsive layouts, but with a different approach under the hood.

## How it works

- Disable the default grid system by setting `$enable-grid-classes: false`.
- Enable the CSS Grid by setting `$enable-cssgrid: true` and recompiling your Sass.
- Replace instances of `.row` with `.grid`. `.grid`s set `display: grid` and create a `grid-template` that you build on with your HTML.
- The number of columns and the width of the gutters are set via CSS variables on the `.grid`, which means you can customize those values on the fly: `--columns` and `--gap`.

## Key differences

Compared to the default grid system:

- Flex utilities won't affect grid columns. Note that columns can still be flex containers.
- There's no `padding` on columns as gutters function more like margins.
- Unlike `.row`s, `.grid`s have no negative margins.
- Margin utilities cannot be used to change the grid gutters. See the customizing section.
- Grid gutters are applied horizontally and vertically by default. See the customizing section.
- Inline and custom styles should be viewed as replacements for modifier classes (e.g., `style="--columns: 3;"` vs `class="row-cols-3"`).

## Examples

### Three columns

Three equal width columns across all viewports and devices can be created by using the `.g-col-4` classes.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div class="g-col-4">.g-col-4</div>
  <div class="g-col-4">.g-col-4</div>
  <div class="g-col-4">.g-col-4</div>
</div>
{{< /example >}}

### No column classes

When there are no classes on the grid items (the immediate children of a `.grid`), each one defaults to one column's width.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
  <div>...</div>
</div>
{{< /example >}}

### Responsive

Use responsive classes to adjust your layout across viewports. Here we start with two columns on the narrowest viewports, and then grow to three columns on medium viewports and above.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div class="g-col-6 g-col-md-4">.g-col-6 .g-col-md-4</div>
  <div class="g-col-6 g-col-md-4">.g-col-6 .g-col-md-4</div>
  <div class="g-col-6 g-col-md-4">.g-col-6 .g-col-md-4</div>
</div>
{{< /example >}}

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>
</div>
{{< /example >}}

## Wrapping

Grid items automatically wrap to the next line when there's no more room horizontally. Note that the `grid-gap` applies to horizontal and vertical gutters between grid items.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>

  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>
</div>
{{< /example >}}

## Starts

Start classes replace our default grid's offsets, but they're not the same. CSS Grid creates columns through styles that tell browsers to "start at this column" and "end at this column." Those properties are `grid-column-start` and `grid-column-end`. Start classes are shorthand for the former.

By comparison, our default grid's offsets use `margin-left` to nudge a column.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div class="g-col-3 g-start-2">.g-col-3 .g-start-2</div>
  <div class="g-col-4 g-start-6">.g-col-4 .g-start-6</div>
</div>
{{< /example >}}

## Auto columns

Any immediate child of `.grid` will automatically be sized to one column.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
</div>
{{< /example >}}

This behavior can be mixed with grid column classes.

{{< example class="bd-example-cssgrid" >}}
<div class="grid">
  <div class="g-col-6">.g-col-6</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
  <div>1</div>
</div>
{{< /example >}}

## Nesting

Because we set the column count, row count, and gutter size on the `.grid` class, it's not automatically inherited by every nested instance of `.grid`. As such, you'll need to specifically tell the `.grid` class to `inherit` those options, or redeclare them.

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="--columns: 3;">
  <div>
    Auto-column
    <div class="grid" style="--columns: inherit;">
      <div>Auto-column</div>
      <div>Auto-column</div>
    </div>
  </div>
  <div>
    Auto-column
    <div class="grid">
      <div class="g-col-6">6 of 12</div>
      <div class="g-col-4">4 of 12</div>
      <div class="g-col-2">2 of 12</div>
    </div>
  </div>
  <div>Auto-column</div>
</div>
{{< /example >}}

## Customizing

Customize the number of columns, the number of rows, and the width of the gutters with local CSS variables.

{{< bs-table "table" >}}
| Variable | Default value | Description |
| --- | --- | --- |
| `--rows` | `1` | The number of rows in your grid template |
| `--columns` | `12` | The number of columns in your grid template |
| `--gap` | `1.5rem` | The size of the gap between columns (vertical and horizontal) |
{{< /bs-table >}}

Because these CSS variables are local to the `.grid` class, you can override them whenever you like, as many times as you like, even across media queries.

### No grid classes

Immediate children elements of `.grid` are grid items, so they'll be sized without explicitly adding a `.g-col` class.

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="--columns: 3;">
  <div>Auto-column</div>
  <div>Auto-column</div>
  <div>Auto-column</div>
</div>
{{< /example >}}

### Columns and gutters

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="--columns: 4; --gap: 5rem;">
  <div class="g-col-2">.g-col-2</div>
  <div class="g-col-2">.g-col-2</div>
</div>
{{< /example >}}

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="--columns: 10; --gap: 1rem;">
  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-4">.g-col-4</div>
</div>
{{< /example >}}

### Adding rows

Adding more rows and changing the placement of columns:

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="--rows: 3; --columns: 3;">
  <div>Auto-column</div>
  <div class="g-start-2" style="grid-row: 2">Auto-column</div>
  <div class="g-start-3" style="grid-row: 3">Auto-column</div>
</div>
{{< /example >}}

### Gutters

Change the vertical gutters only by modifying the `grid-row-gap`. Note that we use `grid-gap` on `.grid`s, but `grid-row-gap` and `grid-col-gap` can be modified as needed.

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="grid-row-gap: 0;">
  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>

  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>
</div>
{{< /example >}}

Because of that, you can have different vertical and horizontal gutters with `grid-gap`, which can take a single value (all sides) or a pair of values (vertical and horizontal).

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="grid-gap: .25rem 1rem;">
  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>

  <div class="g-col-6">.g-col-6</div>
  <div class="g-col-6">.g-col-6</div>
</div>
{{< /example >}}

## Sass

One limitation of the CSS Grid is that our default classes are still generated by two Sass variables, `$grid-columns` and `$grid-gutter-width`. You have two two options here:

- Modify those default Sass variables and recompile your CSS.
- Or, use inline or custom styles to augment the provided classes.

For example, you can customize the column count and gutter, and then size your "columns" with inline styles.

{{< example class="bd-example-cssgrid" >}}
<div class="grid" style="--columns: 18; --gap: .5rem;">
  <div style="grid-column: span 14;">14 columns</div>
  <div class="g-col-4">.g-col-4</div>
</div>
{{< /example >}}
