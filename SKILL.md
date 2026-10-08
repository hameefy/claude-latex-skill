---
name: latex
description: >
  Produces compilable LaTeX for researchers in numerical optimisation and applied
  mathematics. Use for theorems, proofs, convergence analysis, algorithms, tables, TikZ
  figures, derivations, literature reviews, or structured academic documents, and for
  requests such as "write up", "typeset", "format in LaTeX", "produce a .tex file", or
  "give me the LaTeX for". Covers numerical optimisation, nonlinear equations, deep
  learning theory, and numerical analysis; inverse problems, EIT, and PINNs are
  supported through an optional reference file. Three modes: (1) DOCUMENT MODE, a full
  standalone article; (2) SNIPPET MODE, body-only fragments; (3) BEAMER MODE, complete
  Beamer slides, used for "slides", "presentation", "beamer", "talk", "seminar", "slide
  deck". Also use when the user supplies a website URL and wants LaTeX output, e.g. "turn
  this link into LaTeX", "make slides from this page", or "convert this article to a
  report"; the page is fetched and routed to the correct mode.
---

# LaTeX Skill

This skill governs the production of LaTeX output for researchers in numerical optimisation
and applied mathematics: unconstrained and constrained optimisation, nonlinear equations,
conjugate gradient and quasi-Newton methods, stochastic optimisation, deep learning theory,
and numerical analysis. All output must satisfy the mathematical standards of journals such
as SIAM Journal on Optimization, Mathematical Programming, Optimization Methods and
Software, and Mathematics of Computation.

Inverse problems, electrical impedance tomography (EIT), regularisation theory, and PINNs
are covered by the optional file `references/inverse-problems.md` (notation, macros, and
reference entries). Read it only when the request concerns those areas. If the file is not
installed, proceed with the rules in this file.

---

## Step 0. Detect and Ingest Web Content (URL Input)

**Run this step FIRST, before Step 1, whenever the user provides a URL or web link.**

### 0.1 Detect a URL

Treat the request as a URL-sourced request if the user's message contains any of the
following patterns:

- A URL beginning with `http://` or `https://`.
- A bare domain reference such as `arxiv.org/abs/...`, `doi.org/...`, or a journal/blog
  URL that can be resolved.
- Phrases such as: "from this link", "from this page", "from this article", "from this
  website", "from this paper online", "convert this URL", or "turn this link into LaTeX".

If **no URL** is detected, skip Step 0 entirely and proceed directly to Step 1.

### 0.2 Fetch the Page

Use whichever web-fetch tool is available in the current environment (its name differs
between Claude surfaces) to retrieve the page. Pass the URL exactly as provided by the
user, and request readable text or Markdown output where the tool offers that choice. If
no fetch tool is available, say so and ask the user to paste the content.

**If the fetch fails** (network error, access denied, paywall, login wall):
- Inform the user clearly: "I was unable to retrieve the content from `<URL>`. The page
  may require login, be behind a paywall, or be unavailable."
- Ask whether the user can paste the content directly into the chat.
- Do NOT attempt to generate LaTeX from guessed or fabricated content.

**If the fetch partially succeeds** (truncated content, missing sections):
- Proceed with what was retrieved.
- Add a `\todo{}` placeholder wherever content appears incomplete or cut off.
- Mention to the user that the fetched content may be partial.

### 0.3 Analyse and Clean the Fetched Content

Once the page content is retrieved, perform the following analysis before proceeding to
Step 1:

1. **Identify the content type.** Is the page:
   - A research paper or preprint (e.g. arXiv, journal article)?
   - A blog post or technical article?
   - A documentation page (e.g. library or software docs)?
   - A lecture notes or course page?
   - A general web article or news item?

2. **Extract the core structure.** Identify:
   - Title, authors, date/venue (if a paper or article).
   - Abstract or executive summary (if present).
   - Section headings and their content.
   - Mathematical expressions, algorithms, tables, and figures (note their presence;
     do NOT fabricate numerical values or theorems not in the source).
   - References or citations listed on the page.

3. **Filter noise.** Discard: navigation menus, cookie banners, advertisement text,
   footer boilerplate, comment sections, and any content clearly unrelated to the
   main article.

