# Using $\LaTeX$

$\LaTeX$ is a high level language created by Donald Knuth that allows you to write and structure (scientific) documents through a collection of macros. $\LaTeX$ is widely adopted by the scientific community because of its elegant and standardized outputs.

## Work environment
- VSCODE + vscode latex by James Yu
- enable jumping to pdf and jumping to tex with ctrl + click
- build from vscode and view pdf in dual pane
- use Spellcheck to catch typos

TODO: Local option (VS Code, LaTeX extention) vs online option (Overleaf). How to compile.

## II-) Basic document structure
A basic `.tex` template comprises of the following macros:

```
\documentclass{article}
\usepackage[utf8]{inputenc}

% all of your \usepackage go here. Group them by similar purposes.
% For readability separate blocks with %%%%% lines eg:
%%%%% algorithms related %%%%



\title{YOUR_TITLE}
\author{YOUR_NAME}
\date{DATE}  % leave empty for no date

\begin{document}

\maketitle

% content goes here.
% TODO use \input{sections/}

\end{document}
```





## Bibliography
```
\usepackage{natbib}
```
- harmonize journal names abbreviations
- no need for url, dates (only year is enough), publishers in conference papers
- difference between citing with and without parenthesis (citep/citet)

## Citing equations, sections, etc
```
\usepackage{hyperref}
\usepackage[nameinlink]{cleveref}
```
cleveref must always be loaded after hyperref.

Prefixing the labels with `eq:` or `pb:` or `sec:` or `sub:` helps for autocompletion: for example, use `\label{eq:pgd}`.


## Mathematical operators

```
\DeclareMathOperator*{\argmax}{argmax}
\DeclareMathOperator{\logdet}{log\,det}
```

## Algorithms
- number lines to ease communication with reviewer and readers,
- use `\tcp{}` to add inline comments
```
\usepackage{algorithm}
\usepackage{algorithmic}
\usepackage[titlenumbered,linesnumbered,ruled,noend,algo2e]{algorithm2e}
\newcommand\mycommfont[1]{\footnotesize\ttfamily\textcolor{blue}{#1}}
\SetCommentSty{mycommfont}
\SetEndCharOfAlgoLine{}
```

Reset algo line counter in each new algorithm


## Code presentation
- indent code for readability:
```
\begin{align}
    \lambda \norm{\Theta}_1 = \max_{\norm{U}_\infty \leq \lambda} \tr(\Theta U) =  \min_{\norm{U}_\infty \leq \lambda} -\tr(\Theta U)
\end{align}
```

- always use the same spacing to be able to use search and replace efficiently: use `x_{k + 1}` don't write `x_{k+1}` and `x_{k +1}` in other parts of the document. Spaces around binary operators help readability IMO.


## Restating theorems in appendix


## folder structure:
tex folder, one subfolder per conference

## on shortcuts and additional packages
use them parcimoniously: they make collaborating less easy. There's a technical debt to having a 1000 lines shortcut file.
Some package conflict with each other, some packages can't be used when using a particular journal template > travel lightly
declare only the shortcuts you need, don't copy paste from one project to the other (incontrolled growth)

## some useful shortcuts in my lab (YMMV)
```
\newcommand{\bbR}{\mathbb{R}}
\\newcommand{\cO}{\mathcal{O}}
\DeclairedPairDelimiters{norm}
```