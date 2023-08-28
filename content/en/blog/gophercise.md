---
title: 'Gophercise #1 "Quiz" Walkthrough!'
description:
date: 2023-03-22T11:59:47-05:00
image: images/gophercises_punching.gif
license: GPLv3
comments: true
draft: false
toc: true
categories: ["Coding"]
tags: ["Go"]
author: "Said Neder"
---

What's up guys! It's crazyc4t here, once again, I'm sorry for not being fully active these past couple of weeks, but I'm here for you guys and I'm bringing you the best way to learn golang, and that is coding with it!

## What is Gophercises?

[Gophercises](https://gophercises.com/) is a webpage made by [Jon Calhoun](https://www.calhoun.io/), an experienced web developer with go, and we takes us through a series of exercises to code with golang making us improve our way to code in go by exploring topics like channels, concurrency, methods, using the standard library and more! The best thing of all is that is free! So you won't lose anything trying it out, instead you will have tons of fun like I did!

In gophercises there are a lot of exercises ranging from an order from 1 through 20, and of course they increase in difficulty, the exercises are solved by the author in case you get lost so you can get a lead, but also the author adds some additional exercises to the exercise, making you go for the extra mile.

## The "Quiz" Exercise

This exercise is based on the idea of a quiz, hence the name for it, it is a program that reads questions and answers from a csv file and count how many questions did the user got right or wrong, and ask the following questions no matter if the answer was right or wrong, all of these fields are customizable through flags, the format of the csv is as follows:

```csv
5+5,10
7+3,10
1+1,2
8+3,11
1+2,3
8+6,14
3+1,4
1+4,5
5+1,6
2+3,5
3+3,6
2+4,6
5+2,7
```

Printing at the end of the program the results of the user, after completing that, the second part is to add a timer to the exercise, being the default flag 30 seconds, but it needs to be customizable to the end user, and without any delay, cut the quiz if the time runs out and don't wait if the user is going to answer or not the question, so ungiven answers are counted as wrong answers.

### Bonus Exercises

- Clean up the answer so it will count even if it has white spaces, capitalization, etc...
- Add an option to shuffle the order of the quiz each time is runned.

Here's the official repo for the quiz exercise with detailed instructions and solutions from various students: https://github.com/gophercises/quiz

I suggest that you first try it out on your own before reading my solution, give your best shot! 頑張って！

## The solution

I will explain the code detail by detail, so no worries about it!

### Import the required packages

We will be using:

- [`encoding/csv`](https://pkg.go.dev/encoding/csv) for reading the csv file.
- [`flag`](https://pkg.go.dev/flag@go1.20.2) for creating command-line flags to give the user personalization.
- [`fmt`](https://pkg.go.dev/fmt) for formatting I/O (print & scan).
- [`log`](https://pkg.go.dev/log) for logging errors that happen.
- [`math/rand`](https://pkg.go.dev/math/rand) for randomization when shuffling order.
- [`os`](https://pkg.go.dev/os) for accessing the OS through the library.
- [`strings`](https://pkg.go.dev/string) for cleaning the answer string.
- [`time`](https://pkg.go.dev/time) for the quiz timer.
- [`cute`](https://github.com/zakaria-chahboun/cute) Pretty output library made by [zakaria-chahboun](https://github.com/zakaria-chahboun), thanks!

```go
package main

import (
	"encoding/csv"
	"flag"
	"fmt"
	"log"
	"math/rand"
	"os"
	"strings"
	"time"

	"github.com/zakaria-chahboun/cute"
)
```

### Structs for handling flags

We create a struct of the type `Exercise` that has a question and an answer, and the struct `Flag` that has a Csv, Limit and Shuffle.

We create a function to check our errors that we will use later, and to parse the flags we will save each data received in a varible of type "Flag", and return it's pointer since we used the dereferenced variable (the memory address)

```go
// Struct of the exercise
type Exercise struct {
	Question, Answer string
}

// Struct of the flags
type Flags struct {
	Csv     string
	Limit   int
	Shuffle bool
}

// Check my errors
func checkErr(description string, err error) {
	if err != nil {
		log.Println(description, err)
	}
}

// Save each flag result in an variable type Flag, parse it and return it's pointer.
func parsing() Flags {
	options := new(Flags)
	flag.CommandLine = flag.NewFlagSet(os.Args[0], flag.ExitOnError)
	flag.StringVar(&options.Csv, "csv", "problems.csv", "Insert the filepath of the csv file.")
	flag.IntVar(&options.Limit, "limit", 30, "Insert time limit of the quiz")
	flag.BoolVar(&options.Shuffle, "shuffle", false, "Wether the order of the csv file is shuffled or not")
	flag.Parse()
	return *options
}
```

### Reading the csv and creating the shuffle feature

In the `read` function create a varible that will store the csv file from the flag, defer it until we no longer need it, create a new reader and read all the fields at once, after that check if there were any errors, and return the whole reading that is a map of type `string`.

In the `serialize` function read the whole data and "serialize" it to a type `Exercise`, meaning reading the whole data and assign each field if it belongs to the answer or question field.

After that create the shuffle function where it receives a slice of data type `Exercise`, where it takes the length of the slice, and the indexes of the slice, being `i` and `j`, changing the index position of each value stored in the slice, if we remember correctly the `Exercise` data type stores a question and an answer, so when using the shuffle function, it will change the position of an exercise in the exercises slice, not a question or answer, but the whole exercise.

```go
// Read csv all at once
func read() [][]string {
	csvFile, err := os.Open(parsing().Csv)
	checkErr("Error opening file", err)

	defer csvFile.Close() // Do not close it until finished

	r := csv.NewReader(csvFile) // New reader
	r.FieldsPerRecord = -1      // Read all fields, do not expect a fixed number
	reading, err := r.ReadAll() // Data read
	checkErr("Error reading file", err)
	return reading // Give the data read as a result
}

// From the data read, create a slice that supports values type Exercise
func serialize(data [][]string) []Exercise {
	var Exercises []Exercise
	var exercise Exercise
	for _, record := range data {
		exercise.Question = record[0]
		exercise.Answer = record[1]
		Exercises = append(Exercises, exercise)
	}
	return Exercises
}

// From an slice of type Exercise, shuffle the order of the exercises
func shuffleOrder(exercises []Exercise) {
	rand.Shuffle(len(exercises), func(i, j int) {
		exercises[i], exercises[j] = exercises[j], exercises[i]
	})
}
```

### The cute output

Using the [cute](https://github.com/zakaria-chahboun/cute) library, we can create a list that will print out the quiz results, including:

- Correct answers
- Incorrect answers
- Total Questions

All of this being stored in the `quizResults` function.

```go
// Creates cute list that prints quiz results
func quizResults(correct, questions int) {
	results := cute.NewList(cute.BrightBlue, "Quiz Results!")
	results.Addf(cute.BrightGreen, "Correct Answers: %d", correct)
	results.Addf(cute.BrightRed, "Incorrect Answers: %d", questions-correct)
	results.Addf(cute.BrightPurple, "Total Questions: %d", questions)
	results.Print()
}
```

### The func main

Here it gets complex, so let's break it down step by step:

1. Create timer, serialize the whole csv reading, check if the user wants shuffling.
2. Range the exercises, and print it to the user, receive the answer in a channel, avoiding incorrect time.
3. Use the concurrent select keyword, where if the timer channel runs out, it will print the results and exit.
4. If an answer is received, check if it is correct or not, count it and store it.
5. If the user ended the quiz before the time, print the results and exit the program.

```go
func main() {
	var reply string
	var correct, questions int

	timer := time.NewTimer(time.Duration(parsing().Limit) * time.Second) // Timer duration
	Exercises := serialize(read())                                       // Slice with exercises

	if parsing().Shuffle {
		shuffleOrder(Exercises) // if true, shuffle order
	}

	for i := range Exercises { // iterate over the exercises slice
		cute.Println("Exercise", Exercises[i].Question, "= ")
		answerCh := make(chan string) // create a channel where it will receive the answer
		go func() {                   // while the timer run, listen to the answer
			fmt.Scanln(&reply)
			answerCh <- reply // store the scanned reply in the channel
		}()
		select {
		case <-timer.C: // when the timer runs out
			fmt.Println("") // print new line for pretty output
			cute.Println("Time is over!")
			quizResults(correct, questions) // print cute quiz result
			os.Exit(0)                      // exit with success
		case reply := <-answerCh: // store the answer channel in the reply variable
			if strings.TrimSpace(reply) == Exercises[i].Answer { // trim spaces of the reply to avoid incorrect replies
				cute.Println("Correct!")
				correct++ // count the correct replies
			} else {
				cute.Println("Incorrect!", "Your reply:", strings.TrimSpace(reply), "Answer:", Exercises[i].Answer) // if incorrect, show the correct answer
			}
			questions++ // count the questions
		}
	}
	quizResults(correct, questions)
}
```

## Gophercise #1 Solved!

There you have it homeboi!! Hope you enjoyed my solution of the gophercise, I will leave my github repo where I stored the solution if you want to check the whole code out, give a star or fork it, I'm so glad to write blog posts again so I will be here soon, expect tons of blog posts coming!

Github repo: https://github.com/crazyc4t/quiz
