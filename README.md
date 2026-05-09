![LaTeX Skill for Claude](banner.svg)

# LaTeX Skill for Claude

A Claude skill for producing high-quality, immediately compilable LaTeX output in
computational and applied mathematics. Designed for researchers who work with formal
theorem–proof structures, optimisation algorithms, convergence analysis, numerical
experiments, academic seminar presentations, and scientific document preparation.

---

## Overview

This skill instructs Claude to behave as a rigorous LaTeX author, producing output that
meets the stylistic and mathematical standards of journals and conferences such as:

- *SIAM Journal on Scientific Computing*
- *Inverse Problems*
- *Mathematics of Computation*
- *Journal of Computational Physics*

It covers **three output modes** and fourteen structured production steps, from request
classification through preamble construction, mathematical content standards, document
structure, bibliography management, writing quality enforcement, a pre-output quality
checklist, and full Beamer presentation support.

---

## What is New — Beamer Mode (v2.0)

Version 2.0 adds **BEAMER MODE**: a complete third output mode for producing academic
seminar and conference presentation slides. Beamer Mode is fully independent of the
article preamble, ships with its own dedicated steps in `SKILL.md`, and covers the
complete lifecycle of an academic talk — from title page through conclusion and open
questions.

Key additions in this release:

| Addition | `SKILL.md` location | Description |
|---|---|---|
| Beamer classification logic | Step 1 | BEAMER MODE is now checked *first*, before Document or Snippet mode |
| Beamer preamble | Step 2B | Completely separate `\documentclass{beamer}` preamble; incompatible article packages excluded |
| Beamer content standards | Step 3B | Seven subsections covering section scaffold, footnote citations, theorem blocks, algorithms, numerical results, frame rules, and TOC slides |
| Complete Beamer template | Step 10B | Full compilable `.tex` scaffold with all eight default sections and `\todo{}` placeholders |
| Beamer pre-output checklist | Step 10C | Dedicated quality checklist for compilability, frame structure, citation consistency, and placeholder hygiene |

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

### Beamer Mode ← New in v2.0

Produces a complete Beamer presentation (`.tex` file with `\documentclass{beamer}`) for
any academic talk, seminar, or conference presentation. Triggered by:

| Request type | Example trigger |
|---|---|
| Seminar or conference talk | "Prepare slides for my talk on conjugate gradient methods" |
| Presentation from a manuscript | "Make a Beamer presentation from this paper" |
| Talk on a topic | "Create a slide deck on derivative-free methods for nonlinear equations" |
| Named slide sections | "Give me the convergence results slides for my seminar" |
| Explicit Beamer request | "Write a Beamer presentation with the Madrid theme" |

**Content sourcing:**
- If a complete manuscript is provided, each slide section is populated directly from it,
  preserving all mathematical notation exactly.
- If only a title, abstract, or partial notes are provided, a fully structured template
  is generated with `\todo{}` placeholders throughout.
- Theorems, lemmas, and numerical results are never fabricated; missing content is always
  flagged with `\todo{}`.

**Default section scaffold** (all sections are flexible — any may be omitted or reordered):

| Section | Default `\section{}` name | Typical frame count |
|---|---|---|
| Title page | *(title frame, no section)* | 1 |
| Table of contents | *(TOC frame, no section)* | 1 |
| Introduction | `Introduction` | 2–3 |
| Literature review / Related work | `Related Work` | 2–3 |
| Method / Algorithm | `Methodology` | 3–5 |
| Convergence results | `Convergence Analysis` | 2–4 |
| Implementation / Numerical experiments | `Numerical Experiments` | 2–3 |
| Conclusion / Further research | `Conclusion` | 1–2 |

**Footnote citation style:** Beamer Mode uses `\footnotemark` / `\footnotetext{}` pairs
exclusively. There is no reference section at the end of the presentation; every cited
source appears as a footnote on the slide where it is first cited. The `perpage` package
resets the footnote counter on each slide automatically.

