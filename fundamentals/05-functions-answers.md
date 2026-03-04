# Grouping Code into Functions – Answers

## Answer 1 – Why are functions important for structuring Go programs?
Functions let you break complex logic into smaller, focused units that are easier to read, reason about, and test. By grouping related statements into named functions, you reduce duplication, localize changes, and create a clearer structure for your program’s behavior. They also define explicit inputs and outputs, which is critical for modularity and composability.

## Answer 2 – How do you declare a basic function in Go?
The general form is:
```go
func name(params) returnType {
    // body
}
```
For example:
```go
func add(a int, b int) int {
    return a + b
}
```
Here `func` introduces the function, `add` is the name, `(a int, b int)` are parameters with types, and `int` is the return type.

## Answer 3 – How are parameters and multiple parameters handled?
Parameters are written as `name type`, separated by commas. When several parameters share the same type, you can write:
```go
func add(a, b int) int
```
Functions can take many parameters, and the order and types of the parameters form part of the function’s signature.

## Answer 4 – What is the difference between passing by value and by pointer?
By default, Go passes arguments by value: the function receives a copy of the argument, so modifications don’t affect the original:
```go
func incrementValue(x int) {
    x = x + 1
}

func demoValue() {
    n := 10
    incrementValue(n)
    fmt.Println(n) // still 10, copy was modified
}
```

When you pass a pointer (e.g., `*T`), the function receives the address of the value, and changes through the pointer are visible to the caller:
```go
func incrementPointer(x *int) {
    *x = *x + 1
}

func demoPointer() {
    n := 10
    incrementPointer(&n)
    fmt.Println(n) // now 11, original was modified
}
```

You choose pointers when you need to mutate the caller’s data or avoid copying large structures (for example, passing `*MyBigStruct` instead of copying the whole struct on every call).

## Answer 5 – How do functions return values, including multiple values?
Functions specify return types after the parameter list. A single return value is written like `func f() int`, while multiple return values use parentheses, such as:
```go
func divide(a, b int) (int, error)
```
Multiple returns are common in Go for returning a primary result plus an `error` value, enabling explicit error handling without exceptions.

## Answer 6 – What are named return values and when might they be used?
Named return values give identifiers to return slots in the function signature:
```go
func stats(nums []int) (min int, max int) {
    // assign to min and max
    return
}
```
Inside the function you assign to `min` and `max` and can use a bare `return`. This can improve clarity in some cases but should be used sparingly, since excessive reliance on implicit returns can make control flow harder to follow.

## Answer 7 – How do variadic functions work in Go?
A variadic function accepts a variable number of arguments of the same type:
```go
func sum(nums ...int) int {
    total := 0
    for _, n := range nums {
        total += n
    }
    return total
}
```
Callers can pass any number of `int` arguments, and inside the function `nums` behaves like a slice of `int`. Variadic functions are useful for formatting, aggregation, and utility helpers.

## Answer 8 – What are anonymous functions and closures?
Anonymous functions are function literals without a name:
```go
f := func(x int) int {
    return x * x
}
```
A closure is an anonymous (or named) function that captures variables from its surrounding lexical scope. For example, a function that returns another function which increments an internal counter uses a closure to remember the counter value across calls:
```go
func makeCounter() func() int {   // outer function (factory)
    count := 0                   // captured variable
    return func() int {          // this inner anonymous func is the closure
        count++
        return count
    }
}

func demoCounter() {
    c := makeCounter()
    fmt.Println(c()) // 1
    fmt.Println(c()) // 2
    fmt.Println(c()) // 3
}
```
Here the inner function “closes over” `count`: even after `makeCounter` returns, `count` stays alive inside the returned function.

Closures are also useful to parameterize behavior:
```go
func makeAdder(delta int) func(int) int {
    return func(x int) int {
        return x + delta
    }
}

func demoAdder() {
    add2 := makeAdder(2)
    add10 := makeAdder(10)
    fmt.Println(add2(3))  // 5
    fmt.Println(add10(3)) // 13
}
```
Each returned function shares the same body but captures a different `delta`, so they behave differently.

## Answer 9 – How can functions be used as values?
In Go, functions are first‑class values: you can assign them to variables, store them in data structures, pass them as arguments, and return them from other functions. This makes it easy to implement callback patterns, custom sorting logic, or flexible processing pipelines.

## Answer 10 – How does functional composition or “filtering” show up in Go?
One idiom is to write a `filter` function that takes a slice and a predicate function:
```go
func filter(nums []int, keep func(int) bool) []int {
    var out []int
    for _, n := range nums {
        if keep(n) {
            out = append(out, n)
        }
    }
    return out
}
```
Callers can pass different predicates, such as keeping only even numbers or values above a threshold. This leverages functions as values and closures to compose behavior without complex type machinery.
