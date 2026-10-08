# Inverse Problems, EIT, Regularisation, and PINNs

Optional reference for the LaTeX skill. Read this file only when the request concerns
electrical impedance tomography (EIT), regularisation theory, inverse problems, or
physics-informed neural networks (PINNs). The rules of `SKILL.md` continue to apply; this
file adds notation, macros, and reference entries for these areas.

## Contents

1. Macros
2. Notation conventions
3. Reference entries (`\bibitem`)

## 1. Macros

Append to the notation macros of the Step 2 preamble of `SKILL.md` (the block relies on
`\norm` from that preamble).

```latex
% Regularisation (EIT / inverse problems)
\newcommand{\Reg}{\mathcal{R}}
\newcommand{\Forward}{\mathcal{F}}
\newcommand{\Tikhonov}[3]{\norm{#1 - #2}^2 + #3\,\Reg(#2)}
```

`\Tikhonov{data}{model}{parameter}` expands to
$\lVert\text{data} - \text{model}\rVert^2 + \text{parameter}\,\mathcal{R}(\text{model})$.

## 2. Notation conventions

Adopt the following unless the user specifies otherwise.

| Object | Notation |
|---|---|
| Forward (observation) operator | $\Forward$ |
| Regulariser | $\Reg$ |
| Regularisation parameter | $\lambda$ or $\mu$ (not $\alpha$ if that is the step size) |
| Conductivity distribution | $\sigma \in L^\infty(\Omega)$ with $\sigma \geq \sigma_{\min} > 0$ |
| Dirichlet-to-Neumann map | $\Lambda_\sigma \colon H^{1/2}(\partial\Omega) \to H^{-1/2}(\partial\Omega)$ |
| Measurement data | $V \in \R^{m \times n_e}$ |
| Tikhonov functional | `\Tikhonov{\Forward(\sigma)}{V}{\lambda}` |

## 3. Reference entries

Insert directly when the corresponding key is cited. Each entry carries the `natbib`
label `[Authors(Year)]` required by `\citet{}` (see Step 5 of `SKILL.md`).

```latex
% --- Inverse problems and EIT ---

\bibitem[Calder\'{o}n(1980)]{calderon1980}
A.~P.~Calder\'{o}n,
``On an inverse boundary value problem,''
in \textit{Seminar on Numerical Analysis and its Applications to Continuum Physics},
Rio de Janeiro, Brazil, 1980, pp.~65--73.

\bibitem[Cheney et~al.(1990)]{cheney1990}
M.~Cheney, D.~Isaacson, J.~C.~Newell, S.~Simske, and J.~Goble,
``NOSER: An algorithm for solving the inverse conductivity problem,''
\textit{International Journal of Imaging Systems and Technology},
vol.~2, no.~2, pp.~66--75, 1990.

\bibitem[Engl et~al.(1996)]{engl1996}
H.~W.~Engl, M.~Hanke, and A.~Neubauer,
\textit{Regularization of Inverse Problems},
Kluwer Academic Publishers, Dordrecht, 1996.

\bibitem[Vogel(2002)]{vogel2002}
C.~R.~Vogel,
\textit{Computational Methods for Inverse Problems},
SIAM, Philadelphia, PA, 2002.

\bibitem[Kaltenbacher et~al.(2008)]{kaltenbacher2008}
B.~Kaltenbacher, A.~Neubauer, and O.~Scherzer,
\textit{Iterative Regularization Methods for Nonlinear Ill-Posed Problems},
de Gruyter, Berlin, 2008.

\bibitem[Borcea(2002)]{borcea2002}
L.~Borcea,
``Electrical impedance tomography,''
\textit{Inverse Problems},
vol.~18, no.~6, pp.~R99--R136, 2002.

% --- Regularisation ---

\bibitem[Tikhonov(1943)]{tikhonov1943}
A.~N.~Tikhonov,
``On the stability of inverse problems,''
\textit{Doklady Akademii Nauk SSSR},
vol.~39, no.~5, pp.~195--198, 1943.

\bibitem[Rudin et~al.(1992)]{rudin1992}
L.~I.~Rudin, S.~Osher, and E.~Fatemi,
``Nonlinear total variation based noise removal algorithms,''
\textit{Physica D: Nonlinear Phenomena},
vol.~60, no.~1--4, pp.~259--268, 1992.

% --- Deep learning for inverse problems ---

\bibitem[Adler and \"{O}ktem(2017)]{adler2017}
J.~Adler and O.~\"{O}ktem,
``Solving ill-posed inverse problems using iterative deep neural networks,''
\textit{Inverse Problems},
vol.~33, no.~12, p.~124007, 2017.

\bibitem[Hamilton and Hauptmann(2018)]{hamilton2018}
S.~J.~Hamilton and A.~Hauptmann,
``Deep D-bar: Real-time electrical impedance tomography imaging with deep
neural networks,''
\textit{IEEE Transactions on Medical Imaging},
vol.~37, no.~10, pp.~2367--2377, 2018.

\bibitem[Fan and Ying(2023)]{fan2023}
Y.~Fan and L.~Ying,
``Solving traveltime tomography with deep learning,''
\textit{Communications in Mathematics and Statistics},
vol.~11, no.~1, pp.~3--19, 2023.

% --- PINNs and scientific machine learning ---

\bibitem[Raissi et~al.(2019)]{raissi2019}
M.~Raissi, P.~Perdikaris, and G.~E.~Karniadakis,
``Physics-informed neural networks: A deep learning framework for solving forward
and inverse problems involving nonlinear partial differential equations,''
\textit{Journal of Computational Physics},
vol.~378, pp.~686--707, 2019.

\bibitem[Lagaris et~al.(1998)]{lagaris1998}
I.~E.~Lagaris, A.~Likas, and D.~I.~Fotiadis,
``Artificial neural networks for solving ordinary and partial differential equations,''
\textit{IEEE Transactions on Neural Networks},
vol.~9, no.~5, pp.~987--1000, 1998.
```
