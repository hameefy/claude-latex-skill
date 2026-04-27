# LaTeX Skill for Claude

A Claude skill for producing high-quality, immediately compilable LaTeX output in
computational and applied mathematics. Designed for researchers who work with formal
theorem–proof structures, optimisation algorithms, convergence analysis, numerical
experiments, and scientific document preparation.

---

## Overview

This skill instructs Claude to behave as a rigorous LaTeX author, producing output that
meets the stylistic and mathematical standards of journals such as:

- *SIAM Journal on Scientific Computing*
- *Inverse Problems*
- *Mathematics of Computation*
- *Journal of Computational Physics*

It covers two output modes and ten structured production steps, from request
classification through preamble construction, mathematical content standards, document
structure, bibliography management, writing quality enforcement, and a pre-output quality
checklist.

---

## Research Domains Covered

The skill is calibrated for the following areas, with domain-specific notation macros,
algorithm templates, and a curated reference library built in:

- Numerical optimisation (gradient descent, conjugate gradient, proximal methods, Adam)
- Deep learning theory and learning dynamics
- Electrical impedance tomography (EIT) and data-driven inversion
- Regularisation theory (Tikhonov, total variation, iterative regularisation)
- Physics-informed neural networks (PINNs)
- Numerical linear algebra and finite element methods
- Convergence analysis and complexity theory

---

## Output Modes

### Document Mode

Produces a complete, standalone `.tex` file (full preamble through `\end{document}`)
for any of the following:

| Request type | Example trigger |
|---|---|
| Theorem, lemma, proposition, corollary | "Write up this convergence theorem" |
| Derivation or multi-step argument | "Typeset this gradient descent derivation" |
| Convergence analysis | "Produce a .tex file for this rate analysis" |
| Algorithm with theoretical justification | "Format this IRGN algorithm in LaTeX" |
| Literature review or introduction | "Write the introduction section as LaTeX" |
| Technical note or preprint draft | "Generate a report from these results" |

The `.tex` file is saved and presented for download.

### Snippet Mode

Produces raw LaTeX body content only (no preamble, no `\documentclass`) for:

- Single equations or aligned systems
- Algorithm pseudocode blocks in isolation
- Numerical results tables
- TikZ figures or pgfplots graphs
- Any fragment to be pasted into an existing template

Snippet output is delivered as a labelled code block in the chat.

---

## Standard Preamble

The skill ships with a fixed, conflict-free preamble covering:

| Category | Packages |
|---|---|
| Core mathematics | `amsmath`, `amssymb`, `amsthm`, `mathtools`, `bm`, `dsfont` |
| Algorithms | `algorithm2e` (ruled, vlined, line-numbered) |
| Figures and plots | `tikz`, `pgfplots` (compat 1.18), `subfigure`, `adjustbox` |
| Tables | `booktabs`, `tabularx`, `siunitx` |
| Cross-referencing | `natbib`, `hyperref`, `cleveref` |
| Typography | `microtype`, `parskip`, `geometry` (2.5 cm margins) |
| Utilities | `xcolor`, `todonotes` |

### Built-in Notation Macros

The preamble defines macros for the most common objects in applied mathematics:

```latex
\norm{x}          % ||x||
\normF{A}         % ||A||_F  (Frobenius)
\ip{x}{y}         % <x, y>
\grad             % nabla
\Hess             % nabla^2
\bigO{n}          % O(n)  (never \text{O})
\R, \N, \C        % blackboard bold sets
\E, \Prob         % expectation, probability
\Lp{2}, \Sob{1}{} % function spaces
\bA, \bx, \bJ     % bold matrices and vectors
\Forward          % forward operator F (EIT)
\Reg              % regulariser R
\Tikhonov{F(s)}{V}{\lambda}  % Tikhonov functional
\alphak, \etak    % step sizes alpha_k, eta_k
```

---

## Theorem Environments

All `amsthm` environments are pre-configured with consistent numbering and label prefixes:

| Environment | Label prefix | Numbering style |
|---|---|---|
| `theorem` | `thm:` | Shared counter `[section]` |
| `lemma` | `lem:` | Shared with theorem |
| `proposition` | `prop:` | Shared with theorem |
| `corollary` | `cor:` | Shared with theorem |
| `definition` | `def:` | Shared with theorem |
| `assumption` | `ass:` | Independent counter |
| `example` | `ex:` | Shared with theorem |
| `remark` | `rem:` | Shared with theorem |

---

## Bibliography Workflows

The skill supports three workflows. The user is asked to choose before the reference
section is written; if no preference is stated, Option A is used by default.

| Option | Backend | When to use |
|---|---|---|
| **A** — Self-contained | Inline `thebibliography` | Quick notes, isolated proofs, single-file submissions |
| **B1** — BibTeX | `natbib` + `.bib` + `bibtex` | Projects with a shared `.bib` library; IEEE/SIAM style files |
| **B2** — BibLaTeX | `biblatex` + `.bib` + `biber` | BibLaTeX-native styles; Overleaf with LuaLaTeX or XeLaTeX |