**Theme selection:** Default theme is `Madrid`. Any standard Beamer theme
(`Berlin`, `AnnArbor`, `Warsaw`, `Copenhagen`, `Frankfurt`, `Singapore`, `Boadilla`,
`CambridgeUS`, and others) can be substituted by naming it in the request. Colour themes
(`dolphin`, `beaver`, `crane`, `orchid`, etc.) are applied with `\usecolortheme{}` when
requested.

**Algorithm support:** Both `algorithm2e` (default, recommended for pseudocode with line
numbers) and `algorithmicx` are supported. Frames containing algorithm blocks automatically
receive the required `[fragile]` option.

**Theorem block style:** Beamer's built-in `theorem`, `lemma`, `corollary`,
`definition`, and `proof` environments are used by default. The `tcolorbox` package for
coloured framed boxes (matching the sample seminar slides) is available on explicit
request; it is not loaded by default.

**Aspect ratio:** Default is 16:9 (`aspectratio=169`). The 4:3 ratio (`aspectratio=43`)
is available on request.

---

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

---

### Snippet Mode

Produces raw LaTeX body content only (no preamble, no `\documentclass`) for:

- Single equations or aligned systems
- Algorithm pseudocode blocks in isolation
- Numerical results tables
- TikZ figures or pgfplots graphs
- Any fragment to be pasted into an existing template

Snippet output is delivered as a labelled code block in the chat.

---

## Preambles

The skill ships with two fully specified, conflict-free preambles. They are mutually
exclusive and are never mixed.

### Article Preamble (Document Mode — Step 2)

| Category | Packages |
|---|---|
| Core mathematics | `amsmath`, `amssymb`, `amsthm`, `mathtools`, `bm`, `dsfont` |
| Algorithms | `algorithm2e` (ruled, vlined, line-numbered) |
| Figures and plots | `tikz`, `pgfplots` (compat 1.18), `subfigure`, `adjustbox` |
| Tables | `booktabs`, `tabularx`, `siunitx` |
| Cross-referencing | `natbib`, `hyperref`, `cleveref` |
| Typography | `microtype`, `parskip`, `geometry` (2.5 cm margins) |
| Utilities | `xcolor`, `todonotes` |

### Beamer Preamble (Beamer Mode — Step 2B)

| Category | Packages / Settings |
|---|---|
| Document class | `\documentclass[aspectratio=169,10pt]{beamer}` |
| Theme | `\usetheme{Madrid}` (substitutable) |
| Core mathematics | `amsmath`, `amssymb`, `amsthm`, `mathtools`, `bm` |
| Algorithms | `algorithm2e` (primary); `algorithmicx` (alternative, commented out) |
| Figures | `graphicx`, `tikz`, `pgfplots`, `subfigure` |
| Tables | `booktabs`, `tabularx` |
| Footnote control | `perpage` + `\MakePerPage{footnote}` (per-slide counter reset) |
| Optional | `tcolorbox` (coloured theorem boxes, off by default); `multicol` |

Note: `geometry`, `cleveref`, `natbib`, and `hyperref` are **not** loaded in Beamer
Mode — they are incompatible with the beamer document class.

---

### Built-in Notation Macros

The following macros are defined in both preambles for consistency:

```latex
\norm{x}          % ||x||
\normF{A}         % ||A||_F  (Frobenius)  [article mode only]
\ip{x}{y}         % <x, y>
\grad             % nabla
\Hess             % nabla^2  [article mode only]
\bigO{n}          % O(n)  (never \text{O})
\R, \N, \C        % blackboard bold sets
\E, \Prob         % expectation, probability
\Lp{2}, \Sob{1}{} % function spaces  [article mode only]
\bA, \bx, \bJ     % bold matrices and vectors
\Forward          % forward operator F (EIT)  [article mode only]
\Reg              % regulariser R  [article mode only]
\Tikhonov{F(s)}{V}{\lambda}  % Tikhonov functional  [article mode only]
\alphak, \etak    % step sizes alpha_k, eta_k
```

