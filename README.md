# Using $\LaTeX$

Disclaimer: those are general guidelines, they are not absolute.
It's OK disagree with them.

## Work environment
- Use [VS Code](https://code.visualstudio.com/) + [James Yu's latex extension](https://github.com/James-Yu/LaTeX-Workshop/wiki/Install#installation)
- build from directly from vscode  and keep the pdf open in dual pane. Build frequently to catch errors easily.
- enable jumping to pdf and jumping to TeX with ctrl + click to navigate quickly in document
- use a spell checked to catch typos, e.g. [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
- personal opinion: work locally with a git repository instead of using Overleaf. Use Overleaf only if you need to work on a short period of time with other people (e.g. a rebuttal). In other cases, the pros of working locally (use your favorite editor, beautiful and fast pdf rendering, version control that allows you to see who wrote what) overweigh the benefits of Overleaf.
- check out shortcuts to copy, cut, delete a line (c, v, K), to switch a line with the one above, etc.


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
- always use the same spacing to be able to use search and replace efficiently: for example, use `x_{k + 1}` don't write `x_{k+1}` and `x_{k +1}` in other parts of the document. Spaces around binary operators help readability IMO.
- make versioning easier by writing a single sentence per line. It also makes commenting some parts of the code easier.

## Basic document and project structure
A basic `.tex` template comprises the following macros:

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
- When working with git, no matter how tempting it may be, don't ignore all pdfs, pngs, etc. It'll often lead to forgetting to force add an image, which prevents your coauthors from compiling. But do ignore specific pdfs, in particular the result of the compilation of your tex; otherwise the git history will quickly become too large.


## Bibliography
The following snippet make bibliography link clickable (through `hyperref`), and displays them in a nicer color than the default one (flashy red/green boxes).
```latex
\usepackage{natbib}
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

...
\bibliographystyle{unsrtnat}
\bibliography{REFERENCES_FILE.bib}
\end{document}
```

For the entries in the `.bib` file:
- harmonize journal/conferences names abbreviations (avoid mixing "ICML" and "International Conference on Machine Learning")
- no need for url, dates (only year is enough), editors, and publishers in conference papers (keep it simple, title, author and conference are enough for readers to identify the paper)
- the "Google Scholar" browser extension allows you get the bibtex citation snippet for any paper in a few seconds: type the name of a paper in its search bar, in the results list click `Cite` for the paper you're interested in, then at the bottom of the result popup, click `bibtex` and you'll get the content to copy paste in your `.bib`
- avoid huge bibliographic files, they are a pain to maintain
- citations should be presented differently depending on whether or not they are an integral part of the corpus  (i.e. the sentence wouldn't make sense without it):
"The work presented in `\citet{REF1}` introduces such concept, which was later proven wrong `\citep{REF2}`."

when a citation is part of a sentence, use `\citet{someref}`: "As shown by X et al. (2016), it is better to...". When the citation is NOT part of the sentence, use `\citep` (p for parenthesis): "It is better to Y (X et al., 2016)"


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

- Use the packages hyperref and cleveref together in order to easily cite and link equations, sections and other environments.
  ```latex
  \usepackage{hyperref}
  \usepackage[nameinlink]{cleveref}
  ```
  Note that `cleveref` is capricious, and for example must always be loaded after hyperref.

- Prefixing the labels with `eq:` or `pb:` or `sec:` or `sub:` helps for autocompletion: for example, use `\label{eq:pgd}`.
- define new environments with:
  ```latex
  \newtheorem{NEWENVANME}[theorem]{NEWENVNAME_DISPLAYED}
  ```

  The `[theorem]` option allows to share counter with the existing `Theorem` environment.
  Doing so results in the following numbering: Definition 1, Theorem 2, Proposition 3, which makes it easier to find some result in the document (in contrast to: Theorem 1, Proposition 1, Proposition 2, Definition 1, Proposition 3)

## Mathematical operators

Mathematical operators such as `sin` should not be italicized, otherwise it reads a "s times i times n".
To achieve this, instead of using `\mathrm` repeatedly, use:

```latex
\DeclareMathOperator*{\argmax}{argmax}
\DeclareMathOperator{\logdet}{log\,det}
```

## Algorithms
- number lines to ease communication with reviewer and readers,
- use `\tcp{}` to add inline comments
```latex
\usepackage{algorithm}
\usepackage{algorithmic}
\usepackage[titlenumbered,linesnumbered,ruled,noend,algo2e]{algorithm2e}
\newcommand\mycommfont[1]{\footnotesize\ttfamily\textcolor{blue}{#1}}
\SetCommentSty{mycommfont}
\SetEndCharOfAlgoLine{}
```

- Reset algo line counter in each new algorithm with the command `\setcounter{AlgoLine}{0}`
- Put your algorithms at the top of their page/column with `\begin{algorithm}[t]` (`t\ for top)

## Folder structure:
- To minimize the number of conflicts and to navigate quickly between files, you can have one `.tex` file per section, combined with `\input{yourfilename}` in your `main.tex`.
This keeps a light main document.
<!-- one subfolder per conference  -->

## On shortcuts and additional packages

- Use custom shortcuts and additional packages parcimoniously: they make collaborating less easy. There's always a technical debt to having a 1000 lines shortcut file.

  Declare only the shortcuts you need, don't copy paste from one project to the other: the latter leads to uncontrolled growth and, often in my experience, wasted time in the end.

  In addition, some packages conflict with each other, some packages can't be used when using a particular journal template: every package you rely on is a potential liability, so keep that in mind (there's a tradeoff, many packages are very useful)


- Some useful shortcuts in my lab (YMMV)
  ```latex
  \newcommand{\bbR}{\mathbb{R}}
  \newcommand{\cO}{\mathcal{O}}
  \DeclarePairedDelimiter{\abs}{\lvert}{\rvert}
  \DeclarePairedDelimiter{\norm}{\lVert}{\rVert}
  % etc
  ```

- Declare shortcuts inside your main document, or inside an additional file `shortcuts.sty` + use `\usepackage{shortcuts}` in your preamble


## Frequent errors
- When writing an operator, don't use `\mathrm{argmin}`. Instead, declare a mathoperator: `\DeclareMathOperator{\argmin}{arg\,min}`



# Advanced features

## Restating theorems in appendix
TODO


## Defining a `Problem` equation-like environement
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

