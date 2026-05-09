---
name: latex
description: >
  Produce high-quality, compilable LaTeX for researchers in computational and applied
  mathematics. Trigger for theorems, proofs, convergence analysis, algorithms, tables,
  TikZ figures, derivations, literature reviews, or any structured academic document.
  Also trigger for "write up", "typeset", "format in LaTeX", "produce a .tex file",
  "generate a report", or "give me the LaTeX for". Covers numerical optimisation, deep
  learning theory, EIT, regularisation, PINNs, and numerical analysis. Three modes:
  (1) DOCUMENT MODE — full standalone article .tex file; (2) SNIPPET MODE — body-only
  fragments; (3) BEAMER MODE — complete Beamer presentation slides. ALWAYS trigger
  BEAMER MODE for "slides", "presentation", "beamer", "seminar talk", "conference talk",
  "slide deck", or "talk on [topic]". Output is immediately compilable, mathematically
  rigorous, and free of AI-characteristic phrasing.
---

# LaTeX Skill

This skill governs the production of LaTeX output for a researcher in computational and
applied mathematics whose work spans numerical optimisation, deep learning theory, electrical
impedance tomography (EIT), regularisation theory, data-driven inversion, numerical analysis,
PINNs, and related areas. All output must satisfy the mathematical standards of journals such
as SIAM Journal on Scientific Computing, Inverse Problems, Mathematics of Computation, and
Journal of Computational Physics.

---

## Step 1. Classify the Request

Before writing anything, determine which of the **three** output modes applies.
Check for BEAMER MODE first, since it has the strongest surface signals.

### Beamer Mode  ← CHECK THIS FIRST

Produce a complete Beamer presentation (`.tex` file with `\documentclass{beamer}`) when
the request contains any of:

- The words "slides", "presentation", "beamer", "slide deck", "talk", "seminar",
  "conference talk", or "present my work on".
- A request to prepare a talk from a manuscript, paper draft, or set of results.
- A request for a "title slide", "table of contents slide", or named academic sections
  such as "introduction slide", "convergence results slide", "numerical experiments slide".

Proceed to **Step 2B** for the beamer preamble and **Step 3B** for beamer content rules.
Do NOT use the article preamble (Step 2) or article document structure (Step 4) for
Beamer Mode output.

**Content sourcing for Beamer Mode:**
- If the user supplies a complete manuscript or paper: extract content from it faithfully
  to populate each slide section.
- If the user supplies only a title, abstract, or partial notes: generate a fully
  structured template with `\todo{}` placeholders in every slide that lacks content.
- Never fabricate theorems, lemmas, or numerical results that were not provided.

### Document Mode

Produce a complete, standalone `.tex` file (preamble through `\end{document}`) using
`\documentclass{article}` when the request involves:

- A theorem, lemma, proposition, corollary, definition, or remark, with or without proof.
- A multi-step derivation or mathematical argument.
- A convergence analysis or complexity result.
- An algorithm presented alongside its theoretical justification.
- An introduction, related-work survey, or structured academic section.
- A self-contained report, technical note, or preprint draft.

Save the output as a `.tex` file and present it to the user for download.

### Snippet Mode

Produce raw LaTeX body content only (no `\documentclass`, no preamble, no
`\begin{document}`) when the request involves:

- A single equation or aligned system of equations.
- An algorithm or pseudocode block in isolation.
- A numerical results table.
- A TikZ figure or pgfplots graph.
- Any fragment intended to be inserted into the user's own template.

Deliver snippet output as a labelled code block in the chat, not as a file, unless the
user explicitly requests a file.

**When in doubt**, ask: "Should I produce a complete standalone document, a snippet, or a
Beamer presentation?"

---

## Step 2. Apply the Standard Preamble (Document Mode Only)

Use the preamble below verbatim. Do not improvise or abbreviate it.

```latex
\documentclass[11pt,a4paper]{article}

% ---------------------------------------------------------------
% Core mathematics packages
% ---------------------------------------------------------------
\usepackage{amsmath, amssymb, amsthm, mathtools}
\usepackage{bm}            % bold math symbols
\usepackage{dsfont}        % indicator function \mathds{1}

% ---------------------------------------------------------------
% Algorithms
% ---------------------------------------------------------------
\usepackage[ruled,vlined,linesnumbered]{algorithm2e}

% ---------------------------------------------------------------
% Graphics, figures, and plots
% ---------------------------------------------------------------
\usepackage{graphicx}
\usepackage{tikz}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\usepackage{subfigure}     % side-by-side figures
\usepackage{adjustbox}     % fallback rescaling for wide TikZ diagrams

% ---------------------------------------------------------------
% Tables
% ---------------------------------------------------------------
\usepackage{booktabs}
\usepackage{tabularx}
\usepackage{siunitx}       % decimal-aligned columns, SI units

% ---------------------------------------------------------------
% Cross-referencing and citations
% ---------------------------------------------------------------
\usepackage[numbers,sort&compress]{natbib}
\usepackage[colorlinks=true,linkcolor=blue,citecolor=blue,urlcolor=blue]{hyperref}
\usepackage[capitalise,noabbrev]{cleveref}

% ---------------------------------------------------------------
% Typography
% ---------------------------------------------------------------
\usepackage{microtype}
\usepackage{parskip}
\setlength{\parindent}{0pt}
\setlength{\parskip}{6pt}

% ---------------------------------------------------------------
% Page layout
% ---------------------------------------------------------------
\usepackage[margin=2.5cm]{geometry}

% ---------------------------------------------------------------
% Miscellaneous
% ---------------------------------------------------------------
\usepackage{xcolor}
\usepackage{todonotes}     % \todo{} markers for placeholders

% ---------------------------------------------------------------
% Theorem environments (amsthm)
% ---------------------------------------------------------------
\theoremstyle{plain}
\newtheorem{theorem}{Theorem}[section]
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{corollary}[theorem]{Corollary}

\theoremstyle{definition}
\newtheorem{definition}[theorem]{Definition}
\newtheorem{assumption}{Assumption}
\newtheorem{example}[theorem]{Example}

\theoremstyle{remark}
\newtheorem{remark}[theorem]{Remark}

% ---------------------------------------------------------------
% Notation macros (computational and applied mathematics)
% ---------------------------------------------------------------
% Norms and inner products
\newcommand{\norm}[1]{\left\lVert #1 \right\rVert}
\newcommand{\normF}[1]{\left\lVert #1 \right\rVert_{\mathrm{F}}}
\newcommand{\ip}[2]{\left\langle #1,\, #2 \right\rangle}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}

% Calculus and optimisation
\newcommand{\grad}{\nabla}
\newcommand{\Hess}{\nabla^2}
\newcommand{\Lapl}{\Delta}
\newcommand{\divergence}{\operatorname{div}}
\newcommand{\curl}{\operatorname{curl}}

% Function spaces
\newcommand{\Lp}[1]{L^{#1}}
\newcommand{\Sob}[2]{H^{#1}(#2)}
\newcommand{\SobZ}[2]{H^{#1}_0(#2)}

% Operators and matrices
\newcommand{\bA}{\mathbf{A}}
\newcommand{\bB}{\mathbf{B}}
\newcommand{\bJ}{\mathbf{J}}
\newcommand{\bK}{\mathbf{K}}
\newcommand{\bI}{\mathbf{I}}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\by}{\mathbf{y}}
\newcommand{\bz}{\mathbf{z}}
\newcommand{\bu}{\mathbf{u}}
\newcommand{\bv}{\mathbf{v}}

% Probability and statistics
\newcommand{\E}{\mathbb{E}}
\newcommand{\Prob}{\mathbb{P}}
\newcommand{\Var}{\operatorname{Var}}
\newcommand{\Cov}{\operatorname{Cov}}
\newcommand{\indicator}{\mathbf{1}}

% Asymptotic notation
\newcommand{\bigO}[1]{\mathcal{O}\!\left(#1\right)}
\newcommand{\smallO}[1]{o\!\left(#1\right)}

% Common domains
\newcommand{\R}{\mathbb{R}}
\newcommand{\N}{\mathbb{N}}
\newcommand{\C}{\mathbb{C}}

% Regularisation (EIT / inverse problems)
\newcommand{\Reg}{\mathcal{R}}
\newcommand{\Forward}{\mathcal{F}}
\newcommand{\Tikhonov}[3]{\norm{#1 - #2}^2 + #3\,\Reg(#2)}

% Iterates and step sizes
\newcommand{\xk}{x_k}
\newcommand{\alphak}{\alpha_k}
\newcommand{\etak}{\eta_k}
```