---

## Theorem Environments

### Article Mode (amsthm)

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

### Beamer Mode

Beamer's built-in `theorem`, `lemma`, `corollary`, `definition`, `proof`, `block`,
`alertblock`, and `exampleblock` environments are used directly. They are automatically
styled by the chosen theme. The `tcolorbox` package for custom coloured frames is
available on explicit request.

---

## Bibliography Workflows

### Article Mode (Steps 5 and 9)

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

### Beamer Mode (Step 3B.2)

Beamer Mode does **not** use `\begin{thebibliography}`, `\printbibliography`, or any
end-of-document reference section. Citations appear exclusively as footnotes on the
slide where the source is first cited, using `\footnotemark` / `\footnotetext{}` pairs.

```latex
% Within the slide body:
...as shown by La Cruz et al.\footnotemark{}...

% Immediately before \end{frame}:
\footnotetext{W.~La Cruz, J.~Mart\'{i}nez, and M.~Raydan,
  ``Spectral residual method without gradient information,''
  \textit{Math.\ Comp.}, vol.~75, no.~255, pp.~1429--1448, 2006.}
```

---

## Writing Quality Enforcement

The skill enforces academic writing standards that eliminate common markers of
automatically generated text.

**Prohibited in all output:**

- Em dashes (`---`) and en dashes in prose (only `--` in numeric ranges and compound
  proper nouns such as Dolan--Moré)
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

## Pre-Output Quality Checklists

### Article Mode (Step 7)

Before every document, the skill verifies:

1. **Compilability** — every `\begin{}` matched with `\end{}`; no undefined macros; no package conflicts
2. **Label consistency** — every numbered environment labelled; every `\ref`, `\eqref`, `\cref` resolved
3. **Notation consistency** — same symbol used for the same object throughout; no silent switching
4. **Mathematical correctness** — inequalities directionally correct; cited results used accurately; proof steps logically valid
5. **Bibliography completeness** — every cited key has a fully populated entry; no orphan `\bibitem`s
6. **Margin safety** — no table exceeds `\linewidth`; all figures use relative widths; wide diagrams wrapped in `\adjustbox`
7. **Writing quality** — all prohibited constructs absent; all symbols defined before use

### Beamer Mode (Step 10C)

Additional checks applied to every Beamer output:

1. **Compilability** — `\documentclass{beamer}` present; `perpage` loaded; `[fragile]` on all algorithm frames; no incompatible packages (`geometry`, `cleveref`, `natbib`)
2. **Frame structure** — every frame has a non-empty title; no frame body exceeds ten lines of display content; no more than five top-level itemize entries per frame
3. **Footnote citation consistency** — every `\footnotemark` has a matching `\footnotetext` within the same frame; no dangling `\footnotetext` entries; no end-of-document bibliography
4. **Mathematical correctness** — all macros defined; notation consistent across frames; no content fabricated from outside the user's manuscript
5. **Placeholder hygiene** — all `\todo{}` markers represent genuinely missing information; provided content is never left as a placeholder

---

## Trigger Phrases

### Article and Snippet Modes

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

### Beamer Mode

```
"make slides for my talk"           "prepare a Beamer presentation"
"presentation on [topic]"           "slide deck for my seminar"
"conference talk slides"            "talk on [topic]"
"slides from this paper"            "seminar presentation"
"beamer slides"                     "create a presentation"
```

BEAMER MODE is always checked *first* in Step 1. Any request containing a presentation
or slide keyword is routed to Beamer Mode before Document or Snippet Mode is considered.

---

## Repository Structure

