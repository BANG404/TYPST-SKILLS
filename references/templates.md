# Typst Templates Reference

## What Templates Are

Templates in Typst are functions that wrap the entire document using an "everything" show rule. They apply consistent styling, layout, and structure across the whole document.

## Basic Template Pattern

```typst
// template.typ
#let my-template(doc) = {
  // Page setup
  set page(paper: "a4", margin: 2.5cm)
  // Text defaults
  set text(font: "Libertinus Serif", size: 11pt)
  set par(justify: true)
  // Heading styles
  show heading.where(level: 1): set text(size: 16pt)
  // Render document
  doc
}

// main.typ
#import "template.typ": my-template
#show: my-template

= My Document
...
```

## Template with Named Arguments

Use named parameters with defaults for configurable templates:

```typst
// conf.typ
#let conf(
  title: "",
  authors: (),
  abstract: [],
  doc,
) = {
  set page(paper: "us-letter", margin: auto, columns: 2)
  set text(font: "Libertinus Serif", size: 11pt)
  set par(justify: true)

  // Title block (full width, floating)
  place(
    top + center,
    float: true,
    scope: "parent",
    clearance: 2em,
    {
      align(center, text(size: 17pt, weight: "bold")[#title])

      // Author grid (max 3 columns)
      let ncols = calc.min(authors.len(), 3)
      grid(
        columns: (1fr,) * ncols,
        row-gutter: 24pt,
        ..authors.map(a => align(center)[
          #a.name \
          #a.affiliation \
          #link("mailto:" + a.email)
        ]),
      )

      par(justify: false)[*Abstract* \ #abstract]
    }
  )

  doc
}
```

### Using the Template (Closure Pattern)

```typst
#import "conf.typ": conf

#show: doc => conf(
  title: [A Fluid Dynamic Model for Glacier Flow],
  authors: (
    (name: "Alice Smith", affiliation: "MIT", email: "alice@mit.edu"),
    (name: "Bob Jones", affiliation: "Stanford", email: "bob@stanford.edu"),
  ),
  abstract: lorem(80),
  doc,
)

= Introduction
#lorem(90)
```

### Using the Template (.with() Pattern)

Preferred style for Typst Universe packages:

```typst
#import "conf.typ": conf

#show: conf.with(
  title: [My Paper],
  authors: (...),
  abstract: lorem(80),
)

= Introduction
```

The `.with()` method pre-populates named arguments. The document body is automatically passed as the last positional argument by the show rule.

## Variables in Templates

Store reusable values to avoid repetition:

```typst
#let accent-color = rgb("#005b99")
#let code-font = "JetBrains Mono"

#let template(doc) = {
  set text(font: "Libertinus Serif")
  show heading: set text(fill: accent-color)
  show raw: set text(font: code-font)
  doc
}
```

## Scoping Rules in Templates

Set and show rules inside a function body apply to all content rendered within that function:

```typst
#let scoped-template(doc) = [
  // Content block: can use # for set/show
  #set text(red)
  #doc
]

#let code-template(doc) = {
  // Code block: set/show without #
  set text(red)
  doc
}
```

Both are equivalent. Code blocks are more common for complex templates.

## Separate File Structure

Standard project layout:

```
project/
├── main.typ          # Document content
├── template.typ      # Template definition
├── refs.bib          # Bibliography
└── figures/
    └── chart.png
```

`main.typ`:
```typst
#import "template.typ": template

#set document(title: "My Paper", author: "Alice")

#show: template.with(title: "My Paper")

= Introduction
...
```

## Typst Universe Packages

Install community templates via package import:

```typst
// Academic papers
#import "@preview/charged-ieee:0.1.3": ieee
#show: ieee.with(title: "...", authors: (...))

// Presentation slides
#import "@preview/touying:0.6.1": *
#show: metropolis-theme

// CV/Resume
#import "@preview/modern-cv:0.8.0": *
#show: resume.with(...)
```

Browse at: https://typst.app/universe

## Document Metadata

Set PDF/HTML metadata using `set document`:

```typst
#set document(
  title: "My Document Title",
  author: "Alice Smith",
  date: datetime.today(),
  keywords: ("typst", "tutorial"),
)
```

## Page Headers and Footers

```typst
#set page(
  header: context {
    // Access current section heading
    let headings = query(
      selector(heading.where(level: 1)).before(here())
    )
    if headings.len() > 0 {
      align(right, headings.last().body)
    }
  },
  footer: context {
    align(center, counter(page).display("1 / 1", both: true))
  },
)
```

## Multi-Column Layout

```typst
// Whole document in columns
#set page(columns: 2)

// Single-section in columns
#columns(2)[
  Left column content and right column content will
  flow automatically.
]

// Column break
#colbreak()
```

## Cover Page / Title Page

```typst
#let title-page(title, authors) = {
  set page(header: none, footer: none, numbering: none)
  align(center + horizon)[
    #text(size: 24pt, weight: "bold")[#title]
    #v(2em)
    #authors.join(", ")
  ]
  pagebreak()
}
```
