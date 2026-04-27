# Canonical LaTeX Preamble

This is the standard preamble for all DOCUMENT MODE `.tex` files produced by the LaTeX skill.
Copy it exactly. Customise only when the user explicitly requests it (e.g., a different font,
a journal-specific class, or additional packages for a specific task).

---

```latex
\documentclass[12pt, a4paper]{article}

% -----------------------------------------------------------------------
% Core AMS mathematics packages
% -----------------------------------------------------------------------
\usepackage{amsmath}        % align, equation, gather, etc.
\usepackage{amssymb}        % \mathbb, \mathcal, \mathfrak, etc.
\usepackage{amsfonts}       % extra math fonts
\usepackage{amsthm}         % theorem environments

% -----------------------------------------------------------------------
% Theorem environments
% -----------------------------------------------------------------------
\theoremstyle{plain}        % bold heading, italic body
\newtheorem{theorem}{Theorem}[section]
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{corollary}[theorem]{Corollary}

\theoremstyle{definition}   % bold heading, upright body
\newtheorem{definition}[theorem]{Definition}
\newtheorem{assumption}[theorem]{Assumption}
\newtheorem{example}[theorem]{Example}

\theoremstyle{remark}       % italic heading, upright body
\newtheorem{remark}[theorem]{Remark}
\newtheorem{note}[theorem]{Note}

% -----------------------------------------------------------------------
% Algorithms / Pseudocode
% -----------------------------------------------------------------------
\usepackage[ruled, vlined, linesnumbered]{algorithm2e}

% -----------------------------------------------------------------------
% Tables
% -----------------------------------------------------------------------
\usepackage{booktabs}       % \toprule, \midrule, \bottomrule
\usepackage{array}          % extended column types
\usepackage{tabularx}       % auto-width columns that fill \linewidth
\usepackage{adjustbox}      % \resizebox / adjustbox for margin-safe tables
\usepackage{siunitx}        % consistent number/unit formatting
\sisetup{
  round-mode      = places,
  round-precision = 4,
}

% -----------------------------------------------------------------------
% Figures and TikZ
% -----------------------------------------------------------------------
\usepackage{graphicx}
\usepackage{tikz}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\usetikzlibrary{arrows.meta, positioning, calc, shapes.geometric}
\usepackage{subcaption}     % subfigure / subtable environments

% -----------------------------------------------------------------------
% Layout and typography
% -----------------------------------------------------------------------
\usepackage[margin=2.5cm]{geometry}
\usepackage{microtype}      % improved text justification
\usepackage{setspace}
\setstretch{1.15}

% -----------------------------------------------------------------------
% References, cross-referencing, hyperlinks
% -----------------------------------------------------------------------
\usepackage{natbib}         % \citet, \citep — author-year citations
\bibliographystyle{plainnat}

\usepackage[
  colorlinks = true,
  linkcolor  = blue!70!black,
  citecolor  = green!50!black,
  urlcolor   = cyan!60!black
]{hyperref}

\usepackage{cleveref}       % \cref, \Cref — smart cross-references
% cleveref must be loaded AFTER hyperref

% -----------------------------------------------------------------------
% Utilities
% -----------------------------------------------------------------------
\usepackage{todonotes}      % \todo{} markers for incomplete sections
\usepackage{xcolor}         % named colours
\usepackage{bm}             % \bm{} for bold math symbols
\usepackage{mathtools}      % \coloneqq, \prescript, extra environments

% -----------------------------------------------------------------------
% Custom mathematical macros for numerical optimisation
% -----------------------------------------------------------------------

% Gradient and Hessian
\newcommand{\grad}{\nabla}
\newcommand{\hess}{\nabla^2}

% Norms
\newcommand{\norm}[1]{\left\| #1 \right\|}
\newcommand{\abs}[1]{\left| #1 \right|}
\newcommand{\inner}[2]{\left\langle #1,\, #2 \right\rangle}

% Sets and spaces
\newcommand{\R}{\mathbb{R}}
\newcommand{\N}{\mathbb{N}}
\newcommand{\E}{\mathbb{E}}          % expectation
\newcommand{\Prob}{\mathbb{P}}       % probability

% Optimisation objects
\newcommand{\loss}{\mathcal{L}}      % loss function
\newcommand{\obj}{f}                 % generic objective
\newcommand{\dom}{\operatorname{dom}}
\newcommand{\prox}{\operatorname{prox}}
\newcommand{\argmin}{\operatorname*{arg\,min}}
\newcommand{\argmax}{\operatorname*{arg\,max}}

% Big-O notation
\newcommand{\bigO}{\mathcal{O}}
\newcommand{\bigOmega}{\Omega}
\newcommand{\bigTheta}{\Theta}

% Iterates and step sizes
% (use \alpha_k or \eta_k inline; these macros are for common shorthands)
\newcommand{\xk}{x_k}
\newcommand{\xstar}{x^*}
\newcommand{\fstar}{f^*}

% Matrix and vector notation
\newcommand{\mat}[1]{\mathbf{#1}}   % matrices: \mat{A}, \mat{H}
\newcommand{\vec}[1]{\mathbf{#1}}   % vectors: \vec{x}, \vec{g}
\renewcommand{\vec}[1]{\boldsymbol{#1}}  % override default arrow-vec

% Convergence and complexity
\newcommand{\conv}{\rightarrow}
\newcommand{\wconv}{\rightharpoonup}  % weak convergence

% -----------------------------------------------------------------------
% Document metadata (fill in per document)
% -----------------------------------------------------------------------
% \title{Title}
% \author{Author}
% \date{\today}
```

---

## Notes on Customisation

- **Journal submission**: replace `\documentclass[12pt, a4paper]{article}` with the journal class
  (e.g., `\documentclass{elsarticle}`) and adjust packages accordingly. Remove `natbib` if the
  journal class manages citations internally.
- **Beamer slides**: use a separate preamble — do not mix `article` and `beamer` packages.
- **Additional TikZ libraries**: add `\usetikzlibrary{...}` after the existing `\usetikzlibrary`
  line; do not duplicate the library declaration.
- **`siunitx` column type**: in tables requiring aligned decimal points, use `S` column type:
  `\begin{tabular}{l S S S}`.
- **`tabularx`**: use when you want column widths to stretch automatically to fill `\linewidth`.
  The `X` column type absorbs surplus width; combine with `l`, `c`, `r`, or `S` for other columns.
- **`adjustbox`**: use `\resizebox{\linewidth}{!}{...}` to hard-scale any `tabular` or `tikzpicture`
  that would otherwise overflow the text area. This is always the last-resort fallback when column
  counts or figure widths cannot be reduced.