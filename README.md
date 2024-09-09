# Using $\LaTeX$

Disclaimer: these are general guidelines, they are not absolute.
It's OK if you disagree with them.

# Contents

1. [Work environment](#work-environment)
1. [Code presentation](#code-presentation)
1. [Basic document and project structure](#basic-document-and-project-structure)
1. [Images](#images)
1. [Link colors](#link-colors)
1. [Bibliography](#bibliography)
1. [Citing equations, sections, algorithms](#citing-equations-sections-algorithms)
1. [Mathematical operators](#mathematical-operators)
1. [Algorithms](#algorithms)
1. [Tables](#tables)
1. [Folder structure](#folder-structure)
1. [On shortcuts and additional packages](#on-shortcuts-and-additional-packages)
1. [Writing style](#writing-style)
1. [Frequent errors](#frequent-errors)
1. [Minor details](#minor-details)



## Work environment
- Use [VS Code](https://code.visualstudio.com/) + [James Yu's latex extension](https://github.com/James-Yu/LaTeX-Workshop/wiki/Install#installation)
- Build directly from vscode and keep the pdf open in dual pane. Build frequently to catch errors easily.
- Enable jumping to pdf and jumping to TeX with ctrl + click to navigate quickly in the document
- Use a spell checker to catch typos, e.g. ~~[Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)~~ [the amazing Grammarly](https://marketplace.visualstudio.com/items?itemName=znck.grammarly)
- Check out shortcuts to copy, cut or delete a line (c, v, K), to switch a line with the one above, to create a new environment (equation, figure, align), etc
- Configure your editor to remove trailing whitespaces, for lighter diffs in git.


## Git
### Git vs overleaf
My personal opinion: I strongly prefer working locally with a git repository instead of using Overleaf. I use Overleaf only if I need to work on a short period of time with many other people (e.g. a rebuttal).
The pros of working locally with git are:
- your favorite editor with its shortcuts (fast line cutting, line moving, commenting chunks of code)
- beautiful and fast PDF rendering,
- version control that allows you to see and check who wrote what
They completely outweigh the benefits of Overleaf IMO.

People complain about conflicts with git, which do happen. I found out that if you do the following, you'll greatly reduce the number of conflicts and make them much easier to solve:
- write ONE sentence per line only. This also lets you see when one of your sentences is too long
- one file per section

### Which files to ignore
At first, I used to ignore all pdfs and pngs (`*.pdf`, `*.png`), but too many times it happened that I forgot to add an image, which made compilation impossible for my coauthors.
Nowadays, I only ignore the pdfs which are the results of compilation, and do so explicitly (e.g. `main.pdf`)

## Code presentation
- Math code can be hard to parse when you want to find an error or change something. Make it easier by indenting code for readability:
  ```latex
  \begin{align}
      \lambda \norm{\Theta}_1 = \max_{\norm{U}_\infty \leq \lambda}
  \end{align}
  ```
  In big align blocks, using multiple indentation levels may help:
  ```latex
  \begin{align}
     G^*_{\MCP}(\Theta^*)
        &= \sup_{\Theta} \left\{ \langle \Theta^*, \Theta \rangle
            - G_{\MCP}(\Theta) \right\}  \\
        & = \sup_{\Theta} \left\{ \langle \Theta^*, \Theta \rangle + \logdet(\Theta)
            - \tr(S\Theta)     - \MCP(\lambda, \gamma, \Theta) \right\} \\
        & = \sup_{\Theta} \left\{ \tr(\Theta^*, \Theta)  + \logdet(\Theta)
             - \tr(S\Theta) - \MCP(\lambda, \gamma, \Theta) \right\}           \\
        & = \sup_{\Theta} \left\{\logdet(\Theta) - \tr(\Theta, \Theta^* - S)
            - \MCP(\lambda, \gamma, \Theta) \right\}
  \end{align}
  ```
- Always use the same spacing to be able to use search and replace efficiently: for example, use `x_{k + 1}` don't write `x_{k+1}` and `x_{k +1}` in other parts of the document. Spaces around binary operators help readability IMO.
- Make versioning easier by writing a single sentence per line. It also makes commenting out some parts of the code easier.

## Basic document and project structure
A basic `.tex` template for the main document comprises the following macros:

```latex
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

## Images
- Put your images in a separate folder and use `\graphicspath{{./images}}` in the document, so you don't need to write the full path when using `\includegraphics{myimage}`
- When working with git, no matter how tempting it may be, don't ignore all pdfs, pngs, etc. It'll often lead to forgetting to force add an image, which prevents your coauthors from compiling. But do ignore specific pdfs, in particular the result of the compilation of your tex; otherwise, the git history will quickly become too large.
- Images are always better in pdf format because they are vectorized.


## Link colors
Latex default flashy boxes around links can be improved with:
```latex
\usepackage{xcolor}
\definecolor{linkcolor}{RGB}{83,83,182} % you can customize
\definecolor{citecolor}{RGB}{128,0,128} % you can customize
\usepackage{hyperref}
\hypersetup{
    colorlinks=true,
    citecolor=citecolor,
    linkcolor=linkcolor,
    urlcolor=linkcolor
}
```


## Bibliography

- I recommend using the `natbib` package
- The following snippet makes bibliography links clickable (through `hyperref`), and displays them in a nicer color than the default one (flashy red/green boxes).
```latex
\usepackage{natbib}
\usepackage{hyperref}

\usepackage{xcolor}
\definecolor{linkcolor}{RGB}{83,83,182}  % customizable
\definecolor{citecolor}{RGB}{128,0,128}  % customizable
\hypersetup{
    colorlinks=true,
    citecolor=citecolor,
    linkcolor=linkcolor,
    urlcolor=linkcolor
}
\begin{document}
...
\bibliographystyle{unsrtnat}
\bibliography{REFERENCES_FILE.bib}
\end{document}
```

- For the reference entries in the `.bib`:
  - harmonize journal/conferences names abbreviations (avoid mixing "ICML" and "International Conference on Machine Learning")
  - you can usually omit fields such as URL, dates (only year is enough), editors, and publishers in conference papers.
  The title, author and conference are enough to identify the paper
  - avoid huge bibliographic files, they are a pain to maintain
- The ["Google Scholar" browser extension](https://chrome.google.com/webstore/detail/google-scholar-button/ldipcbpaocekfooobnbcddclnhejkcpn) allows you to get the BibTex citation snippet for any paper in a few seconds: type the name of a paper in its search bar, in the results list click `Cite` for the paper you're interested in, then at the bottom of the results popup, click `bibtex` and you'll get the content to copy paste in your `.bib`
- Citations should be presented differently depending on whether they are needed for the sentence to be grammatically correct; use
  - `\citet{ref_key}` if the reference is part of the sentence
  - `\citep{ref_key}` if the reference can be omitted without the sentence losing its meaning

  Example "The work presented by `\citet{REF1}` introduces such a concept, which was later proven wrong `\citep{REF2}`." -- this produces --> "The work presented by X et al. (2016) introduces such a concept, which was later proven wrong (Y et al, 2019)."; Notice we can remove (Y et al, 2019) without affecting the grammatical correctness of the sentence.


<!-- Create a `REFERENCES_FILE.bib` file to store the BibTex code of the paper of interest. -->

<!-- Typical reference structure :
```
@CATEGORY{REF_SHORTCUT,       % Usually AUTHOR_YEAR_WORD
  title     = {PAPER_TITLE},
  author    = {AUTHOR(S)},    % Usually SURNAME, N.
  journal   = {VENUE_NAME},   % Usually harmonized
  year      = {YEAR}
}
``` -->



## Citing equations, sections, algorithms

- Use the packages `hyperref` and `cleveref` together in order to easily cite and link equations, sections and other environments.
  ```latex
  \usepackage{hyperref}
  \usepackage[nameinlink]{cleveref}
  ```
  Note that `cleveref` is capricious, and for example must always be loaded after hyperref.

- Prefixing the labels with `eq:` or `pb:` or `sec:` or `sub:` helps for auto-completion: for example, use `\label{eq:pgd}`.
- Define new environments (such as definition) in the preamble with:
  ```latex
  \newtheorem{theorem}{Theorem}
  \newtheorem{NEWENVANME}[theorem]{NEWENVNAME_DISPLAYED}
  ```
  where for example NEWENVNAME is definition or def (will be used in `\begin{def} ... \end{def}`), and NEWENVNAMEDISPLAYED is Definition (what will appear in the pdf)
  The `[theorem]` option allows sharing counter with the existing `Theorem` environment.
  Doing so results in the following numbering: Definition 1, Theorem 2, Proposition 3, which makes it easier to find some results in the document (in contrast with: Theorem 1, Proposition 1, Proposition 2, Definition 1, Proposition 3)

  To number have theorem numbers prefixed with the section number (i.e. Theorem 2.1 in Section 2) use:
  ```latex
  \numberwithin{theorem}{section}
  ```

## Mathematical operators

Mathematical operators such as `sin` should not be italicized, otherwise $sin$ reads "s times i times n" (compare to: $\sin$).
To achieve this, instead of using `\mathrm` repeatedly, use `\DeclareMathOperator`:

```latex
\DeclareMathOperator*{\argmax}{argmax}
\DeclareMathOperator{\logdet}{log\,det}
```

## Algorithms
- Number lines to ease communication with reviewers and readers,
- Use `\tcp{}` to add inline comments, customize location with `\tcp*[l]` for example. For more info, see [page 32 of the manual](http://tug.ctan.org/macros/latex/contrib/algorithm2e/doc/algorithm2e.pdf).
```latex
\usepackage{algorithm}
\usepackage{algorithmic}
\usepackage[titlenumbered,linesnumbered,ruled,noend,algo2e]{algorithm2e}
\newcommand\mycommfont[1]{\footnotesize\ttfamily\textcolor{blue}{#1}}
\SetCommentSty{mycommfont}
\SetEndCharOfAlgoLine{}
```

- Reset the line counter in each new algorithm with the command `\setcounter{AlgoLine}{0}`
- Put your algorithms at the top of their page/column with `\begin{algorithm}[t]` (`t` for top)

## Tables
Use the `booktabs` package for pretty tables; avoid the default format for tables.
Read this: https://people.inf.ethz.ch/markusp/teaching/guides/guide-tables.pdf

- Avoid vertical lines
- Avoid “boxing up” cells, usually 3 horizontal lines are enough: above, below, and after heading (top and bottom one bolder than the other)
- Enough space between rows

## Folder structure
- To minimize the number of conflicts and to navigate quickly between files, you can have one `.tex` file per section (usually placed together in a `section` folder), combined with `\input{yourfilename}` in your `main.tex`.
  This keeps a light main document.

  If you do so, write `%!TEX root = ../main.tex` at the top of each of your section files so that you can build with VS code directly from this file.
- Folder structure in a nutshell
```
  your_document_folder/
  ├── sections/            <-- split document into sections
  |   └── section1.tex
  |   └── section2.tex
  |—— images/              <-- where to put figures/images
  |   └── image1.png
  |   └── figure.pdf
  |——— main.tex            <-- document entry
  |——— bibliography.bib    <-- where to put references
```
<!-- one subfolder per conference  -->

## On shortcuts and additional packages

- Use custom shortcuts and additional packages parsimoniously: they make collaborating less easy. There's always a technical debt to having a 1000 lines shortcut file.

  Declare only the shortcuts you need, do not copy-paste from one project to the other: the latter leads to uncontrolled growth and, often in my experience, wasted time in the end.

  In addition, some packages conflict with each other, and some packages can't be used when using a particular journal template: every package you rely on is a potential liability, so keep that in mind (there's a tradeoff, many packages are very useful)


- Some useful shortcuts in my lab (YMMV)
  ```latex
  \newcommand{\bbR}{\mathbb{R}}
  \newcommand{\cO}{\mathcal{O}}
  \DeclarePairedDelimiter{\abs}{\lvert}{\rvert}
  \DeclarePairedDelimiter{\norm}{\lVert}{\rVert}
  % etc
  ```

- Declare shortcuts inside your main document, or inside an additional file `shortcuts.sty` + use `\usepackage{shortcuts}` in your preamble


## Writing style

- it is often useful to remind the reader what the objects are: instead of "where $f$ is differentiable", write "where the objective function $f$ is differentiable".
- it is also useful to remind the space the variable live in, e.g. $\min_{x \in \mathbb{R}^d}$ instead of simply $\min_x$




## Frequent errors
- When writing an operator, don't use `\mathrm{argmin}`. Instead, declare a math operator: `\DeclareMathOperator{\argmin}{arg\,min}`

## Minor details
- Use `` for opening quotes and '' for closing ones
- Render beautiful dashes with a double dash: --


## Useful VSCode settings
To help you when writing a tex file, you can add the following to your VSCode user setting (access them by opening a command paalette and typing "user settings json"):
```json
"latex-workshop.synctex.synctexjs.enabled": true,
"latex-workshop.synctex.afterBuild.enabled": true,
"latex-workshop.synctex.indicator.enabled": true
```
These settings will move your pdf rendering to the location corresponding to your cursor location in the .tex, right after compilation.

The following setting will give you a more attractive rendering, as lines will be automatically line-fed (without modifying the file, but only the display).
It will also make it more evident to you when some sentences are too long.
```json
"editor.wordWrap": "on"
```

# Advanced features and snippets


## Defining a `Problem` equation-like environment
Use the following snippet:
```latex
\usepackage{aliascnt}
\newaliascnt{problem}{equation}
\aliascntresetthe{problem}
\creflabelformat{problem}{#2\textup{(#1)}#3}
\makeatletter
\def\problem{$$\refstepcounter{problem}}
\def\endproblem{\eqno \hbox{\@eqnnum}$$\@ignoretrue}
\makeatother
\Crefname{problem}{Problem}{Problems}
```

## Creating a nicely typeset += operator

```latex
\newcommand{\pluseq}{\mathrel{{+}{=}}}
```


## Restating theorems in appendix
TODO
