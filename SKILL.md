---
name: typst
description: This skill should be used when the user asks to "write a Typst document", "create a Typst template", "style a document with Typst", "format text in Typst", "add math equations in Typst", "create a table in Typst", "set up page layout in Typst", "write a Typst function", "use set rules or show rules", "import a Typst package", or any task involving writing or editing `.typ` files.
license: MIT
metadata:
  version: 0.1.0
  author: BANG404
---

# Typst

Typst is a markup-based typesetting system designed as a modern alternative to LaTeX. Documents are written in `.typ` files using a combination of markup syntax, set/show rules, and a built-in scripting language.

## Three Modes

Typst has three syntactical modes:

| Mode   | How to enter                          | Default in     |
|--------|---------------------------------------|----------------|
| Markup | Default                               | `.typ` files   |
| Code   | Prefix with `#`                       | Anywhere       |
| Math   | Surround with `$...$`                 | Anywhere       |

Once inside code mode via `#`, no further `#` is needed until returning to markup.

## Core Markup Syntax

```typst
= Heading Level 1
== Heading Level 2

*bold text*
_italic text_
`inline code`

- Bullet item
+ Numbered item

$ x^2 + y^2 = z^2 $   // inline math
$ x^2 + y^2 = z^2 $   // block math (spaces inside $)
```

## Functions and Content Blocks

Call functions with `#` in markup mode:

```typst
#image("photo.jpg", width: 70%)

#figure(
  image("chart.png", width: 80%),
  caption: [A bar chart showing results.],
) <fig-chart>

// Reference with @
See @fig-chart for details.
```

Content blocks (markup in `[...]`) can be passed as function arguments:

```typst
#text(fill: red)[This is red text]
#align(center)[Centered content]
```

## Set Rules — Configure Defaults

Set rules configure default properties for elements:

```typst
#set text(font: "New Computer Modern", size: 12pt)
#set heading(numbering: "1.1")
#set page(paper: "a4", margin: 2cm)
#set par(justify: true, leading: 0.8em)
```

Set rules are scoped: a top-level rule applies to the whole document; one inside a block `{...}` or `[...]` applies only within that block.

## Show Rules — Transform Elements

Show rules redefine how elements appear:

```typst
// Show-set rule: apply extra styling to element type
#show heading: set text(fill: navy)

// Transformational show rule: full control
#show heading: it => block[
  #it.body — Level #it.depth
]

// Apply to specific level only
#show heading.where(level: 1): set align(center)

// Apply to text matches
#show "Typst": smallcaps
#show regex("\b\d+\b"): it => text(fill: blue, it)

// Everything rule — wraps entire document
#show: doc => template(doc)
```

## Variables and Functions

Define variables and custom functions with `let`:

```typst
#let title = "My Paper"
#let accent = rgb("#005b99")

// Custom function
#let note(body, color: yellow) = {
  rect(fill: color, inset: 8pt)[#body]
}

#note[Important!]
#note(color: red)[Critical!]
```

## Templates

Wrap entire documents with an everything show rule:

```typst
// In conf.typ
#let conf(title: "", authors: (), doc) = {
  set page(paper: "a4", margin: 2.5cm)
  set text(font: "Libertinus Serif", size: 11pt)
  set par(justify: true)

  align(center)[
    #text(size: 18pt, weight: "bold")[#title]
  ]

  doc
}

// In main.typ
#import "conf.typ": conf

#show: conf.with(title: "My Document")

= Introduction
...
```

For Typst Universe packages, use `.with()` style:

```typst
#import "@preview/charged-ieee:0.1.3": ieee
#show: ieee.with(title: "Paper Title", ...)
```

## Math Typesetting

```typst
// Inline
The equation $E = m c^2$ is famous.

// Block (space after/before $)
$ sum_(i=0)^n i = (n(n+1))/2 $

// Subscript and superscript
$x_1^2$,  $x_(i+1)$

// Fractions (automatic with /)
$(a + b) / (c + d)$

// Vectors and matrices
$ vec(1, 2, 3), mat(1, 0; 0, 1) $

// Symbols
$alpha, beta, pi, nabla, arrow.r$
```

## Modules and Packages

```typst
// Import from local file
#import "utils.typ": my-func, helper

// Import community package from Typst Universe
#import "@preview/cetz:0.3.4": canvas, draw

// Include file (inserts its content)
#include "chapter1.typ"
```

## Control Flow

```typst
// Conditional
#if count > 5 [Many items] else [Few items]

// For loop
#for item in items [
  - #item
]

// While loop
#let i = 0
#while i < 3 {
  i += 1
  [Step #i \ ]
}
```

## Common Page Setup

```typst
#set page(
  paper: "a4",           // or "us-letter"
  margin: (top: 2cm, bottom: 2cm, left: 2.5cm, right: 2.5cm),
  header: align(right)[My Document],
  footer: align(center)[#context counter(page).display()],
  numbering: "1",
  columns: 2,
)
```

## Bibliography

```typst
// Cite using same syntax as labels
As shown in @smith2023, ...

// Render bibliography at end
#bibliography("refs.bib")
#bibliography("refs.yml")  // Hayagriva format
```

## Additional Resources

### Reference Files

Consult these for in-depth coverage:

- **`references/syntax.md`** — Full syntax tables for markup, math, and code modes; escape sequences; identifier rules
- **`references/styling.md`** — Complete set/show rule patterns and selector types
- **`references/scripting.md`** — Variables, destructuring, loops, modules, operators, packages
- **`references/math.md`** — Math mode functions, symbols, alignment, and notation
- **`references/templates.md`** — Building reusable templates, named arguments, and `.with()` pattern
