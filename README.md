# TYPST-SKILLS

A Claude Code skill for writing and editing [Typst](https://typst.app) documents — the modern, markup-based alternative to LaTeX.

## What This Skill Does

This skill is automatically invoked when you ask Claude to:

- Write or edit `.typ` files
- Create Typst templates or documents
- Style documents with set/show rules
- Add math equations, tables, figures, or page layouts
- Import Typst packages from Typst Universe
- Write Typst functions or scripting logic

## Structure

```
TYPST-SKILLS/
├── SKILL.md                  # Skill definition loaded by Claude Code
├── skills-lock.json          # Skill dependency lockfile
├── references/
│   ├── syntax.md             # Full syntax tables: markup, math, code modes
│   ├── styling.md            # set/show rules and selector patterns
│   ├── scripting.md          # Variables, loops, modules, operators, packages
│   ├── math.md               # Math mode: functions, symbols, alignment
│   └── templates.md          # Building reusable templates and .with() pattern
└── typst/                    # Typst source submodule
```

## Quick Reference

### Three Modes

| Mode   | Entry             | Default in   |
|--------|-------------------|--------------|
| Markup | default           | `.typ` files |
| Code   | prefix with `#`   | anywhere     |
| Math   | surround with `$` | anywhere     |

### Set Rules

Configure defaults for elements:

```typst
#set text(font: "New Computer Modern", size: 12pt)
#set page(paper: "a4", margin: 2cm)
#set par(justify: true)
```

### Show Rules

Transform how elements render:

```typst
#show heading: set text(fill: navy)
#show "Typst": smallcaps
#show: doc => template(doc)   // wrap entire document
```

### Templates

```typst
// conf.typ
#let conf(title: "", doc) = {
  set page(paper: "a4")
  set text(font: "Libertinus Serif", size: 11pt)
  doc
}

// main.typ
#import "conf.typ": conf
#show: conf.with(title: "My Document")
```

### Packages

```typst
#import "@preview/cetz:0.3.4": canvas, draw
#import "@preview/charged-ieee:0.1.3": ieee
```

## Installation

Install via [Claude Code](https://claude.ai/code) with:

```bash
claude skills install BANG404/TYPST-SKILLS
```

## Resources

- [Typst Documentation](https://typst.app/docs)
- [Typst Universe](https://typst.app/universe) — community packages
- [Typst GitHub](https://github.com/typst/typst)
