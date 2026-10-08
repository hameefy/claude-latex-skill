# Supplementary Optimisation Macros

Optional macros for the LaTeX skill. The canonical preamble is in Step 2 of `SKILL.md`;
this file only adds macros that Step 2 does not define. Append the block below **after**
the notation macros of the Step 2 preamble. Every macro here is compatible with that
preamble (none redefines an existing command) and the block has been compiled together
with it under pdfLaTeX.

```latex
% ---------------------------------------------------------------
% Supplementary macros for numerical optimisation
% ---------------------------------------------------------------
% Operators
\DeclareMathOperator*{\argmin}{arg\,min}
\DeclareMathOperator*{\argmax}{arg\,max}
\DeclareMathOperator{\prox}{prox}
\DeclareMathOperator{\dom}{dom}

% Optimal point and optimal value
\newcommand{\xstar}{x^*}
\newcommand{\fstar}{f^*}

% Loss function (deep learning)
\newcommand{\loss}{\mathcal{L}}

% Weak convergence
\newcommand{\wconv}{\rightharpoonup}
```

## Usage notes

- `\argmin` and `\argmax` place their subscript underneath in display style:
  `\argmin_{x \in \R^n} f(x)`.
- `\bigO` keeps the one-argument form of Step 2, `\bigO{1/k}`. Do not redefine it.
- Use `\ip{u}{v}` (Step 2) for inner products and `\Hess` for the Hessian; no alternative
  names are defined here, so that one object has one macro.
- For journal submission, replace `\documentclass[11pt,a4paper]{article}` with the journal
  class and remove the packages that the class already loads.