4. **Preserve mathematics.** If the page contains mathematical notation (even in
   informal or non-LaTeX form), transcribe it into proper LaTeX using the macros
   defined in Step 2 / Step 2B. Do not simplify, omit, or alter mathematical content.

5. **Note source metadata.** Record the following for use in the LaTeX document:
   - Source URL (for `\url{}` or `\href{}` citation).
   - Author name(s) if available.
   - Publication date if available.
   - Page or document title.

   These will be used to populate the LaTeX `\title{}`, `\author{}`, `\date{}` fields
   (Document Mode), the Beamer metadata (Beamer Mode), or an inline attribution comment
   (Snippet Mode).

### 0.4 Clarify the Output Mode (if ambiguous)

After fetching and analysing the content, if the user has not explicitly stated which
output mode they want, ask ONE clarifying question:

> "I've retrieved the content from `<URL>`. Should I produce:
> (a) a complete standalone LaTeX document (article),
> (b) a Beamer slide presentation, or
> (c) a LaTeX snippet (equations, tables, or algorithm only)?"

If the user's original phrasing already signals the mode (e.g., "make slides from this
link" → Beamer Mode; "typeset this article" → Document Mode; "give me the LaTeX table
from this page" → Snippet Mode), proceed without asking.

### 0.5 Hand Off to the Main Pipeline

Once content is fetched, cleaned, and the output mode is known, treat the extracted
content exactly as if the user had pasted it directly. Proceed to **Step 1** (mode
classification) and then through the full normal pipeline (Steps 2–10 as appropriate).

**Additional rules for URL-sourced content:**

- **Attribution:** In Document Mode, include a comment near the top of the `.tex` file:
  ```latex
  % Source: <URL>
  % Retrieved: <date of fetch>
  ```
  In Beamer Mode, add a `{\tiny\url{<URL>}}` citation on the title or introduction slide.
  In Snippet Mode, add a `% Source: <URL>` comment above the snippet.

- **No fabrication:** Do not invent theorems, lemmas, numerical results, or citations
  that do not appear in the fetched content. Use `\todo{}` for any section where the
  source content is absent or unclear.

- **Copyright note:** Web content is copyrighted. The LaTeX output is a
  **scholarly reformatting** for academic use. Do not reproduce verbatim large passages
  of prose from the source; paraphrase into formal academic register where necessary,
  and preserve mathematics exactly.

- **arXiv and DOI links:** For arXiv links (`https://arxiv.org/abs/NNNN.NNNNN`), also
  attempt to fetch the abstract page. The PDF itself cannot be fetched directly; if the
  user wants full paper content, they should supply the PDF as an upload. For DOI links,
  fetch the resolved landing page.

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
- If the user supplies a URL (processed via Step 0): use the fetched and cleaned content
  as the source manuscript. Populate slides from that content; insert `\todo{}` wherever
  the fetched page did not provide sufficient detail for a slide.
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
- **Content fetched from a URL (Step 0)** where the user requested a full document or
  report format.

Save the output as a `.tex` file and present it to the user for download.

### Snippet Mode

Produce raw LaTeX body content only (no `\documentclass`, no preamble, no
`\begin{document}`) when the request involves:

