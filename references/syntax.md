# Typst Syntax Reference

## Modes

Typst has three syntactical modes:

| New mode | Syntax                          | Example                         |
|----------|---------------------------------|---------------------------------|
| Code     | Prefix with `#`                 | `[Number: #(1 + 2)]`            |
| Math     | Surround with `$...$`           | `[$-x$ is the opposite of $x$]` |
| Markup   | Surround with `[...]`           | `{let name = [*Typst!*]}`       |

Once in code mode via `#`, no further `#` is needed unless returning to markup/math.

## Markup Mode Syntax

| Name               | Example                      | Notes                      |
| ------------------ | ---------------------------- | -------------------------- |
| Paragraph break    | Blank line                   | `parbreak`                 |
| Strong emphasis    | `[*strong*]`                 | `strong`                   |
| Emphasis           | `[_emphasis_]`               | `emph`                     |
| Raw text           | `` [`print(1)`] ``           | `raw`                      |
| Link               | `[https://typst.app/]`       | `link`                     |
| Label              | `[<intro>]`                  | `label`                    |
| Reference          | `[@intro]`                   | `ref`                      |
| Heading            | `[= Heading]`                | `heading`                  |
| Bullet list        | `[- item]`                   | `list`                     |
| Numbered list      | `[+ item]`                   | `enum`                     |
| Term list          | `[/ Term: description]`      | `terms`                    |
| Math               | `[$x^2$]`                    | Inline; block with spaces  |
| Line break         | `[\]`                        | `linebreak`                |
| Smart quote        | `['single' or "double"]`     | `smartquote`               |
| Symbol shorthand   | `[~]`, `[---]`               | Non-breaking space, em-dash|
| Code expression    | `[#rect(width: 1cm)]`        | Scripting                  |
| Character escape   | `[Tweet at us \#ad]`         |                            |
| Comment            | `[/* block */]`, `[// line]` |                            |

## Math Mode Syntax

| Name                   | Example                  | Notes                     |
| ---------------------- | ------------------------ | ------------------------- |
| Inline math            | `[$x^2$]`                | No surrounding spaces     |
| Block-level math       | `[$ x^2 $]`              | Spaces inside `$`         |
| Bottom attachment      | `[$x_1$]`                | Subscript                 |
| Top attachment         | `[$x^2$]`                | Superscript               |
| Fraction               | `[$1 + (a+b)/5$]`        | Auto-detected             |
| Line break             | `[$x \ y$]`              |                           |
| Alignment point        | `[$x &= 2 \ &= 3$]`      | For aligned equations     |
| Variable access        | `[$#x$, $pi$]`           |                           |
| Field access           | `[$arrow.r.long$]`       | Symbol variants           |
| Implied multiplication | `[$x y$]`                | Space between letters     |
| Symbol shorthand       | `[$->$]`, `[$!=$]`       |                           |
| Text/string in math    | `[$a "is natural"$]`     | Literal text in math      |
| Math function call     | `[$floor(x)$]`           | No `#` needed             |
| Code expression        | `[$#rect(width: 1cm)$]`  |                           |
| Character escape       | `[$x\^2$]`               |                           |

## Code Mode Syntax

| Name                     | Example                       |
| ------------------------ | ----------------------------- |
| None                     | `{none}`                      |
| Auto                     | `{auto}`                      |
| Boolean                  | `{false}`, `{true}`           |
| Integer                  | `{10}`, `{0xff}`              |
| Float                    | `{3.14}`, `{1e5}`             |
| Length                   | `{2pt}`, `{3mm}`, `{1em}`     |
| Angle                    | `{90deg}`, `{1rad}`           |
| Fraction                 | `{2fr}`                       |
| Ratio                    | `{50%}`                       |
| String                   | `{"hello"}`                   |
| Array                    | `{(1, 2, 3)}`                 |
| Dictionary               | `{(a: "hi", b: 2)}`           |
| Code block               | `{{ let x = 1; x + 2 }}`      |
| Content block            | `{[*Hello*]}`                 |
| Let binding              | `{let x = 1}`                 |
| Named function           | `{let f(x) = 2 * x}`          |
| Set rule                 | `{set text(14pt)}`            |
| Set-if rule              | `{set text(..) if .. }`       |
| Show-set rule            | `{show heading: set block(..)}` |
| Show rule with function  | `{show raw: it => {..}}`      |
| Show-everything rule     | `{show: template}`            |
| Context expression       | `{context text.lang}`         |
| Conditional              | `{if x == 1 {..} else {..}}`  |
| For loop                 | `{for x in (1, 2, 3) {..}}`   |
| While loop               | `{while x < 10 {..}}`         |
| Loop control flow        | `{break, continue}`           |
| Return from function     | `{return x}`                  |
| Include module           | `{include "bar.typ"}`         |
| Import module            | `{import "bar.typ"}`          |
| Import items             | `{import "bar.typ": a, b, c}` |

## Escape Sequences

Use `\` to escape characters that have special meaning:

```typst
I got an ice cream for \$1.50! \u{1f600}
```

Unicode codepoints: `\u{1f600}` (hex). Works in strings too.

## Identifiers

- May contain letters, numbers, `-`, `_`
- Must start with a letter or `_`
- Recommended style: kebab-case (`top-edge`, `font-size`)
- Unicode letters supported

```typst
#let kebab-case = [Using hyphen]
#let _schön = "😊"
#let π = calc.pi
```

## Comments

```typst
// Single-line comment

/* Multi-line
   comment */
```

Comments are ignored and do not appear in output.
