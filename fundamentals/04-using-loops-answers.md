# Using Loops – Answers

## Answer 1 – What is the general form of a `for` loop in Go?
The classic `for` loop in Go has three components:
```go
for init; condition; post {
    // body
}
```
- The **init** statement runs once before the first iteration.
- The **condition** expression is evaluated before each iteration; if it is `false`, the loop stops.
- The **post** statement runs after each iteration, typically updating a counter.

## Answer 2 – How can you use `for` to count up and down?
To count up:
```go
for i := 0; i < 5; i++ {
    fmt.Println(i)
}
```
To count down:
```go
for i := 4; i >= 0; i-- {
    fmt.Println(i)
}
```
In both cases, the initialization sets the starting value, the condition defines when to stop, and the post statement increments or decrements the counter.

## Answer 3 – How does Go treat `++` and `--` compared to other languages?
In Go, `++` and `--` are **statements**, not expressions. It helps to be very clear about these two ideas:

- An **expression** produces a value.
  - Examples: `x + 1`, `len(s)`, `x > 0`, `foo()`, `"go" + "lang"`.
  - You use expressions:
    - On the right‑hand side of `=`, e.g. `y := x + 1`
    - As arguments, e.g. `fmt.Println(len(s))`
    - In conditions, e.g. `if x > 0 { ... }`

- A **statement** performs an action; it does not itself have a value you can use.
  - Examples: `x = 5`, `if x > 0 { ... }`, `for i := 0; i < 10; i++ { ... }`, `x++`, `x--`.

Because `x++` is a statement, you can only use it on its own line:
```go
x := 5
x++        // valid: a statement
fmt.Println(x)
```

You *cannot* use it where a value is expected, because Go does not define a value for `x++`:
```go
// all invalid in Go:
// y := ++x
// y := x++
// fmt.Println(x++)
```

This is why you will only see `i++` in places that accept statements (like the post part of a `for` loop or on its own line), and never inside larger expressions. Go avoids the confusion around pre‑ and post‑increment operators in other languages by making them statements only, never expressions.

## Answer 4 – How can a `for` loop in Go behave like a `while` loop?
By omitting the init and post statements, you can write:
```go
for condition {
    // body
}
```
This is logically equivalent to a `while` loop in other languages, where the loop continues as long as `condition` evaluates to `true`.

## Answer 5 – What is an infinite loop in Go, and how do you escape it?
An infinite loop omits all three components:
```go
for {
    // body
}
```
It runs indefinitely unless something inside the body breaks out. You use `break` to terminate the loop and `continue` to skip the rest of the current iteration and move on to the next one.

## Answer 6 – How can `for` be used to implement input loops?
A common pattern is:
```go
for {
    fmt.Println("Enter QUIT to exit")
    var input string
    fmt.Print("Please enter a string: ")
    fmt.Scanln(&input)
    if strings.ToUpper(input) == "QUIT" {
        break
    }
    // process input
}
```
This loop repeatedly prompts for input until the user types a sentinel value such as `"QUIT"`, at which point `break` exits the loop.

## Answer 7 – How do you iterate over collections with `range`?
The `for ... range` form lets you iterate over slices, arrays, maps, strings, and channels. For a slice or array:
```go
nums := []int{1, 2, 3}
for i, v := range nums {
    fmt.Println(i, v)
}
```
Each iteration yields the index and the value at that index. You can ignore the index or value by assigning it to `_`.

## Answer 8 – How do you iterate over the characters of a string?
To correctly handle Unicode, you should range over the string:
```go
for i, r := range s {
    fmt.Println(i, r)
}
```
Here `r` is a rune (a Unicode code point), and `i` is the byte index of the start of that rune. This avoids splitting multi‑byte characters, which can happen if you index into the string byte by byte.

## Answer 9 – What role do `break` and `continue` play in loops?
- `break` exits the innermost loop immediately.
- `continue` skips the rest of the current iteration and starts the next one.
For example, you can print only odd numbers:
```go
for n := 1; n < 10; n++ {
    if n%2 == 0 {
        continue
    }
    fmt.Println(n)
}
```
In nested loops, `break` and `continue` act on the innermost loop unless labels are used.

## Answer 10 – How can you use a loop to generate the Fibonacci sequence?
One compact pattern is:
```go
max := 100
for a, b := 0, 1; b <= max; a, b = b, a+b {
    fmt.Println(b)
}
```
Here `a` and `b` track consecutive Fibonacci numbers. On each iteration you print `b`, then update `a` and `b` so that `b` becomes the next Fibonacci number, stopping when `b` exceeds `max`.