- A single equation or aligned system of equations.
- An algorithm or pseudocode block in isolation.
- A numerical results table.
- A TikZ figure or pgfplots graph.
- Any fragment intended to be inserted into the user's own template.
- **Content fetched from a URL (Step 0)** where the user requests only specific elements
  (e.g. "give me the LaTeX for the table on this page", "typeset just the algorithm from
  this link").

Deliver snippet output as a labelled code block in the chat, not as a file, unless the
user explicitly requests a file.

**When in doubt**, ask: "Should I produce a complete standalone document, a snippet, or a
Beamer presentation?"

---

## Step 2. Apply the Standard Preamble (Document Mode Only)

Use the preamble below verbatim: do not improvise, abbreviate, or reorder the package
list (`hyperref` must precede `cleveref`). Macros that a document does not use may stay
in the preamble; they are harmless. Add any further macros after the existing block.

```latex
\documentclass[11pt,a4paper]{article}

% ---------------------------------------------------------------
% Core mathematics packages
% ---------------------------------------------------------------
\usepackage{amsmath, amssymb, amsthm, mathtools}
\usepackage{bm}            % bold math symbols

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
\usepackage{subcaption}    % subfigure environment for side-by-side figures
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

**Author and institute:** replace the placeholders in the metadata block with the user's
details when they are known; otherwise leave the placeholders in place.

**Color theme:** Default is `default` (matching the chosen outer theme's palette). If the
user requests a specific color theme (e.g., `dolphin`, `beaver`, `crane`, `orchid`,
`rose`, `seagull`, `seahorse`, `whale`, `wolverine`), apply it with `\usecolortheme{}`.

```latex
\documentclass[aspectratio=169,10pt]{beamer}
% aspectratio=169 gives 16:9 widescreen; use 43 for 4:3 if user requests it.

% ---------------------------------------------------------------
% Theme: change \usetheme{} to any standard Beamer theme name
% ---------------------------------------------------------------
\usetheme{Madrid}
% \usecolortheme{dolphin}   % Uncomment and change to apply a colour theme

% ---------------------------------------------------------------
% Core mathematics
% ---------------------------------------------------------------
\usepackage{amsmath, amssymb, amsthm, mathtools}
\usepackage{bm}

% ---------------------------------------------------------------
% Algorithms: use ONE of the two options below; comment out the other
% ---------------------------------------------------------------
% OPTION A: algorithm2e (recommended for pseudocode with line numbers)
\usepackage[ruled,vlined,linesnumbered]{algorithm2e}

% OPTION B: algpseudocode (it loads algorithmicx itself). Never load it together
% with algorithmic or algorithm2e: these packages define the same commands.
% \usepackage{algpseudocode}

% ---------------------------------------------------------------
% Graphics and plots
% ---------------------------------------------------------------
\usepackage{graphicx}
\usepackage{tikz}
\usepackage{pgfplots}
\pgfplotsset{compat=1.18}

% ---------------------------------------------------------------
% Tables
% ---------------------------------------------------------------
\usepackage{booktabs}
\usepackage{tabularx}

% ---------------------------------------------------------------
% Theorem-like blocks: configurable style
% ---------------------------------------------------------------
% Beamer provides: theorem, lemma, corollary, proof, definition,
% example, block, alertblock, exampleblock; all are built in.
%
% For coloured framed boxes (tcolorbox style), uncomment below:
% \usepackage{tcolorbox}
% \tcbuselibrary{theorems,skins}
% Then define custom tcolorbox environments as needed (see Step 3B).

% ---------------------------------------------------------------
% Footnote citation control
% ---------------------------------------------------------------
\usepackage{perpage}   % restarts footnote numbers on each slide (needs two runs)
\MakePerPage{footnote}

% ---------------------------------------------------------------
% Miscellaneous
% ---------------------------------------------------------------
\usepackage{xcolor}
\usepackage{multicol}  % for two-column slides

% ---------------------------------------------------------------
% Placeholder marker: \todo{...} prints a red [TODO: ...] in text or math.
% (todonotes is not loaded: its margin notes do not work in beamer.)
% ---------------------------------------------------------------
\DeclareRobustCommand{\todo}[1]{%
  \textcolor{red}{\ifmmode\text{[TODO: #1]}\else[TODO: #1]\fi}}

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
% Presentation metadata: fill in before \begin{document}
% ---------------------------------------------------------------
\title[Short Title]{Full Title of the Presentation}
\subtitle{Subtitle or Paper Title (if applicable)}
\author[A.~Surname]{Author Name}
\institute[Short Inst.]{%
  Research Group or Department\\
  Faculty or School\\
  University, City, Country
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

Beamer Mode cites sources in footnotes. There is NO reference section at the end of the
presentation; every cited source appears as a footnote on the slide where it is cited.

**Default pattern: `\footnote[frame]{...}` at the point of citation.**

```latex
\begin{frame}{Related Work}
  \begin{itemize}
    \item Spectral residual methods\footnote[frame]{W.~La Cruz, J.~M.~Mart\'{\i}nez, and
      M.~Raydan, ``Spectral residual method without gradient information for solving
      large-scale nonlinear systems of equations,'' \textit{Math.\ Comp.}, vol.~75,
      no.~255, pp.~1429--1448, 2006.} require no derivative information.
    \item Performance profiles\footnote[frame]{E.~D.~Dolan and J.~J.~Mor\'{e},
      ``Benchmarking optimization software with performance profiles,''
      \textit{Math.\ Program.}, vol.~91, no.~2, pp.~201--213, 2002.} are the standard
      tool for comparing solvers.
  \end{itemize}
\end{frame}
```

Why this pattern is the default:

- The mark and the text come from one command, so their numbers cannot disagree.
- The `[frame]` option places the text at the bottom of the slide even when the citation
  sits inside `columns`, a `block`, or a theorem environment. Without `[frame]`, the
  footnote is set inside that box.
- `\MakePerPage{footnote}` (already in the preamble) restarts the numbering at 1 on every
  slide. The restart is resolved through the `.aux` file, so the deck must be compiled
  twice.

**Fallback pattern: `\footnotemark` with `\footnotetext[N]{...}`.** Use it only where
`\footnote` cannot be placed, for instance inside `\caption{}` or a `tabular` cell: put
`\footnotemark{}` there and the `\footnotetext` after the environment, inside the same
frame.

A bare `\footnotetext{...}` prints the value that the footnote counter holds at that
moment. This is standard LaTeX behaviour, not an effect of `perpage`. After three
`\footnotemark{}` calls the counter equals 3, so three bare `\footnotetext{}` commands
would all be labelled 3. The file compiles without error and the labels are wrong.
Therefore, **whenever a slide has two or more `\footnotemark{}` calls, every
`\footnotetext` must carry the explicit number `[N]` of its mark:**

```latex
\begin{frame}{Comparison}
  \begin{tabular}{ll}
    \toprule
    Method A\footnotemark{} & description of A \\
    Method B\footnotemark{} & description of B \\
    \bottomrule
  \end{tabular}
  \footnotetext[1]{Author A, ``Title A,'' \textit{Journal}, vol., pp., Year.}
  \footnotetext[2]{Author B, ``Title B,'' \textit{Journal}, vol., pp., Year.}
\end{frame}
```

With a single mark on the slide, a bare `\footnotetext{...}` is correct.

Rules for footnote citations:
1. Each citation footnote appears on the frame where the source is cited.
2. In the fallback pattern, every `\footnotemark` has exactly one `\footnotetext` in the
   same `frame`, and no `\footnotetext` appears without its mark.
3. In the fallback pattern with two or more marks on a slide, use `\footnotetext[N]{...}`
   with the explicit `N` of each mark. Never use bare `\footnotetext{}` there.
4. Format: Author(s), ``Title,'' \textit{Journal/Proceedings}, vol., no., pp., Year.
   For books: Author(s), \textit{Title}, Publisher, Year.
5. If more than three references appear on one slide, reduce the size *inside* the
   footnote text: `\footnote[frame]{\tiny ...}` or `\footnotetext[N]{\tiny ...}`.
   Wrapping the command itself, as in `{\tiny \footnotetext[N]{...}}`, has no effect.
6. If the user provides BibTeX keys without full details, supply the bibliographic entry
   only when every field is known with certainty (a well-known book or paper). Never
   guess a volume, page range, or year. Otherwise insert:
   `\footnote[frame]{\todo{Fill in full bibliographic details.}}`

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

Use `algorithm2e` (already loaded) with the `[H]` placement specifier; floating
algorithms are not available inside frames. The `[fragile]` frame option is not needed for
`algorithm2e` or `algpseudocode`; reserve it for frames that contain `verbatim` or
`listings` content.

```latex
\begin{frame}{Algorithm: Name of Algorithm}
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

If the user prefers `algpseudocode` (Option B in the preamble):

```latex
\begin{frame}{Algorithm: Name}
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
side. `\includegraphics` requires the figure files to exist; when the user has not
supplied them, use the boxed placeholder of Step 10B instead of a file name.

```latex
\begin{frame}{Numerical Results: Performance Profiles}
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
\begin{frame}{Numerical Results: Comparison Table}
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
   subtitle distinguishing them (e.g., "Proof: Part I", "Proof: Part II").
3. Use `\pause` sparingly; prefer complete slides for printed handouts.
4. Use `\alert{}` to highlight a single key term or result per frame, not multiple items.
5. Use `\begin{itemize}` / `\begin{enumerate}` with no more than five items per frame.
   Sub-items are allowed but limit nesting to two levels.
6. Equations on slides must be display-style whenever they span more than a short inline
   fragment. Prefer `\[ ... \]` over inline `$ ... $` for anything non-trivial.
7. Every frame that cites a source carries the citation as a footnote on that frame
   (see Section 3B.2).

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
  By $L$-smoothness (\cref{ass:smoothness}), the descent lemma applied to
  $x_{k+1} = x_k - \alpha \grad f(x_k)$ with $\alpha \leq 1/L$ gives
  \[
    f(x_{k+1})
    \leq f(x_k) - \alpha\Bigl(1 - \frac{L\alpha}{2}\Bigr)\norm{\grad f(x_k)}^2
    \leq f(x_k) - \frac{\alpha}{2}\norm{\grad f(x_k)}^2.
  \]
  Summing from $k = 0$ to $K-1$ and using $f(x_K) \geq f^*$ yields
  $\frac{\alpha}{2}\sum_{k=0}^{K-1}\norm{\grad f(x_k)}^2 \leq f(x_0) - f^*$.
  Since the minimum of the summands does not exceed their average,
  \cref{eq:gd-rate} follows.
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
| Vectors | $x, d, g \in \R^n$ (plain lowercase italic, as in the optimisation literature) |
| Matrices / linear operators | $A, B_k, H_k \in \R^{n \times n}$ (plain uppercase italic) |
| Bold variants | $\bx$, $\bA$ only when the user's source uses bold; then set every vector and matrix in bold |
| Function spaces | $\Lp{2}(\Omega)$, $\Sob{1}{\Omega}$, $\SobZ{1}{\Omega}$ |
| Gradient | $\grad f(x)$ |
| Hessian | $\Hess f(x)$ |
| Iterates | $x_k$ (subscript) or $x^{(k)}$ (superscript); pick one and do not change it |
| Objective / loss | $f$, $\mathcal{L}$, or $F$ following the user's choice |
| Step size | $\alphak$ or $\etak$; do not mix |
| Regularisation parameter | $\lambda$ or $\mu$ (not $\alpha$ if that is the step size) |
| Frobenius norm | $\normF{A}$ |
| Expectation | $\E[\cdot]$ |
| Probability | $\Prob(\cdot)$ |

For EIT and inverse problems, the notation conventions and the macros `\Forward`,
`\Reg`, and `\Tikhonov` are given in `references/inverse-problems.md`.

### 3.4 Algorithm Pseudocode

Use the `algorithm2e` package (loaded with `ruled, vlined, linesnumbered`). Number lines
whenever the proof or analysis refers to specific steps. Add inline comments with `\tcp{}`.

```latex
\begin{algorithm}[H]
\caption{Descent method with backtracking line search}\label{alg:descent}
\KwIn{Initial point $x_0 \in \R^n$; parameters $\sigma, \rho \in (0,1)$;
      tolerance $\varepsilon > 0$}
\KwOut{Approximate stationary point $x_k$}
Set $k \leftarrow 0$\;
\While{$\norm{\grad f(x_k)} > \varepsilon$}{
  Compute a descent direction $d_k$, that is, $\grad f(x_k)^\top d_k < 0$\;
  \tcp{Armijo backtracking}
  Set $\alphak \leftarrow 1$\;
  \While{$f(x_k + \alphak d_k) > f(x_k) + \sigma\alphak \grad f(x_k)^\top d_k$}{
    $\alphak \leftarrow \rho\,\alphak$\;
  }
  $x_{k+1} \leftarrow x_k + \alphak d_k$\;
  $k \leftarrow k + 1$\;
}
\Return{$x_k$}\;
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

Prefer `tabularx` over `\resizebox` wherever possible: rescaling reduces font size relative
to surrounding text.

See Section 3.5 examples below:

```latex
% Narrow table
\begin{table}[ht]
\centering
\caption{Final gradient norm (mean $\pm$ standard deviation over ten starting points).
  Bold indicates the lowest value.}\label{tab:gradnorm}
\begin{tabular}{lccc}
\toprule
Method & $n = 10^3$ & $n = 10^4$ & $n = 10^5$ \\
\midrule
Method A & $0.142 \pm 0.008$ & $0.231 \pm 0.011$ & $0.318 \pm 0.014$ \\
Method B & $\mathbf{0.103 \pm 0.006}$ & $\mathbf{0.187 \pm 0.009}$ & $\mathbf{0.274 \pm 0.013}$ \\
\bottomrule
\end{tabular}
\end{table}

% Wide table
\begin{table}[ht]
\centering
\caption{Number of iterations across five problem dimensions.}\label{tab:perf}
\begin{tabularx}{\linewidth}{l *{5}{>{\centering\arraybackslash}X}}
\toprule
Method & $n_1$ & $n_2$ & $n_3$ & $n_4$ & $n_5$ \\
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

Side-by-side figures use the `subfigure` *environment* of the `subcaption` package (loaded
in the Step 2 preamble). Never load the obsolete `subfigure` package or `subfig`: their
`\subfigure{}` and `\subfloat{}` commands are incompatible with `subcaption`.

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

% Two side-by-side subfigures (subcaption package)
\begin{figure}[ht]
\centering
\begin{subfigure}{0.48\textwidth}
  \centering
  \includegraphics[width=\linewidth]{fig_iterations.pdf}
  \caption{Number of iterations.}\label{fig:profile-iter}
\end{subfigure}\hfill
\begin{subfigure}{0.48\textwidth}
  \centering
  \includegraphics[width=\linewidth]{fig_fevals.pdf}
  \caption{Function evaluations.}\label{fig:profile-fevals}
\end{subfigure}
\caption{Performance profiles of the compared methods.}\label{fig:profiles}
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
Accelerated first-order methods originate with \citet{nesterov1983}. Line-search and
trust-region globalisation strategies are treated in detail by \citet{nocedal2006}, and
the convex theory by \citet{boyd2004}. Adaptive stochastic methods are now standard in
deep learning~\citep{kingmaba2015, goodfellow2016}.
```

---

## Step 5. Reference Section (Document Mode)

### 5.0 Choose a Bibliography Workflow

Default to Option A without asking. Switch to Option B only when the user mentions a
`.bib` file, BibTeX, BibLaTeX, Biber, or a journal bibliography style. The options are
architecturally incompatible: do not mix commands or packages from both in the same
document.

| | Option A — Self-contained | Option B — External `.bib` |
|---|---|---|
| **Backend** | Inline `thebibliography`; no auxiliary files | BibTeX (`natbib`) or BibLaTeX (Biber) |
| **When to use** | Quick notes, isolated proofs, single-file submissions | Ongoing projects with a shared `.bib`; BibLaTeX users |

---

### Option A — Self-Contained (`thebibliography`)

Place immediately before `\end{document}`. Set the argument to the widest label expected
(e.g., `{99}`). Do not include `\bibliographystyle{}` or `\bibliography{}`.

Every `\bibitem` carries the optional `natbib` label `[Authors(Year)]`. Without this
label `\citet{}` prints "(author?)" in place of the author names. Write `Surname(Year)`
for one author, `Surname and Surname(Year)` for two, and `Surname et~al.(Year)` for three
or more, with no space before the opening parenthesis.

#### `\bibitem` Format by Source Type

```latex
% Journal article
\bibitem[Surname and Surname(Year)]{citekey}
A.~Surname and B.~Surname,
``Title,''
\textit{Journal Name},
vol.~X, no.~Y, pp.~NNN--NNN, Year.

% Conference paper
\bibitem[Surname and Surname(Year)]{citekey}
A.~Surname and B.~Surname,
``Title,''
in \textit{Proceedings of the Conference (ACRONYM)},
City, Country, Year, pp.~NNN--NNN.

% Book
\bibitem[Surname(Year)]{citekey}
A.~Surname,
\textit{Title of the Book},
Publisher, City, Year.

% PhD / MSc thesis
\bibitem[Surname(Year)]{citekey}
A.~Surname,
``Title of the thesis,''
Ph.D.\ dissertation, Department, University, City, Country, Year.

% Technical report
\bibitem[Surname(Year)]{citekey}
A.~Surname,
``Title,''
Tech.\ Rep.\ TR-XXXX, Institution, Year.

% arXiv preprint
\bibitem[Surname and Surname(Year)]{citekey}
A.~Surname and B.~Surname,
``Title,''
\textit{arXiv preprint} arXiv:XXXX.XXXXX, Year.
```

If the user cites a key without providing bibliographic details, supply the entry only
when every field is known with certainty (a well-known book or paper). Never guess a
volume, page range, or year, and never invent an entry. When a literature-search or
citation tool is available, verify the entry with it. Otherwise insert:

```latex
\bibitem[TODO(0000)]{citekey}
\todo{Fill in full bibliographic details for \texttt{citekey}.}
```

Do not omit the `\bibitem`; a missing entry causes a compilation error.

---

### Option B1 — BibTeX with `natbib`

Keep the `natbib` line of the Step 2 preamble unchanged.

End-of-document block:

```latex
\bibliographystyle{plainnat}   % or: abbrvnat, unsrtnat
\bibliography{refs}
```

Compilation: `pdflatex` → `bibtex` → `pdflatex` × 2.

The `natbib`-aware styles `plainnat`, `abbrvnat`, and `unsrtnat` support `\citet{}` and
`\citep{}`. The classical styles `plain`, `unsrt`, `ieeetr`, `siam`, and `amsplain` carry
no author data: with them use `\cite{}` only, because `\citet{}` prints "(author?)".

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

If the user cites a key without a `.bib` entry, apply the rule of Option A: supply the
entry only when every field is known with certainty. Otherwise insert:

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
part of the cited work is being used (e.g., `\citet[Theorem~3.2]{nocedal2006}`). Avoid vague
attributions such as "as shown in [3]".

Place quantitative conditions in numbered `assumption` environments rather than burying them
inside theorem statements, whenever those conditions are reusable across multiple results.

### 6.4 Consistency Checks

Before outputting any document, verify:
1. Every macro used in the body is defined in the preamble. Unused macros of the standard preamble may remain.
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
- No package conflicts. `subcaption` is loaded; the obsolete `subfigure` package is not.

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
- **Option A:** Every `\cite{}` key has a fully populated `\bibitem{}` with its `[Authors(Year)]` label; no orphan entries.
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

The following `\bibitem` entries cover foundational works in numerical optimisation, deep
learning, and numerical analysis. Insert directly when the corresponding key is cited.
Entries for EIT, regularisation theory, inverse problems, and PINNs are in
`references/inverse-problems.md`.

```latex
% --- Numerical optimisation ---

\bibitem[Nesterov(1983)]{nesterov1983}
Y.~Nesterov,
``A method for solving the convex programming problem with convergence rate
$\mathcal{O}(1/k^2)$,''
\textit{Doklady Akademii Nauk SSSR},
vol.~269, no.~3, pp.~543--547, 1983.

\bibitem[Nocedal and Wright(2006)]{nocedal2006}
J.~Nocedal and S.~J.~Wright,
\textit{Numerical Optimization},
2nd~ed.,
Springer, New York, NY, 2006.

\bibitem[Boyd and Vandenberghe(2004)]{boyd2004}
S.~Boyd and L.~Vandenberghe,
\textit{Convex Optimization},
Cambridge University Press, Cambridge, 2004.

% --- Deep learning ---

\bibitem[Kingma and Ba(2015)]{kingmaba2015}
D.~P.~Kingma and J.~Ba,
``Adam: A method for stochastic optimization,''
in \textit{Proceedings of the 3rd International Conference on Learning
Representations (ICLR)},
San Diego, CA, USA, 2015.

\bibitem[Goodfellow et~al.(2016)]{goodfellow2016}
I.~Goodfellow, Y.~Bengio, and A.~Courville,
\textit{Deep Learning},
MIT Press, Cambridge, MA, 2016.

% --- Numerical analysis ---

\bibitem[Golub and Van~Loan(2013)]{golub2013}
G.~H.~Golub and C.~F.~Van~Loan,
\textit{Matrix Computations},
4th~ed.,
Johns Hopkins University Press, Baltimore, MD, 2013.

\bibitem[Trefethen and Bau(1997)]{trefethen1997}
L.~N.~Trefethen and D.~Bau,
\textit{Numerical Linear Algebra},
SIAM, Philadelphia, PA, 1997.

\bibitem[Brenner and Scott(2008)]{brenner2008}
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
table). Compile with `pdflatex` twice: cross-references and the per-slide footnote numbers are
resolved through the `.aux` file.

```latex
\documentclass[aspectratio=169,10pt]{beamer}

% [Insert the full preamble from Step 2B here, from \usetheme through \date.]

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
% FRAME 1: Title Page
% =============================================================
\begin{frame}
  \titlepage
\end{frame}

% =============================================================
% FRAME 2: Table of Contents
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
    \item \todo{Author(s) (Year): brief description of contribution.}%
      \footnote[frame]{\todo{Full citation 1.}}
    \item \todo{Author(s) (Year): brief description.}%
      \footnote[frame]{\todo{Full citation 2.}}
    \item \todo{Author(s) (Year): brief description.}%
      \footnote[frame]{\todo{Full citation 3.}}
  \end{itemize}
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

\begin{frame}{Algorithm}
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
        % Replace the box with \includegraphics[width=\linewidth]{fig_iterations.pdf}
        \fbox{\parbox[c][0.45\linewidth][c]{0.9\linewidth}{\centering
          \todo{Insert fig\_iterations.pdf}}}
        \caption{Number of iterations}
      \end{figure}
    \end{column}
    \begin{column}{0.48\textwidth}
      \begin{figure}
        % Replace the box with \includegraphics[width=\linewidth]{fig_fevals.pdf}
        \fbox{\parbox[c][0.45\linewidth][c]{0.9\linewidth}{\centering
          \todo{Insert fig\_fevals.pdf}}}
        \caption{Function evaluations}
      \end{figure}
    \end{column}
  \end{columns}
  {\small Dolan--Mor\'{e} performance profiles;\footnote[frame]{E.~D.~Dolan and
    J.~J.~Mor\'{e}, ``Benchmarking optimization software with performance profiles,''
    \textit{Math.\ Program.}, vol.~91, no.~2, pp.~201--213, 2002.}
    a higher curve is better.}
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
- `\todo` is defined in the preamble as in Step 2B.
- No argument of `\includegraphics` is a placeholder; a missing figure uses the boxed
  `\todo{}` placeholder of Step 10B.
- Every frame with `verbatim` or `listings` content uses `[fragile]`.
- No `geometry`, `cleveref`, `natbib`, `todonotes`, or `subfigure` packages are loaded
  (none is used in Beamer Mode).

### Frame Structure
- Every `\begin{frame}` has a matching `\end{frame}`.
- Every frame has a non-empty title (`\begin{frame}{Title}` or `\frametitle{}`).
- No frame contains more than five itemize entries at the top level.
- No frame body exceeds approximately 10 lines of display content (split if necessary).

### Footnote Citation Consistency
- Citations use `\footnote[frame]{...}` at the point of citation (Section 3B.2).
- Where the `\footnotemark` fallback is used, every mark has exactly one `\footnotetext`
  within the SAME frame, and no `\footnotetext` appears without its mark.
- **MULTI-MARK CHECK (mandatory for the fallback):** in every frame that contains **two
  or more** `\footnotemark{}` calls, each `\footnotetext` uses the **explicit `[N]`
  form**: `\footnotetext[1]{...}`, `\footnotetext[2]{...}`, and so on. A bare
  `\footnotetext{...}` prints the current counter value, so all texts would carry the
  last number. This is a **silent rendering bug**: the file compiles without errors.
- Any size reduction is written inside the footnote text (`\footnote[frame]{\tiny ...}`).
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

Both files are optional; this file is complete without them.

- `references/inverse-problems.md`: notation conventions, macros (`\Forward`, `\Reg`,
  `\Tikhonov`), and `\bibitem` entries for EIT, regularisation theory, inverse problems,
  and PINNs. Read it when the request concerns those areas.
- `references/preamble.md`: supplementary optimisation macros (`\argmin`, `\prox`,
  `\dom`, `\xstar`, and others) that can be appended to the Step 2 preamble.
