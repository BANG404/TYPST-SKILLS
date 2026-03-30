# Typst Math Reference

## Entering Math Mode

```typst
// Inline math (no spaces around content)
The equation $E = m c^2$ is famous.

// Block math (space after opening and before closing $)
$ E = m c^2 $

// In code mode
#math.equation[E = m c^2]
```

## Basic Notation

```typst
// Subscript and superscript
$x_1$        // subscript
$x^2$        // superscript
$x_1^2$      // both
$x_(i+1)$    // multi-character: use parentheses
$x^(n+1)$

// Fractions (auto-detected)
$a/b$
$(a + b)/(c + d)$
$1 + (x+1)/5$

// Implied multiplication
$x y z$   // displays as xyz with multiplication spacing
```

## Common Symbols

```typst
// Greek letters (just type the name)
$alpha, beta, gamma, delta, epsilon$
$theta, lambda, mu, nu, xi$
$pi, rho, sigma, tau, phi, chi, psi, omega$
$Gamma, Delta, Theta, Lambda, Xi, Pi, Sigma, Phi, Psi, Omega$

// Operators
$+, -, times, div, pm, mp$
$=, !=, <, >, <=, >=, approx, equiv, ~$
$in, subset, supset, union, inter, sect$
$sum, prod, integral, iint, iiint$
$nabla, partial, infinity$

// Arrows
$arrow.r, arrow.l, arrow.r.long$
$->, <-, <->, =>$  // shorthand

// Dots
$dots, dots.h, dots.v, dots.c$
```

## Functions Available in Math

```typst
// Vectors and matrices
$vec(1, 2, 3)$
$mat(1, 0; 0, 1)$        // rows separated by ;
$mat(1, 2; 3, 4; 5, 6)$

// Limits
$lim_(x -> 0) sin(x)/x$
$sum_(i=0)^n i^2$
$integral_0^1 f(x) dif x$

// Roots
$sqrt(x)$
$root(3, x)$    // nth root

// Absolute value, norms
$abs(x)$
$norm(x)$

// Floor, ceiling
$floor(x)$
$ceil(x)$

// Calligraphic letters (for sets)
$cal(A), cal(B), cal(C)$

// Bold math
$bold(x), bold(A)$

// Upright text in math
$a "is natural"$
$f: RR -> CC$   // RR and CC are predefined
```

## Alignment in Equations

Use `&` for alignment points, `\` for line breaks:

```typst
// Aligned equations
$
  x + y &= 5 \
  2x - y &= 1
$

// Cases
$f(x) = cases(
  0 & "if" x < 0,
  x & "if" x >= 0,
)$
```

## Spacing in Math

```typst
// Manual spacing
$a thin b$    // thin space
$a med b$     // medium space
$a thick b$   // thick space
$a quad b$    // 1em space
$a wide b$    // 2em space
$a #h(1cm) b$ // custom space
```

## Symbol Variants

Symbols have variants accessed via dot notation:

```typst
$arrow.r$          // →
$arrow.r.long$     // ⟶
$arrow.squiggly$   // ↝
$arrow.r.double$   // ⇒
$dots.h$           // …
$dots.v$           // ⋮
$dots.c$           // ⋯
```

In markup mode, symbols need `#sym.` prefix:
```typst
#sym.arrow.r   // → in regular text
```

## Number Sets

Predefined double-struck letters:

```typst
$NN$   // Natural numbers ℕ
$ZZ$   // Integers ℤ
$QQ$   // Rationals ℚ
$RR$   // Reals ℝ
$CC$   // Complex numbers ℂ
```

## Common Math Patterns

```typst
// Derivative
$(dif f)/(dif x)$
$f'(x)$

// Partial derivative
$(partial f)/(partial x)$

// Limit
$lim_(n -> infinity) (1 + 1/n)^n = e$

// Sum with limits
$sum_(k=1)^n k = (n(n+1))/2$

// Integral
$integral_(-infinity)^(+infinity) e^(-x^2) dif x = sqrt(pi)$

// Matrix equation
$bold(A) bold(x) = bold(b)$

// System of equations
$cases(
  a x + b y = c,
  d x + e y = f,
)$
```

## Equation Numbering and References

```typst
// Numbered equation
$ E = m c^2 $ <eq-einstein>

// Reference
As shown in @eq-einstein, ...
```

## Styling Math

```typst
// Change font for math
#set math.equation(numbering: "(1)")

// Display style (forces large operators)
$display(sum_(i=0)^n i)$

// Inline style (forces compact operators)
$inline(sum_(i=0)^n i)$
```