---

## Step 2B. Beamer Preamble (Beamer Mode Only)

Use the preamble below for ALL Beamer presentations. Do NOT mix with the article preamble
in Step 2. The two preambles are mutually exclusive.

**Theme selection:** The default theme is `Madrid`. If the user specifies a different
standard theme (e.g., `Berlin`, `AnnArbor`, `Warsaw`, `Copenhagen`, `Frankfurt`,
`Singapore`, `Boadilla`, `CambridgeUS`), substitute it in `\usetheme{}`. Never invent
custom themes; use only named Beamer built-in themes.

**Color theme:** Default is `default` (matching the chosen outer theme's palette). If the
user requests a specific color theme (e.g., `dolphin`, `beaver`, `crane`, `orchid`,
`rose`, `seagull`, `seahorse`, `whale`, `wolverine`), apply it with `\usecolortheme{}`.

```latex
\documentclass[aspectratio=169,10pt]{beamer}
% aspectratio=169 gives 16:9 widescreen; use 43 for 4:3 if user requests it.

% ---------------------------------------------------------------
% Theme — change \usetheme{} to any standard Beamer theme name
% ---------------------------------------------------------------
\usetheme{Madrid}
% \usecolortheme{dolphin}   % Uncomment and change to apply a colour theme

% ---------------------------------------------------------------
% Core mathematics
% ---------------------------------------------------------------
\usepackage{amsmath, amssymb, amsthm, mathtools}
\usepackage{bm}

% ---------------------------------------------------------------
% Algorithms — use ONE of the two options below; comment out the other
% ---------------------------------------------------------------
% OPTION A: algorithm2e (recommended for pseudocode with line numbers)
\usepackage[ruled,vlined,linesnumbered]{algorithm2e}

% OPTION B: algorithmicx + algpseudocode (uncomment if preferred)
% \usepackage{algorithmic}
% \usepackage{algorithmicx}
% \usepackage{algpseudocode}

% ---------------------------------------------------------------
% Graphics and plots
% ---------------------------------------------------------------
\usepackage{graphicx}
\usepackage{tikz}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\usepackage{subfigure}

% ---------------------------------------------------------------
% Tables
% ---------------------------------------------------------------
\usepackage{booktabs}
\usepackage{tabularx}

% ---------------------------------------------------------------
% Theorem-like blocks — configurable style
% ---------------------------------------------------------------
% Beamer provides: theorem, lemma, corollary, proof, definition,
% example, block, alertblock, exampleblock — all built-in.
%
% For coloured framed boxes (tcolorbox style), uncomment below:
% \usepackage{tcolorbox}
% \tcbuselibrary{theorems,skins}
% Then define custom tcolorbox environments as needed (see Step 3B).

% ---------------------------------------------------------------
% Footnote citation control
% ---------------------------------------------------------------
\usepackage{perpage}   % resets footnote counter on each slide
\MakePerPage{footnote}

% ---------------------------------------------------------------
% Miscellaneous
% ---------------------------------------------------------------
\usepackage{xcolor}
\usepackage{multicol}  % for two-column slides

% ---------------------------------------------------------------
% Notation macros (shared with article mode for consistency)
% ---------------------------------------------------------------
\newcommand{\norm}[1]{\left\lVert #1 \right\rVert}
\newcommand{\ip}[2]{\left\langle #1,\, #2 \right\rangle}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\grad}{\nabla}
\newcommand{\R}{\mathbb{R}}
\newcommand{\N}{\mathbb{N}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\bigO}[1]{\mathcal{O}\!\left(#1\right)}
\newcommand{\xk}{x_k}
\newcommand{\alphak}{\alpha_k}
\newcommand{\etak}{\eta_k}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\bg}{\mathbf{g}}
\newcommand{\bA}{\mathbf{A}}
\newcommand{\bJ}{\mathbf{J}}

% ---------------------------------------------------------------
% Presentation metadata — fill in before \begin{document}
% ---------------------------------------------------------------
\title[Short Title]{Full Title of the Presentation}
\subtitle{Subtitle or Paper Title (if applicable)}
\author[H.~Mohammad]{Hassan Mohammad}
\institute[BUK]{%
  Numerical Optimisation Research Group\\
  Department of Mathematical Sciences\\
  Faculty of Physical Sciences\\
  Bayero University, Kano, Nigeria
}
\date{\today}
```

---

## Step 3B. Beamer Content Standards (Beamer Mode Only)

### 3B.1 Section Structure — Flexible Defaults

The eight sections below are the default scaffold. The user may omit any section or
reorder them. When a section is omitted, remove its `\section{}` declaration and all
corresponding frames. Never leave an empty `\section{}` block.

| # | Section | `\section{}` name | Typical frame count |
|---|---|---|---|
| 1 | Title page | *(title frame, no section)* | 1 |
| 2 | Table of contents | *(TOC frame, no section)* | 1 |
| 3 | Introduction | `Introduction` | 2–3 |
| 4 | Literature review / Related work / Motivation | `Related Work` | 2–3 |
| 5 | Method / Algorithm | `Methodology` | 3–5 |
| 6 | Convergence results | `Convergence Analysis` | 2–4 |
| 7 | Implementation / Numerical experiments | `Numerical Experiments` | 2–3 |
| 8 | Conclusion / Further research | `Conclusion` | 1–2 |

If the user provides only a title or partial content, generate all eight sections with
`\todo{}` placeholder text inside each frame body. If the user provides a full manuscript,
populate each section from the manuscript, preserving mathematical notation exactly.

### 3B.2 Footnote Citations

Beamer Mode uses `\footnotemark` / `\footnotetext{}` pairs exclusively. There is NO
reference section at the end of the presentation; every cited source appears as a
footnote on the slide where it is first cited.

**Citation pattern — use verbatim:**

```latex
% Within slide body text, place the mark:
...as shown by La Cruz et al.\footnotemark{}...

% Immediately before \end{frame}, place the text:
\footnotetext{W.~La Cruz, J.~Mart\'{\i}nez, and M.~Raydan,
  ``Spectral residual method without gradient information for solving large-scale
  nonlinear systems of equations,''
  \textit{Math.\ Comp.}, vol.~75, no.~255, pp.~1429--1448, 2006.}
```

Rules for footnote citations:
1. Every `\footnotemark` must have a matching `\footnotetext` within the same `frame`.
2. Use `\MakePerPage{footnote}` (already in the preamble) so the counter resets per slide.
3. If more than two references appear on one slide, consider splitting the slide or using
   a smaller font for the `\footnotetext` entries: `{\tiny \footnotetext{...}}`.
4. Format: Author(s), ``Title,'' \textit{Journal/Proceedings}, vol., no., pp., Year.
   For books: Author(s), \textit{Title}, Publisher, Year.
5. Never list a reference in a `\footnotetext` that does not have a corresponding
   `\footnotemark` on the same slide.
6. If the user provides BibTeX keys without full details, supply the correct bibliographic
   entry from knowledge of the standard literature. If genuinely ambiguous, insert:
   `\footnotetext{\todo{Fill in full bibliographic details.}}`

### 3B.3 Theorem-like Blocks

**Default (Beamer built-in environments):** Use `\begin{theorem}`, `\begin{lemma}`,
`\begin{corollary}`, `\begin{definition}`, `\begin{proof}` directly inside frames.
Beamer styles these automatically with coloured headers matching the chosen theme.

```latex
\begin{frame}{Convergence Result}
  \begin{theorem}[Global Convergence]\label{thm:global}
    Let Assumptions~1 and~2 hold. If $\{x_k\}$ is the sequence generated by
    Algorithm~1, then
    \[
      \lim_{k \to \infty} \|F(x_k)\| = 0.
    \]
  \end{theorem}
  \footnotetext{\todo{Citation if theorem is from a paper.}}
\end{frame}
```

**Optional tcolorbox style (activate in preamble):** If the user requests coloured framed
boxes resembling the sample slides, uncomment the `tcolorbox` lines in the preamble and
add definitions such as:

```latex
\usepackage{tcolorbox}
\tcbuselibrary{theorems,skins}
\newtcbtheorem[number within=section]{thm}{Theorem}{%
  colback=blue!5, colframe=blue!40!black,
  fonttitle=\bfseries}{thm}
\newtcbtheorem[number within=section]{lem}{Lemma}{%
  colback=green!5, colframe=green!40!black,
  fonttitle=\bfseries}{lem}
```

Do NOT load `tcolorbox` by default; only include it when the user explicitly requests
coloured framed boxes.

### 3B.4 Algorithm Pseudocode on Slides

Use `algorithm2e` (already loaded) inside a `frame` with `[fragile]` option, because
verbatim-like environments require it.

```latex
\begin{frame}[fragile]{Algorithm: Name of Algorithm}
  \begin{algorithm}[H]
  \caption{AlgorithmName}\label{alg:main}
  \KwIn{Initial point $x_0 \in \R^n$, tolerance $\varepsilon > 0$}
  \KwOut{Approximate solution $x^*$}
  Set $k \leftarrow 0$\;
  \While{$\|F(x_k)\| > \varepsilon$}{
    Compute search direction $d_k$\;
    Find steplength $\alpha_k$ via line search\;
    $x_{k+1} \leftarrow x_k + \alpha_k d_k$\;
    $k \leftarrow k + 1$\;
  }
  \Return{$x_k$}\;
  \end{algorithm}
\end{frame}
```

If using `algorithmicx` instead, the frame must also be `[fragile]`:

```latex
\begin{frame}[fragile]{Algorithm: Name}
  \begin{algorithmic}[1]
    \Require $x_0$, $\varepsilon > 0$
    \Ensure $x^*$
    \For{$k = 0, 1, 2, \ldots$}
      \State Compute $d_k$
      \State $x_{k+1} \leftarrow x_k + \alpha_k d_k$
    \EndFor
  \end{algorithmic}
\end{frame}
```

### 3B.5 Performance Profile and Numerical Results Slides

For numerical experiments, use a two-column layout to show profiles and tables side by
side:

```latex
\begin{frame}{Numerical Results — Performance Profiles}
  \begin{columns}[T]
    \begin{column}{0.48\textwidth}
      \begin{figure}
        \includegraphics[width=\linewidth]{fig_iterations.pdf}
        \caption{Number of iterations}
      \end{figure}
    \end{column}
    \begin{column}{0.48\textwidth}
      \begin{figure}
        \includegraphics[width=\linewidth]{fig_fevals.pdf}
        \caption{Function evaluations}
      \end{figure}
    \end{column}
  \end{columns}
  \vspace{0.3em}
  {\small Dolan--Mor\'{e} performance profiles; higher is better.}
\end{frame}
```

For tables of numerical results:

```latex
\begin{frame}{Numerical Results — Comparison Table}
  \begin{table}
    \centering
    \small
    \begin{tabular}{lrrr}
      \toprule
      Method & Iter. & F-Evals & CPU (s) \\
      \midrule
      Method A & 42 & 89 & \textbf{0.31} \\
      Method B & \textbf{38} & \textbf{76} & 0.45 \\
      \bottomrule
    \end{tabular}
    \caption{Comparison on test problems ($n = 1000$).}
  \end{table}
\end{frame}
```

### 3B.6 Frame Construction Rules

1. Every frame must have a non-empty `\frametitle{}` argument (or use the `{Title}` short
   form of `\begin{frame}{Title}`).
2. Never overload a single frame; limit each slide to one main idea, result, or algorithm
   step. If content overflows, split into multiple frames with the same section and a
   subtitle distinguishing them (e.g., "Proof — Part I", "Proof — Part II").
3. Use `\pause` sparingly; prefer complete slides for printed handouts.
4. Use `\alert{}` to highlight a single key term or result per frame, not multiple items.
5. Use `\begin{itemize}` / `\begin{enumerate}` with no more than five items per frame.
   Sub-items are allowed but limit nesting to two levels.
6. Equations on slides must be display-style whenever they span more than a short inline
   fragment. Prefer `\[ ... \]` over inline `$ ... $` for anything non-trivial.
7. Every frame that cites a source must have at least one `\footnotemark` and its
   matching `\footnotetext` (see Section 3B.2).

### 3B.7 Table of Contents Slide

Use `\tableofcontents` with `[hideallsubsections]` to show only section-level entries.
For a long talk, use `[currentsection]` at the start of each section to re-show the TOC
with the current section highlighted.

```latex
% Main TOC slide (after title)
\begin{frame}{Outline}
  \tableofcontents[hideallsubsections]
\end{frame}

% Optional: section-entry TOC slide at the start of each section
\AtBeginSection[]{
  \begin{frame}{Outline}
    \tableofcontents[currentsection, hideallsubsections]
  \end{frame}
}
```

---

## Step 3. Mathematical Content Standards

### 3.1 Theorem-like Environments

Use `amsthm` environments throughout. Apply consistent label prefixes:
`thm:`, `lem:`, `prop:`, `cor:`, `def:`, `ass:`, `rem:`, `ex:`, `eq:`, `alg:`, `fig:`, `tab:`.

Every numbered environment must carry a `\label{}`. Every `\ref{}` and `\eqref{}` must match
an existing label. Use `\cref{}` (from `cleveref`) in preference to `\ref{}` wherever the
environment type should appear in text. Use `\qed` or rely on `amsthm`'s automatic QED
symbol at the end of proofs. If assumptions are referenced repeatedly, define them as
numbered `assumption` environments.

Example with full proof scaffold:

```latex
\begin{assumption}\label{ass:smoothness}
  The function $f \colon \R^n \to \R$ is $L$-smooth: there exists $L > 0$ such that
  \begin{equation}\label{eq:smoothness}
    \norm{\grad f(x) - \grad f(y)} \leq L\norm{x - y}
    \quad \text{for all } x, y \in \R^n.
  \end{equation}
\end{assumption}

\begin{theorem}[Convergence of gradient descent]\label{thm:gd-convergence}
  Suppose \cref{ass:smoothness} holds and $f$ is bounded below. Let $\{x_k\}_{k \geq 0}$
  be the iterates of gradient descent with constant step size $\alpha \in (0, 1/L]$.
  Then
  \begin{equation}\label{eq:gd-rate}
    \min_{0 \leq k \leq K-1} \norm{\grad f(x_k)}^2
    \leq \frac{2\bigl(f(x_0) - f^*\bigr)}{\alpha K},
  \end{equation}
  where $f^* \coloneqq \inf_{x \in \R^n} f(x)$.
\end{theorem}

\begin{proof}
  By $L$-smoothness (\cref{ass:smoothness}), the descent lemma gives
  \[
    f(x_{k+1}) \leq f(x_k) - \frac{1}{2L}\norm{\grad f(x_k)}^2.
  \]
  Summing from $k = 0$ to $K-1$ and dividing by $K$ yields \cref{eq:gd-rate}.
\end{proof}
```

If the user provides a theorem statement without a proof, insert a scaffold:

```latex
\begin{proof}
  \todo{Proof to be completed. Suggested outline: (i) establish the descent lemma;
  (ii) telescope; (iii) divide by $K$.}
\end{proof}
```

Do not fabricate a proof.

### 3.2 Equations and Alignment

| Context | Environment | Rule |
|---|---|---|
| Single numbered equation | `equation` | Always label |
| Multi-line derivation | `align` | Align at `=` or relational symbol using `&`; label each key step |
| Auxiliary steps (unnumbered) | `align*` | No label required |
| Inline expressions | `$...$` | Use only for short symbols; prefer display for anything non-trivial |

Use `\coloneqq` (from `mathtools`) for definitions. Use `\text{where}`, `\text{for all}`,
and similar inside display math. Place punctuation inside displayed equations when the
surrounding sentence requires it. Never use `\text{O}` for asymptotic notation; always use
`\bigO{\cdot}` as defined in the preamble.

### 3.3 Notation Conventions

Be consistent within every document. The conventions below apply unless the user specifies
otherwise.

| Object | Notation |
|---|---|
| Scalars | $\alpha, \beta, \lambda \in \R$ (lowercase italic) |
| Vectors | $\bx, \by \in \R^n$ (bold lowercase) |
| Matrices / linear operators | $\bA, \bJ \in \R^{m \times n}$ (bold uppercase) |
| Function spaces | $\Lp{2}(\Omega)$, $\Sob{1}{\Omega}$, $\SobZ{1}{\Omega}$ |
| Gradient | $\grad f(x)$ |
| Hessian | $\Hess f(x)$ |
| Iterates | $x_k$ (subscript) or $x^{(k)}$ (superscript); pick one and do not change it |
| Objective / loss | $f$, $\mathcal{L}$, or $F$ following the user's choice |
| Step size | $\alphak$ or $\etak$; do not mix |
| Regularisation parameter | $\lambda$ or $\mu$ (not $\alpha$ if that is the step size) |
| Frobenius norm | $\normF{\bA}$ |
| Expectation | $\E[\cdot]$ |
| Probability | $\Prob(\cdot)$ |
| Forward (observation) operator | $\Forward$ |
| Regulariser | $\Reg$ |

For EIT and inverse problems, adopt the following when not overridden by the user:

- Conductivity distribution: $\sigma \in L^\infty(\Omega)$ with $\sigma \geq \sigma_{\min} > 0$.
- Dirichlet-to-Neumann map: $\Lambda_\sigma \colon H^{1/2}(\partial\Omega) \to H^{-1/2}(\partial\Omega)$.
- Measurement data: $\mathbf{V} \in \R^{m \times n_e}$.
- Tikhonov functional: $\Tikhonov{\Forward(\sigma)}{\mathbf{V}}{\lambda}$ per the macro.

### 3.4 Algorithm Pseudocode

Use the `algorithm2e` package (loaded with `ruled, vlined, linesnumbered`). Number lines
whenever the proof or analysis refers to specific steps. Add inline comments with `\tcp{}`.

```latex
\begin{algorithm}[H]
\caption{Iteratively Regularised Gauss--Newton (IRGN)}\label{alg:irgn}
\KwIn{Initial guess $\sigma^{(0)}$; data $\mathbf{V}$;
      regularisation parameters $\{\lambda_k\}$; tolerance $\varepsilon > 0$}
\KwOut{Approximate solution $\sigma^{(K)}$}
Set $k \leftarrow 0$\;
\While{$\norm{\sigma^{(k+1)} - \sigma^{(k)}} > \varepsilon$}{
  Compute Jacobian $\bJ_k \leftarrow \Forward'(\sigma^{(k)})$\;
  \tcp{Solve the linearised regularised subproblem}
  $\sigma^{(k+1)} \leftarrow \arg\min_{\sigma}
    \bigl\|\bJ_k(\sigma - \sigma^{(k)}) - (\mathbf{V} - \Forward(\sigma^{(k)}))\bigr\|^2
    + \lambda_k \Reg(\sigma)$\;
  $k \leftarrow k + 1$\;
}
\Return{$\sigma^{(k)}$}\;
\end{algorithm}
```

### 3.5 Tables of Numerical Results

Rules (apply without exception):
1. Use `booktabs` (`\toprule`, `\midrule`, `\bottomrule`). Never use vertical rules in the body.
2. Bold the best entry in each column with `\textbf{}`.
3. Report uncertainties as `$\mu \pm \sigma$` using `\pm`.
4. Use `siunitx` with the `S` column type for decimal alignment when precision matters.
5. Never allow a table to exceed `\linewidth`. Choose the construction method by column count:
   - Narrow (<=4 cols): plain `tabular`
   - Wide (5-7 cols): `tabularx` with `\linewidth`
   - Very wide (8+ cols): `\resizebox{\linewidth}{!}{...}`

Prefer `tabularx` over `\resizebox` wherever possible — rescaling reduces font size relative
to surrounding text.

See Section 3.5 examples below:

```latex
% Narrow table
\begin{table}[ht]
\centering
\caption{Relative reconstruction error. Bold indicates the lowest error.}\label{tab:relerr}
\begin{tabular}{lccc}
\toprule
Method & $\delta = 0.01$ & $\delta = 0.05$ & $\delta = 0.10$ \\
\midrule
Tikhonov ($\ell^2$) & $0.142 \pm 0.008$ & $0.231 \pm 0.011$ & $0.318 \pm 0.014$ \\
TV regularisation   & $\mathbf{0.103 \pm 0.006}$ & $\mathbf{0.187 \pm 0.009}$ & $0.274 \pm 0.013$ \\
\bottomrule
\end{tabular}
\end{table}

% Wide table
\begin{table}[ht]
\centering
\caption{Performance metrics across five noise levels.}\label{tab:perf}
\begin{tabularx}{\linewidth}{l *{5}{>{\centering\arraybackslash}X}}
\toprule
Method & $\delta_1$ & $\delta_2$ & $\delta_3$ & $\delta_4$ & $\delta_5$ \\
\midrule
Method A & val & val & val & val & val \\
\bottomrule
\end{tabularx}
\end{table}
```

### 3.6 TikZ Figures and pgfplots Graphs

Every figure must include: axis labels, a legend when multiple series are plotted, a
`\caption`, and a `\label`. For convergence plots, use `ymode=log`.

**Width policy (mandatory):** Every `tikzpicture` and `pgfplots` axis must declare an
explicit width using a relative length. Never use absolute `cm` or `pt` values.

| Layout | `width` value |
|---|---|
| Single figure, full-width | `0.85\textwidth` |
| Single figure, default | `0.75\textwidth` |
| Two side-by-side subfigures | `\linewidth` inside a `0.48\textwidth` subfigure |
| Three in a row | `\linewidth` inside a `0.32\textwidth` subfigure |
| Inset or thumbnail | `0.40\textwidth` |

```latex
% Standard convergence plot
\begin{figure}[ht]
\centering
\begin{tikzpicture}
\begin{semilogyaxis}[
    xlabel={Iteration $k$},
    ylabel={$f(x_k) - f^*$},
    legend pos=north east,
    grid=major,
    width=0.75\textwidth,
    height=0.5\textwidth
]
\addplot[blue, thick] coordinates { ... };
\addlegendentry{Method A}
\addplot[red, dashed, thick] coordinates { ... };
\addlegendentry{Method B}
\end{semilogyaxis}
\end{tikzpicture}
\caption{Convergence comparison. Vertical axis in logarithmic scale.}\label{fig:convergence}
\end{figure}

% Wide diagram fallback
\begin{figure}[ht]
\centering
\adjustbox{max width=\textwidth}{%
  \begin{tikzpicture}
    % wide architecture diagram
  \end{tikzpicture}%
}
\caption{Network architecture.}\label{fig:arch}
\end{figure}
```

---

## Step 4. Document Structure

### 4.1 Theorem or Proof Document

```
\section{Problem Setup}        % domain, objective, algorithm
\section{Assumptions}          % numbered assumption environments
\section{Main Result}          % preliminary lemmas, then main theorem with proof
\section{Discussion}           % implications, special cases, connections (optional)
```

### 4.2 Convergence Analysis

```
\section{Problem Formulation}       % objective, domain, algorithm statement
\section{Assumptions}               % numbered assumption environments
\section{Main Convergence Theorem}  % theorem + proof
\section{Complexity Analysis}       % oracle and iteration complexity (when applicable)
\section{Numerical Experiments}     % tables and figures
```

### 4.3 Derivation Document

```
\section{Setup}       % define all objects, notation, spaces
\section{Derivation}  % step-by-step with align environments
\section{Result}      % \boxed{} final expression
```

Highlight a key final result with:

```latex
\begin{equation}\label{eq:main}
\boxed{ x_{k+1} = x_k - \alphak \grad f(x_k) }
\end{equation}
```

### 4.4 Literature Review or Introduction

Structure:
1. Motivate the problem with context and practical relevance.
2. Survey related work using `\citet{}` and `\citep{}`.
3. Identify the gap or limitation addressed by the present work.
4. State contributions precisely (numbered or itemised list).
5. Outline the remainder of the document.

Example:

```latex
Early work by \citet{calderon1980} established the theoretical basis for EIT.
Regularisation strategies were studied by \citet{engl1996} and \citet{vogel2002}.
Data-driven approaches have recently gained attention~\citep{hamilton2018, adler2017}.
```

---

## Step 5. Reference Section (Document Mode)

### 5.0 Choose a Bibliography Workflow

**Ask the user which workflow they use before writing the reference section.** If no
preference is stated, default to Option A. The options are architecturally incompatible:
do not mix commands or packages from both in the same document.

| | Option A — Self-contained | Option B — External `.bib` |
|---|---|---|
| **Backend** | Inline `thebibliography`; no auxiliary files | BibTeX (`natbib`) or BibLaTeX (Biber) |
| **When to use** | Quick notes, isolated proofs, single-file submissions | Ongoing projects with a shared `.bib`; BibLaTeX users |

---

### Option A — Self-Contained (`thebibliography`)

Place immediately before `\end{document}`. Set the argument to the widest label expected
(e.g., `{99}`). Do not include `\bibliographystyle{}` or `\bibliography{}`.

#### `\bibitem` Format by Source Type

```latex
% Journal article
\bibitem{citekey}
A.~Surname and B.~Surname,
``Title,''
\textit{Journal Name},
vol.~X, no.~Y, pp.~NNN--NNN, Year.

% Conference paper
\bibitem{citekey}
A.~Surname and B.~Surname,
``Title,''
in \textit{Proceedings of the Conference (ACRONYM)},
City, Country, Year, pp.~NNN--NNN.

% Book
\bibitem{citekey}
A.~Surname,
\textit{Title of the Book},
Publisher, City, Year.

% PhD / MSc thesis
\bibitem{citekey}
A.~Surname,
``Title of the thesis,''
Ph.D.\ dissertation, Department, University, City, Country, Year.

% Technical report
\bibitem{citekey}
A.~Surname,
``Title,''
Tech.\ Rep.\ TR-XXXX, Institution, Year.

% arXiv preprint
\bibitem{citekey}
A.~Surname and B.~Surname,
``Title,''
\textit{arXiv preprint} arXiv:XXXX.XXXXX, Year.
```

If the user cites a key without providing bibliographic details, populate from knowledge of
the standard literature. For well-known works, supply complete and accurate entries. If
genuinely ambiguous, insert:

```latex
\bibitem{citekey}
\todo{Fill in full bibliographic details for \texttt{citekey}.}
```

Do not omit the `\bibitem`; a missing entry causes a compilation error.

---

### Option B1 — BibTeX with `natbib`

Replace the natbib preamble line with:

```latex
\usepackage[numbers,sort&compress]{natbib}
```

End-of-document block:

```latex
\bibliographystyle{plainnat}   % or: unsrtnat, ieeetr, siam, plain
\bibliography{refs}
```

Compilation: `pdflatex` → `bibtex` → `pdflatex` × 2.

Common `.bst` files: `plainnat` (author-year), `plain` (numbered), `ieeetr` (IEEE),
`siam` (SIAM), `amsplain` (AMS), `unsrt` (citation order).

---

### Option B2 — BibLaTeX with Biber

Remove `\usepackage{natbib}` entirely. Replace with:

```latex
\usepackage[
  backend=biber,
  style=authoryear,
  sorting=nyt,
  maxbibnames=99,
  giveninits=true,
  doi=false,
  url=false,
  eprint=true
]{biblatex}
\addbibresource{refs.bib}
```

End-of-document: `\printbibliography`. Do not use `\bibliographystyle{}` or `\bibliography{}`.

Compilation: `pdflatex` (or `lualatex`) → `biber` → `pdflatex` × 2.

Citation commands: `\parencite{}` (parenthetical), `\textcite{}` (textual),
`\citeyear{}` (year only), `\citeauthor{}` (author only).

Sample `.bib` entry:

```bibtex
@article{nesterov1983,
  author  = {Nesterov, Yurii},
  title   = {A method for solving the convex programming problem with convergence
             rate {$\mathcal{O}(1/k^2)$}},
  journal = {Doklady Akademii Nauk SSSR},
  volume  = {269},
  number  = {3},
  pages   = {543--547},
  year    = {1983}
}
```

If the user cites a key without a `.bib` entry, supply the correct entry from knowledge of
the standard literature. If genuinely ambiguous, insert:

```bibtex
@misc{citekey,
  note = {TODO: Fill in full bibliographic details.}
}
```

---

## Step 6. Writing Quality Standards

### 6.1 Prohibited Characters and Typographic Flags

The following must never appear in any output:

| Item | Rule |
|---|---|
| Em dash (---) | Prohibited. Use comma, semicolon, colon, or parentheses. |
| En dash (--) outside LaTeX ranges | Prohibited in prose. In LaTeX, `--` is correct only for numeric ranges and compound proper-noun modifiers (Dirichlet--Neumann map). |
| Ellipsis (...) | Use `\ldots` in math mode. In prose, avoid entirely; state the complete idea. |
| Exclamation mark | Prohibited in scientific prose. |
| Scare quotes | Avoid. Use italics on first use: `\textit{prior model}`. |
| Contractions | Prohibited. Write: it is, do not, we have. |
| Rhetorical questions | Prohibited. |
| Transitional intensifiers | Prohibited: "importantly", "crucially", "notably", "it is worth noting that". State the point directly. |
| First-person singular (I) | Use "we" consistently, even for single-author work. |
| Noun stacks (3+ consecutive) | Avoid "deep learning-based inverse problem regularisation framework". Rewrite as "a regularisation framework for inverse problems based on deep learning". |

### 6.2 Sentence and Paragraph Construction

Write in complete, declarative sentences. Every sentence carries one identifiable claim or
step. Paragraph breaks signal a shift in focus.

Prefer active constructions: "We derive an upper bound" over "An upper bound is derived."
Reserve the passive for results independent of the authors: "The problem is ill-posed in
the sense of Hadamard."

Avoid vague intensifiers: "very", "quite", "rather", "highly", "extremely". Quantify
where possible: "the condition number grows as $\bigO{h^{-2}}$".

### 6.3 Mathematical Prose

Every displayed equation referred to subsequently must be labelled and introduced by a
complete grammatical sentence. Treat the equation as part of the sentence with appropriate
punctuation.

Define every symbol before or at its first use. When citing a result, state precisely which
part of the cited work is being used (e.g., `\citet[Theorem~2.1]{engl1996}`). Avoid vague
attributions such as "as shown in [3]".

Place quantitative conditions in numbered `assumption` environments rather than burying them
inside theorem statements, whenever those conditions are reusable across multiple results.

### 6.4 Consistency Checks

Before outputting any document, verify:
1. Every symbol introduced in the preamble is used at least once in the body; remove unused macros.
2. The same physical quantity uses the same symbol throughout; no silent switching.
3. All theorem environments are closed; all proofs end with `\end{proof}`.
4. Every `\begin{}` has a matching `\end{}`.
5. No conflicting packages (e.g., `amsmath` not loaded twice; `algorithm2e` and `algorithmic` not both loaded).

---

## Step 7. Pre-Output Quality Checklist

Run through all items below before producing the final output.

### Compilability
- Every `\begin{}` has a matching `\end{}`.
- No undefined control sequences.
- All required packages loaded in the preamble.
- No package conflicts.

### Label Consistency
- Every numbered environment has a `\label{}`.
- Every `\ref{}`, `\eqref{}`, `\cref{}` resolves to an existing label.
- No label defined more than once.

### Notation Consistency
- Same symbol used for the same object throughout.
- Step sizes, regularisation parameters, and iterates follow Section 3.3 conventions.
- All macros used in the body are defined in the preamble.

### Mathematical Correctness
- Inequalities point in the correct direction.
- Convergence rates and complexity bounds are dimensionally consistent.
- Cited results are used correctly and not misrepresented.
- Proof steps follow logically from stated assumptions.

### Bibliography Completeness
- **Option A:** Every `\cite{}` key has a fully populated `\bibitem{}`; no orphan entries.
- **Option B1:** Every cited key exists in the `.bib` file; `\bibliographystyle{}` and `\bibliography{}` present; `biblatex` not loaded.
- **Option B2:** Every cited key in the `.bib` file; `\printbibliography` present; no `\bibliographystyle{}` or `\bibliography{}`; `natbib` not loaded.

### Margin Safety
- No plain `tabular` with more than 4 columns unless wrapped in `tabularx` or `\resizebox`.
- Every `pgfplots` axis uses a relative width; no absolute `cm` or `pt` values.
- Every free-standing wide `tikzpicture` wrapped in `\adjustbox{max width=\textwidth}`.
- Side-by-side subfigures use `width=\linewidth` inside their `subfigure` environment.

### Writing Quality
- No em dashes, en dashes (outside LaTeX ranges), or prose ellipses.
- No contractions, exclamation marks, or rhetorical questions.
- No prohibited transitional intensifiers.
- Every symbol defined before or at first use.
- Every displayed equation introduced by a complete grammatical sentence with correct punctuation.

---

## Step 8. Handling Ambiguous or Incomplete Requests

### Missing proof
If the user requests "write up this theorem" without providing a proof, produce the theorem
statement and a `\begin{proof}...\end{proof}` scaffold with `\todo{}` markers. Do not
construct a proof that was not provided.

### Partial notation
If the user provides incomplete notation or an unfinished derivation, ask exactly one
clarifying question before proceeding. Do not guess at notation.

### Mixed content
If the request combines document and snippet content, produce a complete document and include
all elements within it.

### User-provided notation
If the user provides existing mathematical content, preserve their notation and mathematical
choices exactly. Normalise only formatting and typographic conventions; do not alter the
mathematics or rename symbols.

---

## Step 9. Domain-Specific Reference Entries

The following `\bibitem` entries cover foundational works in EIT, regularisation theory,
inverse problems, numerical analysis, deep learning, PINNs, and numerical optimisation.
Insert directly when the corresponding key is cited.

```latex
% --- Inverse problems and EIT ---

\bibitem{calderon1980}
A.~P.~Calder\'{o}n,
``On an inverse boundary value problem,''
in \textit{Seminar on Numerical Analysis and its Applications to Continuum Physics},
Rio de Janeiro, Brazil, 1980, pp.~65--73.

\bibitem{cheney1990}
M.~Cheney, D.~Isaacson, J.~C.~Newell, S.~Simske, and J.~Goble,
``NOSER: An algorithm for solving the inverse conductivity problem,''
\textit{International Journal of Imaging Systems and Technology},
vol.~2, no.~2, pp.~66--75, 1990.

\bibitem{engl1996}
H.~W.~Engl, M.~Hanke, and A.~Neubauer,
\textit{Regularization of Inverse Problems},
Kluwer Academic Publishers, Dordrecht, 1996.

\bibitem{vogel2002}
C.~R.~Vogel,
\textit{Computational Methods for Inverse Problems},
SIAM, Philadelphia, PA, 2002.

\bibitem{kaltenbacher2008}
B.~Kaltenbacher, A.~Neubauer, and O.~Scherzer,
\textit{Iterative Regularization Methods for Nonlinear Ill-Posed Problems},
de Gruyter, Berlin, 2008.

\bibitem{borcea2002}
L.~Borcea,
``Electrical impedance tomography,''
\textit{Inverse Problems},
vol.~18, no.~6, pp.~R99--R136, 2002.

% --- Regularisation and optimisation ---

\bibitem{tikhonov1943}
A.~N.~Tikhonov,
``On the stability of inverse problems,''
\textit{Doklady Akademii Nauk SSSR},
vol.~39, no.~5, pp.~195--198, 1943.

\bibitem{rudin1992}
L.~I.~Rudin, S.~Osher, and E.~Fatemi,
``Nonlinear total variation based noise removal algorithms,''
\textit{Physica D: Nonlinear Phenomena},
vol.~60, no.~1--4, pp.~259--268, 1992.

\bibitem{nesterov1983}
Y.~Nesterov,
``A method for solving the convex programming problem with convergence rate
$\mathcal{O}(1/k^2)$,''
\textit{Doklady Akademii Nauk SSSR},
vol.~269, no.~3, pp.~543--547, 1983.

\bibitem{nocedal2006}
J.~Nocedal and S.~J.~Wright,
\textit{Numerical Optimization},
2nd~ed.,
Springer, New York, NY, 2006.

\bibitem{boyd2004}
S.~Boyd and L.~Vandenberghe,
\textit{Convex Optimization},
Cambridge University Press, Cambridge, 2004.

% --- Deep learning for inverse problems ---

\bibitem{adler2017}
J.~Adler and O.~\"{O}ktem,
``Solving ill-posed inverse problems using iterative deep neural networks,''
\textit{Inverse Problems},
vol.~33, no.~12, p.~124007, 2017.

\bibitem{hamilton2018}
S.~J.~Hamilton and A.~Hauptmann,
``Deep d-bar: Real-time electrical impedance tomography imaging with deep
neural networks,''
\textit{IEEE Transactions on Medical Imaging},
vol.~37, no.~10, pp.~2367--2377, 2018.

\bibitem{fan2020}
Y.~Fan and L.~Ying,
``Solving traveltime tomography with deep learning,''
\textit{Research in the Mathematical Sciences},
vol.~7, no.~3, p.~17, 2020.

\bibitem{kingmaba2015}
D.~P.~Kingma and J.~Ba,
``Adam: A method for stochastic optimization,''
in \textit{Proceedings of the 3rd International Conference on Learning
Representations (ICLR)},
San Diego, CA, USA, 2015.

\bibitem{goodfellow2016}
I.~Goodfellow, Y.~Bengio, and A.~Courville,
\textit{Deep Learning},
MIT Press, Cambridge, MA, 2016.

% --- PINNs and scientific machine learning ---

\bibitem{raissi2019}
M.~Raissi, P.~Perdikaris, and G.~E.~Karniadakis,
``Physics-informed neural networks: A deep learning framework for solving forward
and inverse problems involving nonlinear partial differential equations,''
\textit{Journal of Computational Physics},
vol.~378, pp.~686--707, 2019.

\bibitem{lagaris1998}
I.~E.~Lagaris, A.~Likas, and D.~I.~Fotiadis,
``Artificial neural networks for solving ordinary and partial differential equations,''
\textit{IEEE Transactions on Neural Networks},
vol.~9, no.~5, pp.~987--1000, 1998.

% --- Numerical analysis ---

\bibitem{golub2013}
G.~H.~Golub and C.~F.~Van~Loan,
\textit{Matrix Computations},
4th~ed.,
Johns Hopkins University Press, Baltimore, MD, 2013.

\bibitem{trefethen1997}
L.~N.~Trefethen and D.~Bau,
\textit{Numerical Linear Algebra},
SIAM, Philadelphia, PA, 1997.

\bibitem{brenner2008}
S.~C.~Brenner and L.~R.~Scott,
\textit{The Mathematical Theory of Finite Element Methods},
3rd~ed.,
Springer, New York, NY, 2008.
```

---

## Step 10. Complete Document Template

Replace all placeholder text with actual content.

```latex
\documentclass[11pt,a4paper]{article}

% [Insert full preamble from Step 2 here]

\title{Title of the Document}
\author{Author Name\\
  Department, Institution, City, Country\\
  \texttt{email@example.com}}
\date{\today}

\begin{document}

\maketitle

\begin{abstract}
  A concise summary (150--250 words) of the problem, main results, methods used, and
  significance. Written in the third person; past tense for results, present tense for
  conclusions.
\end{abstract}

\section{Introduction}\label{sec:intro}

\section{Problem Formulation}\label{sec:problem}

\section{Assumptions}\label{sec:assumptions}

\begin{assumption}\label{ass:main}
  [State the assumption precisely.]
\end{assumption}

\section{Main Results}\label{sec:results}

\begin{theorem}[Descriptive title]\label{thm:main}
  Under \cref{ass:main}, \ldots
\end{theorem}

\begin{proof}
  [Proof.]
\end{proof}

\section{Numerical Experiments}\label{sec:experiments}

\section{Conclusion}\label{sec:conclusion}

% --- CHOOSE ONE ending below; delete the other two ---

% OPTION A: Self-contained
\begin{thebibliography}{99}
% [Populate \bibitem entries from Step 5 Option A and Step 9.]
\end{thebibliography}

% OPTION B1: BibTeX + natbib
% \bibliographystyle{plainnat}
% \bibliography{refs}

% OPTION B2: BibLaTeX + Biber
% \printbibliography

\end{document}
```

---

## Step 10B. Complete Beamer Presentation Template (Beamer Mode Only)

This is the canonical full-file scaffold for a Beamer presentation. Populate every
section from the user's manuscript or insert `\todo{}` placeholders for missing content.
Omit or reorder any section the user does not need (see Section 3B.1 for the section
table). Compile with `pdflatex` (two passes to resolve cross-references).

```latex
\documentclass[aspectratio=169,10pt]{beamer}

% ---------------------------------------------------------------
% Theme — substitute any standard Beamer theme name
% ---------------------------------------------------------------
\usetheme{Madrid}
% \usecolortheme{dolphin}

% ---------------------------------------------------------------
% Core mathematics
% ---------------------------------------------------------------
\usepackage{amsmath, amssymb, amsthm, mathtools}
\usepackage{bm}

% ---------------------------------------------------------------
% Algorithms
% ---------------------------------------------------------------
\usepackage[ruled,vlined,linesnumbered]{algorithm2e}

% ---------------------------------------------------------------
% Graphics
% ---------------------------------------------------------------
\usepackage{graphicx}
\usepackage{tikz}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}
\usepackage{subfigure}

% ---------------------------------------------------------------
% Tables
% ---------------------------------------------------------------
\usepackage{booktabs}
\usepackage{tabularx}

% ---------------------------------------------------------------
% Footnote citation control
% ---------------------------------------------------------------
\usepackage{perpage}
\MakePerPage{footnote}

% ---------------------------------------------------------------
% Miscellaneous
% ---------------------------------------------------------------
\usepackage{xcolor}
\usepackage{multicol}

% ---------------------------------------------------------------
% Notation macros
% ---------------------------------------------------------------
\newcommand{\norm}[1]{\left\lVert #1 \right\rVert}
\newcommand{\ip}[2]{\left\langle #1,\, #2 \right\rangle}
\newcommand{\abs}[1]{\left\lvert #1 \right\rvert}
\newcommand{\grad}{\nabla}
\newcommand{\R}{\mathbb{R}}
\newcommand{\E}{\mathbb{E}}
\newcommand{\bigO}[1]{\mathcal{O}\!\left(#1\right)}
\newcommand{\xk}{x_k}
\newcommand{\alphak}{\alpha_k}
\newcommand{\bx}{\mathbf{x}}
\newcommand{\bJ}{\mathbf{J}}

% ---------------------------------------------------------------
% Presentation metadata
% ---------------------------------------------------------------
\title[Short Title]{Full Title of the Presentation}
\subtitle{Subtitle or Paper Title (if applicable)}
\author[H.~Mohammad]{Hassan Mohammad}
\institute[BUK]{%
  Numerical Optimisation Research Group\\
  Department of Mathematical Sciences\\
  Faculty of Physical Sciences\\
  Bayero University, Kano, Nigeria
}
\date{\today}

% ---------------------------------------------------------------
% Optional: re-show TOC at each section start
% ---------------------------------------------------------------
% \AtBeginSection[]{
%   \begin{frame}{Outline}
%     \tableofcontents[currentsection, hideallsubsections]
%   \end{frame}
% }

\begin{document}

% =============================================================
% FRAME 1 — Title Page
% =============================================================
\begin{frame}
  \titlepage
\end{frame}

% =============================================================
% FRAME 2 — Table of Contents
% =============================================================
\begin{frame}{Outline}
  \tableofcontents[hideallsubsections]
\end{frame}

% =============================================================
% SECTION 1: Introduction
% =============================================================
\section{Introduction}

\begin{frame}{Introduction}
  \begin{itemize}
    \item \todo{State the problem and its practical or theoretical motivation.}
    \item \todo{Describe the specific problem considered in this work.}
    \item \todo{Outline the main contributions.}
  \end{itemize}
\end{frame}

\begin{frame}{Problem Formulation}
  We consider the problem of finding $x^* \in \R^n$ such that
  \[
    F(x^*) = 0,
  \]
  where $F \colon \R^n \to \R^n$ \todo{[describe properties of $F$]}.

  The iterative scheme has the form
  \[
    x_{k+1} = x_k + \alphak d_k, \quad k = 0, 1, 2, \ldots,
  \]
  where $\alphak > 0$ is the steplength and $d_k$ is the search direction.
\end{frame}

% =============================================================
% SECTION 2: Related Work / Literature Review / Motivation
% =============================================================
\section{Related Work}

\begin{frame}{Related Work}
  \begin{itemize}
    \item \todo{Author(s) (Year)\footnotemark{} — brief description of contribution.}
    \item \todo{Author(s) (Year)\footnotemark{} — brief description.}
    \item \todo{Author(s) (Year)\footnotemark{} — brief description.}
  \end{itemize}
  \footnotetext{\todo{Full citation 1.}}
  \footnotetext{\todo{Full citation 2.}}
  \footnotetext{\todo{Full citation 3.}}
\end{frame}

\begin{frame}{Motivation and Gap}
  \begin{itemize}
    \item \todo{Identify the limitation in existing methods that this work addresses.}
    \item \todo{State why the proposed approach is needed.}
  \end{itemize}
\end{frame}

% =============================================================
% SECTION 3: Methodology / Algorithm
% =============================================================
\section{Methodology}

\begin{frame}{Assumptions}
  \begin{block}{Assumption 1}
    \todo{State Assumption 1 precisely (e.g., Lipschitz continuity of $F$).}
  \end{block}
  \vspace{0.5em}
  \begin{block}{Assumption 2}
    \todo{State Assumption 2 precisely (e.g., boundedness of the Jacobian).}
  \end{block}
\end{frame}

\begin{frame}{Search Direction}
  The search direction $d_k$ is defined by
  \[
    d_k = \begin{cases}
      \todo{[initial direction]}, & k = 0, \\
      \todo{[recursive formula]}, & k \geq 1,
    \end{cases}
  \]
  where \todo{define all parameters}.
\end{frame}

\begin{frame}[fragile]{Algorithm}
  \begin{algorithm}[H]
  \caption{\todo{AlgorithmName}}\label{alg:main}
  \KwIn{\todo{Inputs: initial point, tolerances, parameters}}
  \KwOut{\todo{Output}}
  Set $k \leftarrow 0$\;
  \While{$\norm{F(x_k)} > \varepsilon$}{
    Compute search direction $d_k$\;
    Find steplength $\alphak$ via line search\;
    $x_{k+1} \leftarrow x_k + \alphak d_k$\;
    $k \leftarrow k + 1$\;
  }
  \Return{$x_k$}\;
  \end{algorithm}
\end{frame}

% =============================================================
% SECTION 4: Convergence Analysis
% =============================================================
\section{Convergence Analysis}

\begin{frame}{Preliminary Lemmas}
  \begin{lemma}\label{lem:prelim1}
    \todo{State Lemma 1.}
  \end{lemma}
  \vspace{0.5em}
  \begin{lemma}\label{lem:prelim2}
    \todo{State Lemma 2.}
  \end{lemma}
\end{frame}

\begin{frame}{Main Convergence Theorem}
  \begin{theorem}[Global Convergence]\label{thm:global}
    \todo{Under Assumptions 1 and 2, if $\{x_k\}$ is generated by the algorithm, then
    state the convergence result.}
  \end{theorem}
  \vspace{0.5em}
  \begin{proof}[Proof sketch]
    \todo{Outline the main steps of the proof.}
  \end{proof}
\end{frame}

\begin{frame}{Rate of Convergence}
  \begin{theorem}[R-linear Convergence]\label{thm:rate}
    \todo{State the rate-of-convergence theorem. For example:
    there exist $C > 0$ and $\mu \in (0,1)$ such that
    $\norm{x_k - x^*} \leq C\mu^k$.}
  \end{theorem}
\end{frame}

% =============================================================
% SECTION 5: Numerical Experiments
% =============================================================
\section{Numerical Experiments}

\begin{frame}{Experimental Setup}
  \begin{itemize}
    \item \todo{Describe the test problems, dimension, and parameter settings.}
    \item \todo{List the competing methods.}
    \item \todo{State the stopping criterion and hardware/software details.}
  \end{itemize}
\end{frame}

\begin{frame}{Performance Profiles}
  \begin{columns}[T]
    \begin{column}{0.48\textwidth}
      \begin{figure}
        \includegraphics[width=\linewidth]{\todo{fig\_iterations}}
        \caption{Number of iterations}
      \end{figure}
    \end{column}
    \begin{column}{0.48\textwidth}
      \begin{figure}
        \includegraphics[width=\linewidth]{\todo{fig\_fevals}}
        \caption{Function evaluations}
      \end{figure}
    \end{column}
  \end{columns}
  {\small Dolan--Mor\'{e} performance profiles;\footnotemark{} higher curve = better.}
  \footnotetext{E.~D.~Dolan and J.~J.~Mor\'{e},
    ``Benchmarking optimization software with performance profiles,''
    \textit{Math.\ Program.}, vol.~91, no.~2, pp.~201--213, 2002.}
\end{frame}

\begin{frame}{Results Summary}
  \begin{table}
    \centering
    \small
    \begin{tabular}{lrrr}
      \toprule
      Method & Iter. & F-Evals & CPU (s) \\
      \midrule
      \todo{Method A} & \todo{--} & \todo{--} & \todo{--} \\
      \todo{Method B} & \todo{--} & \todo{--} & \todo{--} \\
      \bottomrule
    \end{tabular}
    \caption{\todo{Caption describing the table.}}
  \end{table}
\end{frame}

% =============================================================
% SECTION 6: Conclusion / Further Research
% =============================================================
\section{Conclusion}

\begin{frame}{Concluding Remarks}
  \begin{itemize}
    \item \todo{Summarise what was presented.}
    \item \todo{State the main theoretical guarantee established.}
    \item \todo{Comment on practical performance.}
  \end{itemize}
\end{frame}

\begin{frame}{Future Research Questions}
  \begin{enumerate}
    \item \todo{Open question 1.}
    \item \todo{Open question 2.}
    \item \todo{Open question 3.}
  \end{enumerate}
\end{frame}

% =============================================================
% CLOSING FRAME
% =============================================================
\begin{frame}[plain]
  \begin{center}
    {\Large \textbf{Thank You}}\\[1em]
    \todo{Author name}\\
    \texttt{\todo{email@institution.edu}}
  \end{center}
\end{frame}

\end{document}
```

---

## Step 10C. Beamer Pre-Output Quality Checklist (Beamer Mode Only)

Run through all items below before delivering the final Beamer `.tex` file.

### Compilability
- `\documentclass{beamer}` is present; NOT `\documentclass{article}`.
- The `perpage` package is loaded and `\MakePerPage{footnote}` is called.
- Every frame with `algorithm2e` or verbatim content uses `[fragile]`.
- No `geometry`, `cleveref`, or `natbib` packages are loaded (incompatible with beamer).

### Frame Structure
- Every `\begin{frame}` has a matching `\end{frame}`.
- Every frame has a non-empty title (`\begin{frame}{Title}` or `\frametitle{}`).
- No frame contains more than five itemize entries at the top level.
- No frame body exceeds approximately 10 lines of display content (split if necessary).

### Footnote Citation Consistency
- Every `\footnotemark` on a slide has a matching `\footnotetext` within the SAME frame.
- No `\footnotetext` is present without a corresponding `\footnotemark` on the same slide.
- No bibliography section (`\begin{thebibliography}`, `\printbibliography`) is present.

### Mathematical Correctness
- All macros used in the body are defined in the preamble.
- Same notation used throughout; no silent symbol reassignment between frames.
- Every theorem, lemma, and definition that comes from the user's manuscript is reproduced
  faithfully; no content is fabricated.

### Placeholder Hygiene
- All `\todo{}` markers represent genuinely missing information (not leftover from
  copy-paste). If the user provided the content, it must appear — not a `\todo{}`.

---

## Reference Files

- `references/preamble.md` — Legacy preamble reference. The canonical preamble is now
  embedded in Step 2. Consult only for additional macro variants not covered in Step 2.