A curated library of 19 ready-to-insert `\bibitem` entries is built into the skill,
covering foundational works in EIT (Calderón 1980, Cheney 1990, Borcea 2002),
regularisation (Tikhonov 1943, Rudin–Osher–Fatemi 1992, Engl–Hanke–Neubauer 1996),
optimisation (Nesterov 1983, Nocedal–Wright 2006), deep learning (Kingma–Ba 2015,
Goodfellow 2016), PINNs (Raissi 2019), and numerical analysis (Golub–Van Loan 2013,
Trefethen–Bau 1997).

---

## Writing Quality Enforcement

The skill enforces academic writing standards that eliminate common markers of
automatically generated text:

**Prohibited in all output:**
- Em dashes (`---`) and en dashes in prose (only `--` in numeric ranges and compound proper nouns)
- Contractions (`it's`, `don't`, `we've`)
- Exclamation marks
- Rhetorical questions
- Transitional intensifiers: "importantly", "crucially", "notably", "it is worth noting that"
- First-person singular (`I`); "we" is used throughout, including single-author work
- Noun stacks of three or more consecutive nouns used as adjectives

**Required in all output:**
- Every displayed equation introduced by a complete grammatical sentence
- Every symbol defined before or at its first use
- Active constructions preferred; passive reserved for results independent of the authors
- Quantified claims rather than vague intensifiers

---

## Pre-Output Quality Checklist

Before every output, the skill verifies:

1. **Compilability** — every `\begin{}` matched with `\end{}`; no undefined macros; no package conflicts
2. **Label consistency** — every numbered environment labelled; every `\ref`, `\eqref`, `\cref` resolved
3. **Notation consistency** — same symbol used for the same object throughout; no silent switching
4. **Mathematical correctness** — inequalities directionally correct; cited results used accurately; proof steps logically valid
5. **Bibliography completeness** — every cited key has a fully populated entry; no orphan `\bibitem`s
6. **Margin safety** — no table exceeds `\linewidth`; all figures use relative widths; wide diagrams wrapped in `\adjustbox`
7. **Writing quality** — all prohibited constructs absent; all symbols defined before use

---

## Trigger Phrases

The skill activates on any of the following (and similar) phrases:

```
"write this as LaTeX"          "typeset this proof"
"convert to LaTeX"             "produce a .tex file"
"LaTeX for this theorem"       "write a LaTeX document"
"LaTeX pseudocode"             "LaTeX table"
"TikZ figure"                  "typeset this derivation"
"write up this theorem"        "formalise this derivation"
"format in LaTeX"              "generate a report"
"give me the LaTeX for"
```

It also activates without an explicit LaTeX mention when the user submits mathematical
content (a theorem statement, an algorithm, experimental results) and asks to "write it up"
or "format it".

---

## Repository Structure

```
latex-skill/
├── SKILL.md              # Main skill file — all production rules
├── references/
│   └── preamble.md       # Legacy preamble reference (supplementary)
├── README.md             # This file
└── LICENSE               # MIT Licence
```

The canonical preamble is embedded directly in `SKILL.md` (Step 2). The
`references/preamble.md` file is retained for supplementary macro variants.

---

## Installation

### From a Release (recommended)

1. Go to the [Releases](../../releases) page of this repository.
2. Download `latex.skill` from the latest release.
3. In Claude Code, run:
   ```bash
   claude skill install latex.skill
   ```

### From source

```bash
git clone https://github.com/YOUR_USERNAME/latex-skill.git
cd latex-skill
# Then install via Claude Code:
claude skill install .
```

---

## Usage Examples

**Generate a standalone convergence theorem document:**
> "Write up this convergence theorem for gradient descent as a LaTeX document."

**Produce a pseudocode snippet:**
> "Give me the LaTeX pseudocode for the IRGN algorithm."

**Typeset a numerical results table:**
> "Format these experimental results as a LaTeX table with booktabs."

**Draft a literature review section:**
> "Write the related work section of my EIT paper in LaTeX, citing Calderón 1980 and Engl 1996."

**Produce a TikZ convergence plot:**
> "Generate a TikZ figure showing the convergence of methods A and B on a log scale."

---

## Compatibility

| Requirement | Detail |
|---|---|
| Claude version | Claude Sonnet 4 or later (Claude Code) |
| LaTeX engine | pdfLaTeX (primary); LuaLaTeX and XeLaTeX supported for BibLaTeX/Biber workflow |
| TeX distribution | TeX Live 2022 or later; MiKTeX 22 or later |
| pgfplots | compat = 1.18 or later |

---

## Licence

This skill is released under the [MIT Licence](LICENSE). You are free to use, modify, and
redistribute it, provided the original copyright notice is retained.

---

## Author

**Hassan Mohammad**
Researcher in numerical optimisation and applied mathematics.
Specialising in optimisation algorithm design, convergence theory, deep learning theory,
and inverse problems.
Contact: [hameefy247@gmail.com](mailto:hameefy247@gmail.com)

---

## Contributing

Contributions are welcome. If you find an error in the preamble, a missing reference entry,
or a domain that the skill does not currently cover well, please open an issue or submit a
pull request. When submitting changes to `SKILL.md`, ensure that:

- The preamble in Step 2 remains conflict-free and compilable.
- New macros follow the naming conventions established in Step 3.3.
- Any new reference entries in Step 9 use the `\bibitem` format specified in Step 5.
- The pre-output checklist in Step 7 remains complete and internally consistent.
