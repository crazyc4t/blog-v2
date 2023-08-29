---
title: LaTeX for humans
date: 2022-01-19 17:22:31Z
draft: false
author: "Said Neder"
tags: ["LaTeX"]
toc: true
categories: "Productivity"
image: "images/hellolatex.png"
license: GPLv3
math: true
---

## Intro

This post is going to help you learn \\(\LaTeX\\) and convert you into a intellectual and scientific professional and by far superior with anyone that shares their report made in word!

But before anything...

## What is all this \\(\TeX\\) thingy?
In summary \\(\LaTeX\\) is the form of creating a document with the highest quality of typesetting possible, being made with commands you will have to compile it each time to see the result so \\(\LaTeX\\) isn't a WYSIWYG(what you see is what you get) editor, but don't worry! \\(\LaTeX\\) is so awesome now that a quick `CTRL+S` will compile your result and let you see how you are going on time! so no worries there. The story goes back to 1977 when Donald Knuth understood the need for a high quality typesetting program, and \\(\LaTeX\\) being still used today shows that is the best tool for it's purpose, although many useful additions have been made and \\(\LaTeX\\) has evolutionated since then.

### What is \\(\TeX\\)?
The original system made by Donald Knuth was called \\(\TeX\\), what \\(\TeX\\) do is to take a document in plain text and convert it to a beutifully typeset document with the highest quality possible, with a lot of in-built commands available for writing beutiful math equations, writing with the right fonts specific words and so on,  but \\(\TeX\\) was still quite basic, until \\(\LaTeX\\) came by...

### \\(\LaTeX\\)
The purpose of Leslie Lamport, the creator of \\(\LaTeX\\) in the 80's was to provide a higher level language for everyone to work on, since creating a document in \\(\TeX\\) was in fact a bit complicated for doing simple things, making \\(\LaTeX\\) a \\(\TeX\\) but more friendly and easier to use, creating a package system that made the community to grow huge, being available thousands of packages to typeset whatever you want, code, images, you name it since \\(\LaTeX\\) was document classes that let you typeset books, lab reports, articles, letters, math-focused documents, awesome presentations, PhD thesis, you name it, \\(\LaTeX\\) got you covered with the highest quality available.

### pdf\\(\TeX\\) & pdf\\(\LaTeX\\)
The original \\(\TeX\\) converted the .\\(\TeX\\) files into a .dvi (DeVice Independent format) later on pdf\\(\TeX\\) being created by Hàn Thế Thành for his PhD thesis which converted .\\(\TeX\\) to .pdf being a great upgrade since pdf allowed things like hyperlinks, metadata, modern image formats, etc...

## Why learn \\(\LaTeX\\)?
Because \\(\LaTeX\\) is used all over the world! apart of creating beutifully typeset documents, it allows users to tacke complicated things like inputting math, code, bibliography, table of contents, and so much more with a breeze, and with the huge amount of open source packages for \\(\LaTeX\\) the possibilities are endless, and one of the most important reasons is that is easy and highly customizable since it separates the content from the style of the document, so you can style your document without ever needing to touch the content! and you can create your own style for the whole document very easily, that enables that creators share \\(\LaTeX\\) templates, being pre-made layouts serving a specific purpose, like a CV/resume or a template for a math book, or just your english homework of school, you name it, there is a template that can help you!

## Installing \\(\LaTeX\\)
This could be a bit complex since \\(\LaTeX\\) comes with a lot of built-in packages but don't worry! am going to explain step by step so it will be a breeze!

