---
title: "Programming with go!"
description: How to get started programming with Go!
date: 2022-08-28T13:25:00-05:00
image: images/gopherjumping.gif
license: GPLv3
comments: true
draft: false
toc: true
categories: ["Coding"]
tags: ["Go"]
author: "Said Neder"
---

# How to start programming with Go

Hello guys it's crazyc4t once again, sorry for being a bit distant on my blog but prepare yourselves because there's more content in the oven!

I disappeared but with a purpose, I was laser-locked focused learning go! And it is really fun, but first...

## What is go?

![Gopher](/images/gopher-dance-long-3x.gif)

[Go](https://go.dev/) is an open-source programming language supported by Google, created by Ken Thompson and Rob Pike, the creators of the UNIX operating system, made for Google-sized problems, being Go a static fast-compiled language, that includes native concurrency, garbage collection and efficiency at scale.

## Why go?

Go is a programming language easy to pick up and get started with (as we are going to do in this post!) and easier if you come from other programming languages, it's syntax is similar to C, it has built-in concurrency and a robust standard library, the go community is growing as time passes and it features tons of modules, libraries, tools to play with and documentation to learn from like this one!

Go is used for Google-sized problems, making FAANG interested in go, these are companies that use go to power their software:

- Google
- PayPal
- American Express
- Microsoft
- Meta
- Twitch
- Tweeter
- Netflix

And so much more companies as well are joining the list since is really easy to pick up and the safety+speed concept that helps them right quality code, and make debugging really easy, since the compiler lints the errors you have in your code before you even compile it!

Go is used on different code purposes like:

- Cloud & network services
- Command-line interfaces
- Web development
- DevOps

Projects that were built with go are:

- Docker
- Kubernetes
- Hugo
- Cockroach DB
- Gophish
- Arduino-CLI
- Github-CLI

## Getting started with go!

![Gopher](/images/witch-learning.png)

We need to setup our coding environment to code with go, first we need to install it.

### Installation of go

- **Linux:**
  - **Debian/Ubuntu:** `sudo apt install golang`
  - **RHEL/Fedora:** `sudo dnf install go`
  - **Arch BTW:** `sudo pacman -S go`
- **Windows & macOS:**
  - Go to this website and install the binary: [https://go.dev/dl](https://go.dev/dl)

That's it! Check if you have it installed by checking it's version: `go version`

### Setting up your text editor

You can use any type of text editor you want but my personal recommendation is:

- [VSCodium](https://vscodium.com/) (VSCode FOSS binary)
  - Install the [go extension](https://code.visualstudio.com/docs/languages/go)
- [Neovim](https://neovim.io/)
  - Install the "gopls" LSP and the "gofmt" formatter using your favorite LSP installer ([lsp-installer](https://github.com/williamboman/nvim-lsp-installer), [mason.nvim](https://github.com/williamboman/mason.nvim))
- [Jetbrains's Goland](https://www.jetbrains.com/go/)

## Write some code!

### Hello world!

```go
// Print hello world!

package main
// meaning that it should be compiled as a executable program, not as a shared libary

import "fmt" // The format package from the standard libary for printing the string

func main() {
  fmt.Println("Hello Gopher!")
}
```

After coding you can run it by just doing so:
`go run main.go`

Or you can compile it for your system!
`go build main.go`

### Let's make a greeter!

```go
package main

import "fmt"

func main() {
  fmt.Println("Hello! What's your name? ")
  var name string // creating a variable without a value of type string
  fmt.Scanf("%s", &name) // Scan the input text and format it determined by the type
  fmt.Println("Hello", name, "How are you doing?")
}
```

After scratching the surface of go, this is the next step...

## Keep learning!

![Gopher](/images/hiking.png)

These are some resources that are awesome!

### Webpages

- [https://gobyexample.com/](https://gobyexample.com/)
- [https://go.dev/tour/welcome/1](https://go.dev/tour/welcome/1)

### Tutorials

- [TechWorld with Nana Go for beginners](https://youtu.be/yyUHQIec83I)
- [FreeCodeCamp 7-hour Go tutorial](https://youtu.be/YS4e4q9oBaU)
- [Go programming: The complete developer's guide](https://www.udemy.com/course/go-programming-golang-the-complete-developers-guide/) (It's a udemy course)

### Exercises

- [CodeWars](https://www.codewars.com/)
- [Exercism](https://exercism.org/tracks/go)
- [Gophercises](https://gophercises.com/)

### Books

- [Introducing Go](http://shop.oreilly.com/product/0636920046516.do)
- [Concurrency Go](https://www.oreilly.com/library/view/concurrency-in-go/9781491941294/)
- [The Go programming language](https://www.gopl.io/)

## Thank you for reading!

I'm learning go at the moment so expect more go content coming your way!
