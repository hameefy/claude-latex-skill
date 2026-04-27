---
name: latex
description: >
  Produce high-quality, compilable LaTeX for researchers in computational and applied
  mathematics. Trigger whenever the user requests a theorem, lemma, proposition, corollary,
  definition, proof, convergence analysis, algorithm pseudocode, numerical results table,
  TikZ figure, derivation, literature review, or any structured academic document. Also
  trigger for "write up", "typeset", "format in LaTeX", "produce a .tex file",
  "generate a report", or "give me the LaTeX for". Covers numerical optimisation, deep
  learning theory, EIT, regularisation, PINNs, and numerical analysis. Output must be
  immediately compilable, mathematically rigorous, and free of AI-characteristic phrasing.
  Two modes: (1) DOCUMENT MODE — full standalone .tex file; (2) SNIPPET MODE — raw body-only
  LaTeX for fragments to paste into the user's own template.
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

Before writing anything, determine which of the two output modes applies.

### Document Mode

Produce a complete, standalone `.tex` file (preamble through `\end{document}`) when the
request involves any of the following:

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

**When in doubt**, ask: "Should I produce a complete standalone document, or a snippet to
paste into your template?"

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

## Reference Files

- `references/preamble.md` — Legacy preamble reference. The canonical preamble is now
  embedded in Step 2. Consult only for additional macro variants not covered in Step 2.