# Econsultancy CSS Style Guide

These guidelines strongly encourage the use of existing, common, sensible
patterns. This is a living document and new ideas are always welcome. Please
contribute.

## Table of contents

1. [General principles](#general-principles)
2. [Whitespace](#whitespace)
3. [Naming](#naming)
4. [Comments](#comments)
5. [Format](#format)
6. [Practical example](#example)

<a name="general-principles"></a>
## 1. General principles

> "Part of being a good steward to a successful project is realizing that
> writing code for yourself is a Bad Idea™. If thousands of people are using
> your code, then write your code for maximum clarity, not your personal
> preference of how to get clever within the spec." - Idan Gazit

* You are not a human code compiler/compressor, so don't try to be one.
* All code in any code-base should look like a single person typed it, no
  matter how many people contributed.
* Strictly enforce the agreed-upon style.
* If in doubt when deciding upon a style, use existing, common patterns.

<a name="whitespace"></a>
## 2. Whitespace

Only one style should exist across the entire source of your code-base. Always
be consistent in your use of whitespace. Use whitespace to improve
readability.

* _Never_ mix spaces and tabs for indentation.
* Use soft indents (spaces).
* Use 2 characters per indentation level


<a name="naming"></a>
## 3. Naming

We use a BEM [Block Element Modifier](http://bem.github.io/bem-method/html/all.en.html) inspired naming convetion, which although verbose helps in:

* easily identifing UI elements, their state and variant in HTML
* better maintanace through more readable stylesheets
* avoiding cascade pitfalls


<a name="comments"></a>
## 4. Comments

Well commented code is extremely important. Take time to describe components,
how they work, their limitations, and the way they are constructed. Don't leave
others in the team guessing as to the purpose of uncommon or non-obvious
code.

Comment style should be simple and consistent within a single code base.

* Place comments on a new line above their subject.
* Keep line-length to a sensible maximum, e.g., 80 columns.
* Make liberal use of comments to break CSS code into discrete sections.
* Use "sentence case" comments and consistent text indentation.

Note that all the CSS in the deathstar is written in SASS and so mostly
we use the alternative comment syntax. CSS comments can still be used
for purposes of development, licences or similar.

We use the three patterns below to separate stylesheets into sections

We use the following comment header styles:

```scss

A partial header, use only on top of the file. Feeling like you need another one?
Make a new partial. Start CSS after two empty new lines.

// ----------------------------------------------------------------------------
// Page
// Optional page/stylesheet description.
// ----------------------------------------------------------------------------

A section or partial heading.
Make sure there is two empty lines above and one after.

// @ SECTION
// --------------------------------------------------------------------------


```

Example of a basic comment
```scss
// This is a selector
.selector { color: #fff; }
```

<a name="format"></a>
## 5. Format

The chosen code format must ensure that code is: easy to read; easy to clearly
comment; minimizes the chance of accidentally introducing errors; and results
in useful diffs and blames.

* Use one discrete selector per line in multi-selector rulesets.
* Include a single space before the opening brace of a ruleset.
* Include one declaration per line in a declaration block.
* Use one level of indentation for each declaration.
* Use lowercase and shorthand hex values, e.g., `#aaa`.
* Use hex values for colors. If rgba is needed provide a fallback in rgba and
a comment with the hex value. This is done so it is easier to remove these
declarations in the future.
* Use double quotes consistently.
* Quote attribute values in selectors, e.g., `input[type="checkbox"]`.
* _Where allowed_, avoid specifying units for zero-values, e.g., `margin: 0`.
* Include a space after each comma in comma-separated property or function
  values.
* Include a semi-colon at the end of the last declaration in a declaration
  block.
* Place the closing brace of a ruleset in the same column as the first
  character of the ruleset.
* Separate each ruleset by a blank line.

```scss
.selector-1,
.selector-2,
.selector-3[type="text"] {
  -webkit-box-sizing: border-box;
  -moz-box-sizing: border-box;
  box-sizing: border-box;
  display: block;
  font-family: helvetica, arial, sans-serif;
  color: #333;
  background: #fff;
  background: linear-gradient(#fff, rgba(0, 0, 0, 0.8));
}

.selector-a,
.selector-b {
  padding: 10px;
  color: rgb(0, 0, 0, 0.3);
  color: rgba(0, 0, 0, 0.3); // #000
}
```

#### Declaration order

If faced with a selector with a large number of properites consider
breaking them up by category. This can differ widely from selector to selector,
but always good candidates are positioning, box-model and colors.

A usual case

```scss
.selector {
  // Positioning
  position: absolute;
  z-index: 10;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;

  // Display & Box Model
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  width: 100px;
  height: 100px;
  padding: 10px;
  border: 10px solid #333;
  margin: 10px;

  // Other
  background: #000;
  color: #fff;
  font-family: sans-serif;
  font-size: 16px;
  text-align: right;
}
```

#### Exceptions and slight deviations

Single declarations with up to 3 properties can use a slightly different,
single-line format. In this case, a space should be included after the opening
brace and before the closing brace.

```css
.selector-1 { width: 10%; }
.selector-2 { width: 20%; }
.selector-3 { width: 30%; }
```

Long, comma-separated property values - such as collections of gradients or
shadows - can be arranged across multiple lines in an effort to improve
readability and produce more useful diffs.

```css
.selector {
  background-image:
    linear-gradient(#fff, #ccc),
    linear-gradient(#f3c, #4ec);
  box-shadow:
    1px 1px 1px #000,
    2px 2px 1px 1px #ccc inset;
}
```

### SASS

* Limit nesting to 1 level deep. Reassess any nesting more than 2 levels deep.
  This prevents overly-specific CSS selectors.
* Avoid large numbers of nested rules. Break them up when readability starts to
  be affected. Preference to avoid nesting that spreads over more than 20
  lines.
* Always place `@extend` statements on the first lines of a declaration
  block.
* Where possible, group `@include` statements at the top of a declaration
  block, after any `@extend` statements.
* Consider prefixing custom functions with `x-` or another namespace. This
  helps to avoid any potential to confuse your function with a native CSS
  function, or to clash with functions from libraries.

```scss
.selector-1 {
  @extend .other-rule;
  @include clearfix();
  @include box-sizing(border-box);
  width: x-grid-unit(1);
  // other declarations
}
```


<a name="example"></a>
## 6. Practical example

An example of various conventions.

```scss

// ----------------------------------------------------------------------------
// Grid
// ----------------------------------------------------------------------------


// Column layout with horizontal scroll.
//
// This creates a single row of full-height, non-wrapping columns that can
// be browsed horizontally within their parent.
//
// Example HTML:
//
// <div class="grid">
//   <div class="grid__cell grid__cell--3"></div>
//   <div class="grid__cell grid__cell--3"></div>
//   <div class="grid__cell grid__cell--3"></div>
// </div>
//


// @ GRID CONTAINER
// --------------------------------------------------------------------------

.grid {
  height: 100%;
  // Remove inter-cell whitespace
  font-size: 0;
  // Prevent inline-block cells wrapping
  white-space: nowrap;
}


// @ GRID CELLS
// --------------------------------------------------------------------------

.grid__cell {
  position: relative;
  display: inline-block;
  overflow: hidden;
  box-sizing: border-box;
  height: 100%;
  padding: 0 10px;
  border: 2px solid #333;
  vertical-align: top;
  white-space: normal;

  // Reset font-size
  font-size: 16px;
}

.grid__cell--highlight { background-color: #fffdec; }

.grid__cell--1 { width: 10%; }
.grid__cell--2 { width: 20%; }
.grid__cell--3 { width: 30%; }
.grid__cell--4 { width: 40%; }
.grid__cell--5 { width: 50%; }

.grid__cell---detail,
.grid__cell---important {
  border-width: 4px;
}

```


## License

This document is a fork of
_Principles of writing consistent, idiomatic CSS_ by Nicolas Gallagher
licensed under the [Creative Commons Attribution 3.0 Unported
License](http://creativecommons.org/licenses/by/3.0/).
