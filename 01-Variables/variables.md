# Variables in GO

## Basic Variables
- `bool`: a boolean value, either true or false
- `string`: a sequence of characters
- `int`: a signed integer
- `float64`: a floating-point number
- `byte`: exactly what it sounds like: 8 bits of data

**Declaring a Variable the Sad Way**
```go
var mySkillIssues int
mySkillIssues = 42

// or you can use `:=` to declare var

mySkillIssues := 42

```

The `walrus operator, :=`, declares a new variable and assigns a value to it in one line. 

The limitation is that := can't be used outside of a function (in the global/package scope which we'll talk about later).

### Check the type of variable in go: 
```go
// use the `%T` to print out the type of the variable
package main

import "fmt"

func main() {
    x := 42
    y := "hello"
    fmt.Printf("Type of x: %T\n", x)
    fmt.Printf("Type of y: %T\n", y)
}
```

```go
// `reflect.TypeOf` to get the type of variable programmatically
package main

import (
    "fmt"
    "reflect"
)

func main() {
    x := 42
    t := reflect.TypeOf(x)
    fmt.Println("Type of x:", t)
}
```


## The Compilation Process
Computers need machine code, they don't understand English or even Go. We need to convert our high-level (Go) code into machine language, which is really just a set of instructions that some specific hardware can understand. In your case, your CPU.

The Go compiler's job is to take Go code and produce machine code, an .exe file on Windows or a standard executable on Mac/Linux.

![diagram](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/rfR5MNc-1280x490.png)


## Go Program Structure
We'll go over this all later in more detail, but to sate your curiosity:

`package main` lets the Go compiler know that we want this code to compile and run as a standalone program, as opposed to being a library that's imported by other programs.
`import "fmt"` imports the fmt (formatting) package from the standard library. It allows us to use fmt.Println to print to the console.
`func main()` defines the main function, the entry point for a Go program.
Two Kinds of Bugs

Generally speaking, there are two kinds of errors in programming:

`Compilation errors`. Occur when code is compiled. It's generally better to have compilation errors because they'll never accidentally make it into production. You can't ship a program with a compiler error because the resulting executable won't even be created.
`Runtime errors`. Occur when a program is running. These are generally worse because they can cause your program to crash or behave unexpectedly.

---
## Type Sizes
Integers, uints, floats, and complex numbers all have type sizes.

### Signed Integers (No Decimal)
int  int8  int16  int32  int64

### Unsigned Integers (Whole Numbers/No Decimal)
"uint" stands for "unsigned integer".

uint uint8 uint16 uint32 uint64 uintptr

### Signed Decimal Numbers
float32 float64

### Complex Numbers (a Complex Number Has a Real and Imaginary Part)
complex64 complex128

**Converting Between Types**
Some types can be easily converted like this:

```go
btemperatureFloat := 88.26
temperatureInt := int64(temperatureFloat)
```

## Default types in GO

- bool
- string
- int
- uint
- byte
- rune
- float64
- complex128


## Go Is Statically Typed
Go enforces static typing meaning variable types are known before the code runs. That means your editor and the compiler can display type errors before the code is ever run, making development easier and faster.

Contrast this with most dynamically typed languages like JavaScript and Python... Dynamic typing often leads to subtle bugs that are hard to detect. The code must be run to catch syntax and type errors. (sometimes in production if you're unlucky 😨)

**Concatenating Strings**
Two strings can be concatenated with the + operator. But the compiler will not allow you to concatenate a string variable with an int or a float64.

---
## Compiled vs. Interpreted
You can run a compiled program without the original source code. You don't need the compiler anymore after it's done its job. That's how most video games are distributed! Players don't need to install the correct version of Go to run a PC game: they just download the executable game and run it.

![diagram](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/7RBQRNA-994x720.png)

--- 
## Same Line Declarations
You can declare multiple variables on the same line:
```go

mileage, company := 80276, "Toyota"

// The above is the same as:

mileage := 80276
company := "Toyota"

```

---
## Small Memory Footprint
Go programs are fairly lightweight. Each program includes a small amount of extra code that's included in the executable binary called the Go Runtime. One of the purposes of the Go runtime is to clean up unused memory at runtime. It includes a garbage collector that automatically frees up memory that's no longer in use.

**Comparison**
As a general rule, Java programs use more memory than comparable Go programs. There are several reasons for this, but one of them is that Java uses a virtual machine to interpret bytecode at runtime and typically allocates more on the heap.

On the other hand, Rust and C programs use slightly less memory than Go programs because more control is given to the developer to optimize the memory usage of the program. The Go runtime just handles it for us automatically.

**Go Binary Structure - Self-Contained Runtime**

Go binaries are self-contained, meaning the compiled executable includes everything needed to run. This includes:


- Go Runtime: Garbage collector, scheduler, goroutine management, I/O.
- Standard Library: All used packages.
- Third-party Dependencies: All external modules and their dependencies.
Key Advantages:


- Easy Deployment: Single executable, no external runtime or library installations needed.
- Version Stability: Runtime and library versions are locked in at compile time, preventing compatibility issues.
Considerations:

- Larger Binary Size: Due to bundling everything, binaries can be larger than dynamically linked alternatives, though Go's linker is efficient.

---

## Constants
Constants are declared with the const keyword. They can't use the := short declaration syntax.

```go
const pi = 3.14159

```

Constants can be primitive types like strings, integers, booleans and floats. They can not be more complex types like slices, maps and structs, which are types we will explain later.

---
## Computed Constants
Constants must be known at compile time. They are usually declared with a static value:

const myInt = 15

However, constants can be computed as long as the computation can happen at compile time.

For example, this is valid:

```go
const firstName = "Lane"
const lastName = "Wagner"
const fullName = firstName + " " + lastName

// That said, you cannot declare a constant that can only be computed at run-time like you can in JavaScript. This breaks:

// the current time can only be known when the program is running
const currentTime = time.Now()

```

---
## Comparing Go's Speed
Go is generally faster and more lightweight than interpreted or VM-powered languages like:

- Python
- JavaScript
- PHP
- Ruby
- Java

However, in terms of execution speed, Go does lag behind some other compiled languages like:
- C
- C++
- Rust

Go is a bit slower mostly due to its automated memory management, also known as the "Go runtime". Slightly slower speed is the price we pay for memory safety and simple syntax!


![diagram](https://storage.googleapis.com/qvault-webapp-dynamic-assets/course_assets/tUIWLob-705x400.png)

---
## Formatting String in go

Go follows the printf tradition from the C language. In my opinion, string formatting/interpolation in Go is less elegant than Python's f-strings, unfortunately.

- `fmt.Printf` - Prints a formatted string to standard out.
- `fmt.Sprintf()` - Returns the formatted string
These following "formatting verbs" work with the formatting functions above:

#### Default Representation
The `%v` variant prints the Go syntax representation of a value, it's a nice default.

```go
s := fmt.Sprintf("I am %v years old", 10)
// I am 10 years old

s := fmt.Sprintf("I am %v years old", "way too many")
// I am way too many years old

```

If you want to print in a more specific way, you can use the following formatting verbs:

**String**
```go
s := fmt.Sprintf("I am %s years old", "way too many")
// I am way too many years old

// Integer
s := fmt.Sprintf("I am %d years old", 10)
// I am 10 years old

// Float
s := fmt.Sprintf("I am %f years old", 10.523)
// I am 10.523000 years old

// The ".2" rounds the number to 2 decimal places
s := fmt.Sprintf("I am %.2f years old", 10.523)
// I am 10.52 years old

```

If you're interested in all the formatting options, you can look at the fmt package's docs.


