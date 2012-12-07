---
layout: post
title: "The root of all Tex"
date: 2012-12-07 13:14
comments: true
categories: latex
tags: tex root latex input include
---
Hi everyone!

Recently [we talked about](/blog/2012/12/06/latex-is-now-sublime/) how to set-up a delightful working environment for LaTeX using Sublime Text 2.

Today I want to share with you a trick that will help you while dealing with multi-part LaTeX documents using the ` \input ` or the ` \include ` commands.

<!-- more -->

# The scenario
Suppose you're working on a large and structured LaTeX document, maybe a paper for an upcoming conference, maybe a thesis or similar stuff.
Being a tidy person you correctly decided to split your document in several parts, each one in a separate ` .tex ` file.
So you end up having a ` main.tex ` document that looks like this

{% gist 4235760 %}

(**note**: I used the ` \input ` command in the above example. There's a difference between ` \input ` and ` \include ` which is well explained [here](http://tex.stackexchange.com/questions/246/when-should-i-use-input-vs-include)).

# The problem
The structure looks great but let's take a look at the annoying problem we have here.

Suppose you're working on the `abstract.tex` file and you want to compile it to see how the document looks like. As we learned in the previous pill we press `Cmd+B` (or `Ctrl+B` on Windows and Linux) in our beautiful Sublime Text editor and we are done, right? 

Unfortunately that's not the case and we'll get an error looking more or less like
```
./abstract.tex:2: LaTeX Error: Missing \begin{document}.
```

The LaTeX compiler cannot figure out by itself that the current document is included in another one (in our case `main.tex`) so it complains about the missing `\begin{document}` command.
If you want to compile your document you should go back to the `main.tex` file and do it from there.

Kind of annoying, isn't it?

# The solution
It turns out that there's a very simple and elegant way to help the compiler to figure out that a file is a part of a larger document.

Simply add this line at the top of `abstract.tex`:

```
% !TEX root = main.tex
```

This will tell the compiler that the root of our document is called `main.tex` and it will compile that one even when we are running the command while working on `abstract.tex`.

Productivity for the win!

Stay tuned for more pills

Bye