# Typst Styling Reference

## Set Rules

Set rules configure default properties for element functions. Written as `set element(params...)` in code, or `#set element(params...)` in markup.

Only *optional* parameters of element functions can be used in set rules.

### Scope

- Top-level set rule: applies until end of file
- Inside a block `{...}` or `[...]`: scoped to that block

```typst
// Applies to whole document
#set text(font: "Libertinus Serif", size: 11pt)

// Scoped to one list only
This list is affected: #[
  #set list(marker: [--])
  - Dash
]

This one is not:
- Bullet
```

### Set-if Rule

Apply a set rule conditionally:

```typst
#let task(body, critical: false) = {
  set text(red) if critical
  [- #body]
}

#task(critical: true)[Food today?]
#task(critical: false)[Work deadline]
```

### Common Set Rules

```typst
// Text
#set text(
  font: "New Computer Modern",
  size: 12pt,
  fill: rgb("#333333"),
  lang: "en",
  weight: "regular",   // "bold", int 100-900
  style: "normal",     // "italic", "oblique"
)

// Headings
#set heading(numbering: "1.1.", outlined: true)

// Page
#set page(
  paper: "a4",         // "us-letter", "a5", etc.
  margin: 2cm,         // or (top: 2cm, bottom: 2cm, left: 2.5cm, right: 2.5cm)
  header: align(right)[Running head],
  footer: context counter(page).display("1 of 1", both: true),
  numbering: "1",
  columns: 1,
  flipped: false,
)

// Paragraph
#set par(
  justify: true,
  leading: 0.8em,
  first-line-indent: 1.2em,
  spacing: 1.2em,
)

// List
#set list(marker: ([•], [–], [◦]))
#set enum(numbering: "1.a.")
```

---

## Show Rules

Show rules transform or replace elements. Written as `show selector: transformation`.

### Show-Set Rule (most common)

Apply extra set rules to specific elements:

```typst
#show heading: set text(fill: navy)
#show link: set text(fill: blue)
#show heading.where(level: 1): set align(center)
```

### Transformational Show Rule

Full control over rendering. Function receives the element (`it` by convention):

```typst
#show heading: it => block[
  \~ #emph(it.body) #counter(heading).display() \~
]

// With set rules for composability
#show heading: set align(center)
#show heading: set text(font: "Inria Serif")
#show heading: it => block[
  \~ #emph(it.body) #counter(heading).display() \~
]
```

The `it` parameter has fields matching the element function's parameters.

### Selectors

| Selector                          | Description                                  |
|-----------------------------------|----------------------------------------------|
| `heading`                         | All headings                                 |
| `heading.where(level: 1)`         | Only level-1 headings                        |
| `"Text"`                          | Literal text matches                         |
| `regex("\\w+")`                   | Regex pattern matches                        |
| `<label-name>`                    | Elements with that label                     |
| `show: rest => ..`                | Everything rule (whole document)             |

### Everything Rule (Templates)

Passes entire remaining document to a function:

```typst
#let my-template(doc) = [
  #set text(font: "Inria Serif")
  #doc
]

#show: my-template
```

Or with a closure for extra arguments:

```typst
#show: doc => conf(
  title: "My Paper",
  doc,
)
```

### Substitution Show Rules

Replace element with literal string or content:

```typst
#show "Project": smallcaps
#show "badly": "great"
#show "Typst": [_Typst_]
```

---

## Selectors with `.where()`

Filter elements by field values:

```typst
#show heading.where(level: 1): set text(size: 18pt)
#show heading.where(level: 2): set text(size: 14pt)
#show figure.where(kind: image): set align(center)
```

---

## Precedence

Show rules and set rules are applied from bottom to top (later rules take precedence). Set rules inside transformational show rules cannot be overridden by later show-set rules — keep them separate for composability.

---

## Context-Aware Styling

Use `context` to access values that depend on document state:

```typst
// Current page number
#context counter(page).display()

// Current text language
#context text.lang

// Conditional on page number
#context {
  if counter(page).get().first() == 1 {
    [First page!]
  }
}
```

---

## Practical Examples

### Academic Paper Style

```typst
#set page(paper: "a4", margin: 2.5cm)
#set text(font: "Libertinus Serif", size: 11pt)
#set par(justify: true, leading: 0.65em)
#set heading(numbering: "1.1")

#show heading.where(level: 1): set text(size: 14pt, weight: "bold")
#show heading.where(level: 2): set text(size: 12pt, style: "italic")
#show link: set text(fill: blue)
```

### Custom Callout Box

```typst
#let callout(title: "Note", color: blue, body) = block(
  stroke: color,
  inset: 10pt,
  radius: 4pt,
)[
  #text(fill: color, weight: "bold")[#title] \
  #body
]

#callout(title: "Warning", color: red)[
  This is a warning.
]
```
