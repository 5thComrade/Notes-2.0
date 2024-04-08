# Golang - The complete developer's guide

### Hello World!

The good old hello world of programming in Go.

Create a new file **main.go**
```go
package main

import "fmt"

func main() {
	fmt.Println("Hello world!")
}
```

- How do we run the code in our project?
  ```sh
  go run main.go
  ```
  ### Go CLI
  - compiles a bunch of go source code files
    
    ```sh
    go build
    ```
  - compiles and executes one or two files
    ```sh
    go run
    ```
  - formats all the code in each file in the current directory
    ```sh
    go fmt
    ```
  - compiles and installs a package
    ```sh
    go install
    ```
  - Downloads the raw source code of someone else's package
    ```sh
    go get
    ```
  - Runs any tests associated with the current project
    ```sh
    go test
    ```
    
- What does 'package main' mean?
  
  package == project == workspace
  
  A package can have many related files. The only requirement for all the files inside a package is that the very first line of each file must declare
  the package it belongs to.

  If there are three files main.go, support.go and helper.go all belonging to package main, then the first line of each file should be **package main**

  Files in the same package do not have to be imported into each other.

  #### Types of packages
  - Executable: Generates a file that we can run
  - Reusable: Code used as helpers. Good place to put reusable logic.
 
    So what determines if a file spits out an executable file or not.

    If you have **package main** written on top of your file it will generate an executable file called main.exe after go build.

    If you put any other package name it will be considered a reusable package.

    Any time we make a executable file using the statement **package main** the file should have a func named **main** inside.
  
- What does 'import "fmt"' mean?

  If you want to import another package into your package you use the import statement. fmt is the name of a standard library package
  that is included with the Go programming language by default.

  To know more about standard packages included vist [https://pkg.go.dev/std](https://pkg.go.dev/std)
  
- Whats the func thing?

  func is short for function.
  
- How is the main.go file organized?

  package declaration -> import other packages that we need -> declare functions, tell Go to do things
  
---
## Creating variables in Go

```go
var card string = "Ace of Spades"
```

Another way to declare a variable in GoLang is shown below

```go
card := "Ace of Spades"
```

### Basic Go Types

- bool
- string
- int
- float64

We can initialize a variable outside of a function, we just can't assign a value to it.

---

## Functions and Return types

If you are declaring another function that returns something, you should mention the return type in the function declaration

```go
package main

import "fmt"

func main() {
	var card string = newCard()
	fmt.Println(card)
}

func newCard() string {
	return "Five of Diamonds"
}
```

---

## Slices and For loops

Go has two basic data structures to handle a list of records.

- Array: fixed length list of things
- Slice: an array that can grow or shrink

Every element in an array/slice must be of same type

```go
func main() {
	cards := []string{"Card 1", "Card 2"}

	fmt.Println(cards)
}
```

To add new items into the slice

```go
cards = append(cards, "Card 3")
```

How to iterate over a slice

```go
for i, card := range cards {
	fmt.Println(i, card)
}
```

---

## Creating custom types

```go
type deck []string
```

We just created a type deck which is essentially a slice of strings.

Receiver Functions

```go
package main

import "fmt"

type deck []string

func (d deck) print() {
	for i, card := range d {
		fmt.Println(i, card)
	}
}
```

What are receiver functions?

```go
func (d deck) print()
```

Any variable of type 'deck' now gets access to the print() method. The actual copy of the deck we're working
with is available in the function as a variable called 'd'


```go
func newDeck() deck {
	cards := deck{} // creates a new variable cards of type deck which is empty

	cardSuits := []string{"Spades", "Diamonds", "Hearts", "Clubs"}
	cardValues := []string{"Ace", "Two", "Three", "Four"}

	for _, suit := range cardSuits {
		for _, value := range cardValues {
			cards = append(cards, value+" of "+suit)
		}
	}

	return cards
}
```

Split a Slice in Go

```go
fruits = []string{"apple", "banana", "mango", "pear"}

fruits[0:2] // {"apple", "banana"} notice the colon between 0 and 2. We don't comma separate the indexes
fruits[:2] // is same as fruits[0:2]
fruits[0:] // from 0 till the end of the array
```
---

## Functions with arguments and multiple returns

```go
func  deal(d deck, handSize int) (deck, deck) {
	return d[:handSize], d[handSize:]
}
```

Capture the multiple returns from a function

```go
hand, remainingDeck := deal(cards, 5)

fmt.Println("hand", hand)
fmt.Println("remainingDeck", remainingDeck)
```