### Disclamer!
At this day and age is not longer necessary to install \\(\LaTeX\\), although recommended, you can use a online web editor like [overleaf](https://www.overleaf.com/) and you will be ready to rock! no caveats involved.
### \\(\LaTeX\\) for Linux
This is pretty straight forward, just install the *texlive* package for your distro!
There are a lot of packages for \\(\LaTeX\\) so they are grouped into size of the package (minimal, basic, small, medium, full)
Like so:
```
======================= Name Matched: texlive-scheme ========================
texlive-scheme-basic.noarch : basic scheme (plain and latex)
texlive-scheme-context.noarch : ConTeXt scheme
texlive-scheme-full.noarch : full scheme (everything)
texlive-scheme-gust.noarch : GUST TeX Live scheme
texlive-scheme-medium.noarch : medium scheme (small + more packages and
                             : languages)
texlive-scheme-minimal.noarch : minimal scheme (plain only)
texlive-scheme-small.noarch : small scheme (basic + xetex, metapost, a few
                            : languages)
texlive-scheme-tetex.noarch : teTeX scheme (more than medium, but nowhere
                            : near full)
```

The best package for beginners is the medium one, I use it myself! And also you can install a \\(\LaTeX\\) editor, my personal favourite is [Kile](https://kile.sourceforge.io/) is the best editor out there really, but it's from KDE, so you will need QT/KDE libraries so could be a bit heavy but if you are using GTK (gnome, cinnamon, xfce) I would recommend [gummi](https://github.com/alexandervdm/gummi) or [gnome-\\(\LaTeX\\)](https://wiki.gnome.org/Apps/GNOME-LaTeX) but is descontinued, so if you are using gnome 40-41 better go with gummi, although if you read my other post about [vim](https://saidneder.tech/blog/vimforeveryone/) you can use the [vim\\(\LaTeX\\)](https://github.com/lervag/vimtex) plugin and a document viewer like [okular](https://okular.kde.org/) or [evince](https://wiki.gnome.org/Apps/Evince), if you want to explore another alternatives to a \\(\LaTeX\\) IDE you can read [here](https://wiki.archlinux.org/title/List_of_applications/Documents#TeX_editors)

**Fedora/RHEL**
```bash
sudo dnf in texlive-scheme-medium
```

**Debian/Ubuntu**
```bash
sudo apt in texlive-latex-extra
```

**Arch BTW**
```bash
sudo pacman -S texlive-most
```

### \\(\LaTeX\\) for Windows
Really straight forward with the [MiKTeX](https://miktex.org/download) distribution of \\(\LaTeX\\)!

### \\(\LaTeX\\) for Mac
The same applies with the [MacTeX](https://www.tug.org/mactex/) distribution of \\(\LaTeX\\)!

## \\(\LaTeX\\)ing
Let's start \\(\LaTeX\\)ing baby!

```LaTeX
% This is a comment
% We specify what type of document we are creating in this case, an article
\documentclass[a4paper,10pt]{article}
% UTF-8 encoding
\usepackage[utf8]{inputenc}
% We start the document
\begin{document}
% We use \LaTeX so it can look cool
% we specify a \ after the \LaTeX function since it eats an space
Isn't that hard to \LaTeX \ as I thought!
\end{document}
% Here the document ends, anything below this command will not be considered.
```

The end result would be:

![LaTeX example](/images/hellolatex.png)

You see! is not that hard isn't it? spaces in \\(\LaTeX\\) are specified with a blank line between each line of text, just as in markdown!

Now let's create a nice formated document!

### Adding title, author and date
```LaTeX
\documentclass[a4paper,10pt]{article}
\usepackage[utf8]{inputenc}

% Document opening with title, author and date
\title{My first \LaTeX \ document}
\author{crazyc4t}
\date{\today}

\begin{document}
% Spawning the title
\maketitle

Oh yea!

\end{document}
```

The end result would be:

![LaTeX opening](/images/latexopening.png)

That was great and a breeze right?
Now let's create some sections and a table of contents with underlined, bold and italic text!

### Table of contents, sections and special text
```LaTeX
\documentclass[a4paper,10pt]{article}
\usepackage[utf8]{inputenc}

%opening
\title{My first \LaTeX \ document}
\author{crazyc4t}

\begin{document}
% Table of contents and title
\maketitle
\tableofcontents

\section{My first section}
My first section of my \LaTeX \ document!

\section{My second section}
%Bold, then underline, and finally italic
% \textbf = bold
% \textit = italic
\textbf{My} \underline{second} \textit{section!}

\end{document}
```
The end result would be:

![latexing](/images/latextableofcontents.png)

You are getting the hang of it! now you know the basics of \\(\LaTeX\\), and you will be able to write a simple document without struggling!

## Outro
\\(\LaTeX\\) is a endless world to discover and I have just showed you a tiny bit of it! the rest of your journey with \\(\LaTeX\\) is decided by you, I recommend to learn more about \\(\LaTeX\\) that what I just showed you, and after that try to write a great document for your homework, or an article just like this one! (although this one was written with markdown) or to update your CV and then learn \\(\LaTeX\\) beamer, the best way to create presentations! I will leave you lots of documentation to read and start LaTeXing in no time!

## Docs
[\\(\LaTeX\\) in 30 minutes by overleaf](https://www.overleaf.com/learn/latex/Learn_LaTeX_in_30_minutes)

[\\(\LaTeX\\) wiki, just search and you will have a result!](https://www.overleaf.com/learn)

[Luke Smith's series of tutorials about \\(\LaTeX\\)](https://www.youtube.com/watch?v=NwnYHoNtfJ0&list=PL-p5XmQHB_JSQvW8_mhBdcwEyxdVX0c1T)

[\\(\LaTeX\\) beamer tutorial by overleaf](https://www.overleaf.com/learn/latex/Beamer_Presentations%3A_A_Tutorial_for_Beginners_(Part_1)%E2%80%94Getting_Started)

Remember, **THE WORLD IS YOURS!**

