<!-- no toc --> 
# Go introduction

## Table of content

- [Go introduction](#go-introduction)
  - [Table of content](#table-of-content)
  - [What is Go](#what-is-go)
    - [Why is Go](#why-is-go)
    - [Who uses Go](#who-uses-go)
    - [Why use Go](#why-use-go)
    - [Go for other purposes](#go-for-other-purposes)
  - [Install Go](#install-go)
    - [Go Playground](#go-playground)
  - [Programming basics](#programming-basics)
    - [Entry point](#entry-point)
    - [Variables](#variables)
      - [Declaration and assignment](#declaration-and-assignment)
      - [Slices](#slices)
    - [Functions](#functions)
      - [Pointer vs values](#pointer-vs-values)
    - [Keywords](#keywords)
    - [Standard types](#standard-types)
  - [Structs](#structs)
    - ['Classes'](#classes)
    - [Methodes](#methodes)
    - [Interfaces](#interfaces)
  - [Exercise basic](#exercise-basic)
  - [Project Packages](#project-packages)
  - [Importing packages](#importing-packages)
    - [Go get](#go-get)
    - [Import](#import)
    - [Library documentation (Go doc, go.dev)](#library-documentation-go-doc-godev)
    - [Dependency management](#dependency-management)
    - [Vendor](#vendor)
    - [Go mod basics](#go-mod-basics)
  - [Exercise reading file](#exercise-reading-file)
  - [Webserver with std library](#webserver-with-std-library)
  - [Webserver with Gorilla mux](#webserver-with-gorilla-mux)
  - [Exercise web server](#exercise-web-server)
  - [Concurrency](#concurrency)
    - [Go keyword](#go-keyword)
    - [Sync](#sync)
    - [Channels](#channels)
  - [Exercise concurrency](#exercise-concurrency)
  - [Interesting sources of information](#interesting-sources-of-information)

## What is Go

Go also known as Golang for searchability and domain (https://golang.org),  
is a statically typed, compiled programming language designed at Google by Robert Griesemeer (JS V8), Rob Pike(UTF-8, Acme, Unix, Plan 9) and Ken Thompson (B, UTF-8, Unix, Plan 9).

It's main focus is to be a language that makes it easy to build simple, reliable and efficient software.

Features:
- Open source
- Readable syntax (C-like)
- Static typing
- Garbage collection
- Builtin concurrency support
- Powerful standard library and tools
- Builtin support for testing and benchmarking
- Fast compile
- Easy cross platform compilation and development
- ...

### Why is Go

When Go was designed mostly Java, C++ and Python was used to write server software.

Java and C++ were fast but required a lot of bookkeeping and repetition, and some programmers switched to Python for a more dynamic and fluid language at cost of efficiency and type safety.

The designers felt it should be possible to have efficiency, the safety, and the fluidity in a single language.

Throughout its design, the designers have tried to reduce clutter and complexity. Everything needs to be declared just once. Initialization is expressive, automatic, and easy to use. Syntax is clean and light on keywords

Another important principle is to keep the concepts orthogonal. Methods can be implemented for any type; structures represent data while interfaces represent abstraction; and so on. Orthogonality makes it easier to understand what happens when things combine. 

### Who uses Go

- Google: As they developed it, it makes sense that they use the language throughout the company.
- Moby (opensource part of Docker) (https://github.com/moby)
- Kubernetes (https://github.com/kubernetes)
- Uber (https://github.com/uber)
- Twitch (https://github.com/twitchtv)
- Sendgrid (https://github.com/sendgrid)
- SoundCloud (https://github.com/soundcloud)
- American Express (https://github.com/americanexpress)
- PayPal (https://github.com/paypal)
- The New York Times (https://github.com/nytimes)
- Target (American retailer) (https://github.com/target)
- ...

 It is mostly used for server applications, (cloud) infrastructure tooling, cli applications and tools, ...

### Why use Go

For the mascot of course!

![](./assets/gopher1.webp)  
![](./assets/gopher2.png)


### Go for other purposes

Go is not only a language to write server backend code, it can also be used for embedded programming with the [TinyGo](https://tinygo.org/) compiler and has a WebAssembly target to be used in frontend.

## Install Go

https://go.dev/doc/install


### Go Playground

Alternatively there is an online tool to play around with the Go language.
It has some limitations: no support for importing external packages, so only (most) of the standard library can be used, limited execution time and resource usage, ...

https://go.dev/play

## Programming basics

### Entry point

The compiler will look for the the function `main` (no args not return values) of package `main` to start running the program
```go
package main

func main(){}
```

### Variables

#### Declaration and assignment

A variable can be declared by using the keyword `var` and can then be assigned with `=`

```go
var i int // By default this will be 0
i = 1
```

This can also be done in one line
```go
var i int = 1
```

Even shorter, `:=` can be used to declare and assign at the same time
The type will be assumed from the value
```go
i := 1 // int
f := 1.0 // float64
s := "1" // string
```

Multiple variables can be declared and assigned in a single line
```go
i, j := "test", 1
```

#### Slices

To go a bit more into detail about working with arrays in Go.

Most of the time a `Slice` is used, which is an abstraction of an array that allow to have a flexible length array.
A `Slice` can be seen as a dynamic view on an array

```go
// Declaration of a fixed size array
var i [4]int

// Declaration of a dynamic size slice
var j []int

// Declaration of slice with make keyword
length := 4 // 
k = make([]int, length)

// // Declaration of slice with make keyword with capacity
capacity := 7 // size of the array being used under the hood
k = make([]int, length, capacity)


// Print length of array/slice
fmt.Println(len(k))

// Print capacity of array/slice 
fmt.Println(cap(k))
```

The capacity passed in the make command will not limit the amount of items being able to be put in the slice. It is used to set the length of the initial array so memory can be used a bit more efficiently as it provides control of the underlying array.

When using the `append` function to add another item to the slice and it would exceed the current capacity, it would increase capacity of the underlying array. (doubles up to 1024, afterwards capacity is increased with 1/4th of the current capacity)

https://go.dev/play/p/HKYTBlZIk7I


### Functions

Functions can be defined with the `func` keyword.
Arguments for the functions are defined by stating `name` and `type`. They are always named and are required to by passed when called
```go
func greet(name string) {
  fmt.Printf("Hello: %s!\n", name)
}
```

When the function returns a variable, the type needs to be added at the end of the signature:
```go
func create_greet_string(name string) string {
  return fmt.Sprintf("Hello: %s!", name)
}
```

It is possible to pass a variable amount of arguments, that can only be done as the argument and be of the same type.
This can be done with a `variadic` paramenter (... prefix).
`names` inside the function will be a slice of the defined type.

```go
package main

import (
	"fmt"
	"strings"
)

func main() {
  s := concat_names("hello world", "foo bar", "lorem ipsum")
  fmt.Println(s) // Hello World, Foo Bar, Lorem Ipsum
}

func concat_names(names ...string) string {
	names = capitalise_names(names...) // Explode into individual items
	return strings.Join(names, ", ")
}

func capitalise_names(names ...string) []string {
	for i, name := range names {
		names[i] = strings.Title(name)
	}
	return names
}
```

#### Pointer vs values

Variables in Go can be defined/passed as pointer or (a copy of) the value

Pointer type (*Type)
```go
var i *int
i = new(int) // Create a new int pointer that is not nil
```

Value of the pointer type (*variable)
```go
*i = 5
```

Get pointer of value (&variable)
```go
i := 5 // normal int
j := &i // pointer to i
```

Example:

```go
package main

import (
	"fmt"
)

func main() {
	i := 5

	setZeroValue(i)
	fmt.Println(i)

	setZeroPointer(&i)
	fmt.Println(i)
}

func setZeroPointer(i *int) {
	*i = 0
}

func setZeroValue(i int) {
	// Doesn't change the value in main as it's not a pointer
	i = 0
}
```

### Keywords

### Standard types

## Structs

### 'Classes'

### Methodes

### Interfaces

## Exercise basic


## Project Packages


## Importing packages

### Go get

### Import

### Library documentation (Go doc, go.dev)

### Dependency management

### Vendor

### Go mod basics

## Exercise reading file


## Webserver with std library

## Webserver with Gorilla mux

## Exercise web server


## Concurrency

### Go keyword

### Sync

### Channels

## Exercise concurrency

## Interesting sources of information

- [Go blog](https://go.dev/blog/)
- [Go time podcast](https://changelog.com/gotime)
- [Gophercon Youtube](https://www.youtube.com/c/GopherAcademy)
- [JustForFunc youtube tutorials by Francesc Campoy](https://www.youtube.com/c/JustForFunc)