```
claude-latex-skill/
├── SKILL.md                  # Main skill file — all production rules
│                             #   Step 1    — Three-mode classification
│                             #   Step 2    — Article preamble
│                             #   Step 2B   — Beamer preamble (NEW)
│                             #   Step 3    — Article mathematical content standards
│                             #   Step 3B   — Beamer content standards (NEW)
│                             #   Step 4    — Article document structure
│                             #   Step 5    — Bibliography workflows
│                             #   Step 6    — Writing quality standards
│                             #   Step 7    — Article pre-output checklist
│                             #   Step 8    — Handling ambiguous requests
│                             #   Step 9    — Domain-specific reference entries
│                             #   Step 10   — Complete article template
│                             #   Step 10B  — Complete Beamer template (NEW)
│                             #   Step 10C  — Beamer pre-output checklist (NEW)
├── references/
│   └── preamble.md           # Legacy preamble reference (supplementary)
├── banner.svg                # Repository banner image
├── latex.skill               # Packaged skill file for installation
├── README.md                 # This file
└── LICENSE                   # MIT Licence
```

The canonical article preamble is embedded in Step 2. The Beamer preamble is embedded in
Step 2B. The `references/preamble.md` file is retained for supplementary macro variants.

---

## Installation

### From a Release (recommended)

1. Go to the [Releases](https://github.com/hameefy/claude-latex-skill/releases) page of this repository.
2. Download `latex.skill` from the latest release.
3. In Claude Code, run:

```bash
claude skill install latex.skill
```

### From source

```bash
git clone https://github.com/hameefy/claude-latex-skill.git
cd claude-latex-skill
claude skill install .
```

---

## Usage Examples

### Article Mode

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

### Beamer Mode

**Full presentation from a manuscript:**
> "Prepare a Beamer presentation from my attached paper on diagonal conjugate gradient
> methods. Use the Madrid theme."

**Presentation from a title and notes:**
> "Make a slide deck for my seminar talk titled 'Global convergence of derivative-free
> methods for nonlinear equations'. I'll fill in the details."

**Specific section of slides:**
> "Give me the convergence analysis slides for my talk on spectral residual methods,
> based on these lemmas and theorems."

**Customised theme:**
> "Create a Beamer presentation on PRP-like algorithms using the AnnArbor theme and the
> dolphin colour theme."

**Algorithm slide:**
> "Add a slide showing the pseudocode for the dprp+ algorithm using algorithm2e."

---

## Compatibility

| Requirement | Detail |
|---|---|
| Claude version | Claude Sonnet 4 or later (Claude Code) |
| LaTeX engine — article | pdfLaTeX (primary); LuaLaTeX and XeLaTeX supported for BibLaTeX/Biber |
| LaTeX engine — Beamer | pdfLaTeX (recommended); LuaLaTeX supported |
| TeX distribution | TeX Live 2022 or later; MiKTeX 22 or later |
| pgfplots | compat = 1.18 or later |
| Beamer | Any version included in TeX Live 2022 or later |
| `perpage` package | Included in TeX Live and MiKTeX standard installations |

---

## Licence

This skill is released under the [MIT Licence](LICENSE). You are free to use, modify, and
redistribute it, provided the original copyright notice is retained.

---

## Author

**Hassan Mohammad**
Researcher in numerical optimisation and applied mathematics. Specialising in optimisation
algorithm design, convergence theory, deep learning theory, and inverse problems.
Contact: <hameefy247@gmail.com>

---

## Contributing

Contributions are welcome. If you find an error in a preamble, a missing reference entry,
or a domain that the skill does not currently cover well, please open an issue or submit a
pull request. When submitting changes to `SKILL.md`, ensure that:

- The article preamble in Step 2 and the Beamer preamble in Step 2B remain independent
  and conflict-free.
- New macros follow the naming conventions established in Step 3.3.
- Any new reference entries in Step 9 use the `\bibitem` format specified in Step 5.
- Beamer-specific additions maintain the footnote citation rules in Step 3B.2 — no
  end-of-document bibliography sections in Beamer output.
- The pre-output checklists in Step 7 (article) and Step 10C (Beamer) remain complete
  and internally consistent.
- The `SKILL.md` description field does not exceed 1024 characters (validation limit).
