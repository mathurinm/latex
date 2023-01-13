# Using $\LaTeX$

$\LaTeX$ is a high level language created by Donald Knuth that allows you to write and structure (scientific) documents through a collection of macros. $\LaTeX$ is widely adopted by the scientific community because of its elegant and standardized outputs.

## I-) Starting out

TODO: Local option (VS Code, LaTeX extention) vs online option (Overleaf). How to compile.

## II-) Creating a first doc
A basic .tex template comprises of the following macros:

```
\documentclass{article}
\usepackage[utf8]{inputenc}

\title{YOUR_TITLE}
\author{YOUR_NAME}
\date{DATE}

\begin{document}

\maketitle

YOUR_CONTENT

\end{document}
```
