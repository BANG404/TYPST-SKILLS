# Typst Scripting Reference

## Expressions

Embed code into markup with `#`. End an expression with `;` if the next character would continue it but should be text:

```typst
#emph[Hello]
#emoji.face
#"hello".len()
#(1 + 2)  // parentheses for binary operators
```

## Blocks

**Code block** `{ ... }` — multiple statements, results joined:

```typst
#{
  let a = [from]
  let b = [*world*]
  [hello ]
  a + [ the ] + b
}
```

**Content block** `[...]` — markup as a value:

```typst
#let greeting = [Hello, *world*!]
#greeting
```

## Variables and Functions

```typst
// Variable binding
#let name = "Typst"
#let count = 42
#let flag = true

// Custom function
#let my-add(x, y) = x + y
#my-add(2, 3)

// Function with named parameter and default
#let note(body, color: yellow) = {
  rect(fill: color, inset: 8pt)[#body]
}
#note(color: red)[Warning!]
```

## Destructuring

```typst
// Array destructuring
#let (x, y) = (1, 2)
#let (a, .., b) = (1, 2, 3, 4)  // .. collects rest

// Discard elements
#let (_, y, _) = (1, 2, 3)

// Dictionary destructuring
#let books = (Shakespeare: "Hamlet", Homer: "The Odyssey")
#let (Homer: h) = books  // rename on extract
```

## Conditionals

```typst
#if x == 1 [One] else if x == 2 [Two] else [Other]

// Code block bodies
#if condition {
  let result = compute()
  [Result: #result]
}
```

## Loops

```typst
// For loop over array
#for item in ("a", "b", "c") [
  - #item
]

// For loop with destructuring
#for (key, val) in dict [
  #key: #val \
]

// For loop over string (iterates grapheme clusters)
#for c in "ABC" [#c is a letter. ]

// While loop
#let n = 1
#while n < 10 {
  n = n * 2
  [#n ]
}

// Control flow
#for x in range(10) {
  if x == 5 { break }
  if calc.odd(x) { continue }
  [#x ]
}
```

## Fields and Methods

```typst
// Field access
#let h = [= Heading]
#h.body    // content of heading
#h.depth   // nesting level

// Method calls (equivalent forms)
#str.len("hello")
#"hello".len()

// Common array methods
#let arr = (3, 1, 4, 1, 5)
#arr.len()
#arr.sorted()
#arr.map(x => x * 2)
#arr.filter(x => x > 2)
#arr.fold(0, (acc, x) => acc + x)

// Common string methods
#"hello world".split(" ")
#"hello".replace("l", "r")
#"  trim me  ".trim()
```

## Operators

| Operator   | Effect                          | Precedence |
|:----------:|---------------------------------|:----------:|
| `-` (unary)| Negation                        | 7          |
| `*`        | Multiplication                  | 6          |
| `/`        | Division                        | 6          |
| `+`        | Addition / content join         | 5          |
| `-`        | Subtraction                     | 5          |
| `==`       | Equality                        | 4          |
| `!=`       | Inequality                      | 4          |
| `<`, `<=`  | Less than (or equal)            | 4          |
| `>`, `>=`  | Greater than (or equal)         | 4          |
| `in`       | Check membership                | 4          |
| `not in`   | Check non-membership            | 4          |
| `not`      | Logical not                     | 3          |
| `and`      | Short-circuit logical and       | 3          |
| `or`       | Short-circuit logical or        | 2          |
| `=`        | Assignment                      | 1          |
| `+=`, `-=` | Add/subtract-assign             | 1          |

## Modules

```typst
// Include — inserts file content
#include "chapter1.typ"

// Import — creates module in scope
#import "utils.typ"
#utils.my-func()

// Import with rename
#import "utils.typ" as u
#u.helper()

// Import specific items
#import "utils.typ": my-func, helper
#my-func()

// Import all
#import "utils.typ": *
```

## Packages (Typst Universe)

```typst
// Format: @namespace/name:version
#import "@preview/cetz:0.3.4": canvas, draw
#import "@preview/charged-ieee:0.1.3": ieee

// Use .with() for template packages
#show: ieee.with(
  title: "My Paper",
  authors: (...)
)
```

Browse packages at: https://typst.app/universe

## Spread Operator

Pass array items as individual arguments:

```typst
#let sizes = (1fr, 1fr, 2fr)
#table(columns: sizes, ...)

// Or inline
#grid(columns: (1fr,) * 3, ...)  // repeat 3 times

// Spread in function call
#let args = ("hello", color: red)
#text(..args)
```

## Context

Access document state that depends on layout:

```typst
// Page number
#context counter(page).display()
#context counter(page).display("1 of 1", both: true)

// Current heading
#context {
  let headings = query(heading)
  headings.last().body
}

// Conditional on computed value
#context {
  if counter(page).get().first() > 1 [
    Continued...
  ]
}
```

## Data Loading

```typst
// Read text file
#let data = read("data.txt")

// Parse CSV
#let table-data = csv("data.csv")

// Parse JSON
#let config = json("config.json")

// Parse YAML
#let meta = yaml("meta.yml")

// Parse TOML
#let settings = toml("settings.toml")

// Read raw bytes
#let img-bytes = read("photo.jpg", encoding: none)
```
