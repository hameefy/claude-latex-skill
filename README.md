![LaTeX Skill for Claude](banner.svg)

# Claude LaTeX Skill

> **Produce high-quality, compilable LaTeX instantly — using Claude, for free.**
>
> Created by [Hassan Mohammad](https://github.com/hameefy) · Open Source · MIT Licence

---

## What Does This Skill Do?

This skill turns Claude into a **specialist LaTeX assistant** for mathematics, science, and engineering. Instead of generic output, you get:

- ✅ Fully compilable `.tex` files — ready to compile with any LaTeX compiler
- ✅ Complete Beamer presentation slides with proper slide structure
- ✅ Correct mathematical notation (theorems, proofs, algorithms, convergence analysis)
- ✅ Professional preamble with all packages pre-configured
- ✅ Three modes: **Document**, **Snippet**, and **Beamer Presentation**

**You do not need Claude Code. You do not need a terminal. You only need a free Claude account.**

---

## Who Is This For?

- 📐 Mathematics and science students writing reports or theses
- 🎓 Researchers typesetting theorems, proofs, and derivations
- 👨‍🏫 Lecturers preparing Beamer presentation slides
- 💻 Anyone who finds LaTeX preambles and package configuration frustrating
- 🌍 Users worldwide — no subscription, no installation required

---

## How to Use (Free Claude — No Terminal Needed)

This is the simplest method. It works with the **free version** of Claude at [claude.ai](https://claude.ai).

### Step 1 — Download the skill file

Click this link to open `SKILL.md` on GitHub:

👉 **[View SKILL.md](https://github.com/hameefy/claude-latex-skill/blob/main/SKILL.md)**

On that page, click the **Copy raw file** button (the two-overlapping-squares icon near the top right of the file). This copies the entire skill to your clipboard.

> **Alternative:** Click the green **Code** button on the main repo page → **Download ZIP** → this gives you a `.zip` file containing `SKILL.md`.

---

### Step 2 — Open Claude and install the skill

Go to **[claude.ai](https://claude.ai)** and install the skill using the built-in Skills manager:

1. Click the **➕ (plus) icon** in the main chat interface
2. Navigate to **Skills** in the menu that appears
3. Click **Manage Skills**
4. Click the **➕ icon** near the search bar
5. Click **Create Skill**, then click **Upload Skill**
6. **Drag and drop** your downloaded `SKILL.md` file (or `.zip` containing it) into the upload area, or click to browse and select the file
7. Click **Save** — the skill is now installed and active

> **Not sure how to install?** You can also ask Claude directly: type *"How do I install a skill in Claude?"* and Claude will walk you through the steps for your current version.

---

### Step 3 — Start a conversation and make your request

Once the skill is installed, open a new conversation in Claude. The skill will activate automatically when you make a LaTeX-related request. Just describe what you need:

**Example requests you can try:**

| What you want | What to type |
|---|---|
| A theorem with proof | `Write a theorem and proof for the convergence of gradient descent` |
| A Beamer slide deck | `Create a Beamer presentation on conjugate gradient methods` |
| A LaTeX snippet | `Give me a LaTeX table comparing three optimisation algorithms` |
| A full report | `Produce a full LaTeX document for my numerical experiments section` |

> **Without skill installation:** You can also use the skill without installing it. Copy the contents of `SKILL.md`, then paste it into your message wrapped in `<skill>...</skill>` tags, followed by your request. This works in any conversation without any setup.

---

### Step 4 — Compile the output

Copy Claude's `.tex` output and compile it using **any LaTeX compiler available to you**:

**Online (no installation required):**
- [Overleaf](https://overleaf.com) — paste into a blank project and click Recompile
- [Papeeria](https://papeeria.com), [LaTeX.Online](https://latexonline.cc), or any other online LaTeX editor

**On your computer (if you have a LaTeX distribution installed):**
- **TeX Live** (Linux/macOS/Windows) — compile with `pdflatex filename.tex` in a terminal
- **MiKTeX** (Windows) — open in TeXworks or any MiKTeX-compatible editor and compile
- **MacTeX** (macOS) — open in TeXShop or TeXstudio and click Typeset

The output from Claude is immediately compilable — no manual package installation or preamble editing needed.

That's it. No command line required for the online route.

---

## How to Use (Claude Code — For Advanced Users)

If you have Claude Code installed, you can install this skill permanently so it activates automatically whenever you work on LaTeX.

### From a Release (recommended)

1. Go to the [Releases](https://github.com/hameefy/claude-latex-skill/releases) page
2. Download `latex.skill` from the latest release
3. In Claude Code, run:

```bash
claude skill install latex.skill
```

### From Source

```bash
git clone https://github.com/hameefy/claude-latex-skill.git
cd claude-latex-skill
claude skill install .
```

---

## What Can I Ask It?

### Document Mode — Full `.tex` Files

Ask for anything that needs a complete, standalone document:

```
Write a LaTeX document containing Theorem 3.1 and its proof for the global convergence 
of the Fletcher-Reeves conjugate gradient method under Wolfe line search conditions.
```

```
Produce a LaTeX report of my numerical experiments comparing Adam, SGD, and RMSProp 
on a logistic regression problem. Include a performance table and convergence graph placeholder.
```

### Beamer Mode — Presentation Slides

Any of these phrases will trigger slide output:

```
Create Beamer slides for my talk on proximal gradient methods.
```

```
Make a presentation on regularisation techniques for inverse problems.
Include introduction, related work, methodology, convergence results, 
numerical experiments, and conclusion sections.
```

### Snippet Mode — Single Equations or Tables

```
Give me a LaTeX snippet for a 3×4 table of optimisation results with booktabs formatting.
```

```
Write the LaTeX for the proximal operator definition and the ISTA update rule.
```

---

## Three Modes at a Glance

| Mode | Output | When to use |
|---|---|---|
| **Document** | Full `.tex` file | Reports, papers, thesis chapters, technical notes |
| **Snippet** | LaTeX code block | Single equations, tables, algorithms to paste into your own doc |
| **Beamer** | Full `.tex` slide deck | Conference talks, seminar presentations, lecture slides |

---

## Compatibility

| Requirement | Detail |
|---|---|
| Claude version | Any version at claude.ai (free or paid) · Claude Sonnet 4 or later for Claude Code |
| LaTeX engine — article | pdfLaTeX (primary); LuaLaTeX and XeLaTeX supported for BibLaTeX/Biber |
| LaTeX engine — Beamer | pdfLaTeX (recommended); LuaLaTeX supported |
| TeX distribution | TeX Live 2022 or later; MiKTeX 22 or later · Or use Overleaf (no installation) |
| pgfplots | compat = 1.18 or later |
| Beamer | Any version included in TeX Live 2022 or later |
| `perpage` package | Included in TeX Live and MiKTeX standard installations |

---

## Feedback and Contributions

This skill is actively maintained. If something doesn't work, or you want a new feature:

- 🐛 **Found a bug?** [Open an issue](https://github.com/hameefy/claude-latex-skill/issues)
- 💡 **Have a suggestion?** Open a discussion or submit a pull request
- ⭐ **If it helped you**, please star the repository — it helps others find it

### What feedback is most useful?

- LaTeX that didn't compile — paste the error message in the issue
- A mathematical structure the skill didn't handle correctly
- Missing packages or notations for your field
- Any mode (Document / Snippet / Beamer) producing wrong output

---

## Quick-Start Summary

```
1. Download SKILL.md (or ZIP) from GitHub
2. Open claude.ai → click ➕ → Skills → Manage Skills → ➕ → Create Skill → Upload Skill
3. Drag and drop SKILL.md (or ZIP) → Save
4. Start a new conversation and describe your LaTeX task — the skill activates automatically
5. Copy Claude's .tex output → compile with any LaTeX compiler (online or on your computer)
```

> **Shortcut (no install):** Paste the contents of SKILL.md inside `<skill>...</skill>` tags in any message, followed by your request. Works immediately without Step 2–3.

**No terminal required. Works on the free plan.**

---

*Built by [Hassan Mohammad](https://github.com/hameefy) · Applied Mathematics & Numerical Optimisation*
*Bayero University | Kyungpook National University · Open Source · MIT Licence*

