# Making Decisions – Answers

## Answer 1 – How are booleans and comparison operators used in Go?
In Go, boolean values (`true` and `false`) are typically produced by comparison expressions. Common comparison operators include:
- `==` (equal to)
- `!=` (not equal to)
- `<` (less than)
- `<=` (less than or equal to)
- `>` (greater than)
- `>=` (greater than or equal to)
For example, `num == 0` evaluates to a boolean that can be used to control program flow.

## Answer 2 – What logical operators does Go provide and how do they behave?
Go provides three logical operators:
- `&&` (logical AND): evaluates to `true` only if both operands are `true`.
- `||` (logical OR): evaluates to `true` if at least one operand is `true`.
- `!` (logical NOT): inverts a boolean value (`true` becomes `false` and vice versa).
These operators allow you to combine or negate boolean expressions when making decisions.

## Answer 3 – How do you write a basic `if`/`else` statement in Go?
A basic `if`/`else` looks like this:
```go
if num%2 == 1 {
    fmt.Println("Number is odd")
} else {
    fmt.Println("Number is even")
}
```
The condition must be a boolean expression; unlike some languages, Go does not allow non‑boolean values (such as integers) to be used directly as conditions.

## Answer 4 – What is short‑circuit evaluation and why is it important?
Short‑circuit evaluation means that in expressions using `&&` and `||`, Go stops evaluating as soon as the overall result is known. For example, in `a || b`, if `a` is `true`, `b` is never evaluated; in `a && b`, if `a` is `false`, `b` is not evaluated. This matters when the operands are function calls or expressions with side effects, and it is often used deliberately to avoid unnecessary work or panics.

## Answer 5 – How can you keep variable scope tight in `if` statements?
You can use an initialization statement within an `if` to limit the scope of variables:
```go
if v, err := doSomething(); err != nil {
    // handle error
} else {
    fmt.Println(v)
}
```
Here `v` and `err` exist only within the `if`/`else` block, which keeps the surrounding scope clean and avoids accidentally reusing these variables elsewhere.

## Answer 6 – Why doesn’t Go have a ternary operator?
Go deliberately omits the `?:` ternary operator because it can encourage dense, hard‑to‑read expressions. The language favors clarity over brevity, so you are expected to write out `if`/`else` blocks or small helper functions instead. While more verbose, this style tends to be easier to read and maintain.

## Answer 7 – How is a `switch` statement structured in Go?
A typical `switch` looks like:
```go
switch num {
case 1:
    day = "Monday"
case 2:
    day = "Tuesday"
// ...
default:
    day = "Unknown"
}
```
The value after `switch` (here `num`) is compared against each `case` value in order. When a match is found, the associated block runs and, by default, control exits the `switch` without needing explicit `break` statements.

## Answer 8 – What is `fallthrough` in Go’s `switch`, and when would you use it?
By default, Go executes only the matched `case` block and then leaves the `switch`. The `fallthrough` keyword tells Go to continue executing the statements in the next `case` as well, regardless of whether its condition matches. This can be useful when multiple grades or categories should share the same handling, such as treating `"A"`, `"B"`, `"C"`, and `"D"` all as “pass” cases.

For example:
```go
switch grade {
case "A":
    fallthrough
case "B":
    fallthrough
case "C":
    fallthrough
case "D":
    fmt.Println("Passed")
case "F":
    fmt.Println("Failed")
default:
    fmt.Println("Absent or undefined")
}
```
If `grade` is `"B"`, execution enters the `"B"` case, then `fallthrough` forces it into the `"C"` case, then into the `"D"` case, where `"Passed"` is printed. This way, all grades A–D are handled by the same final case body without duplicating the `fmt.Println("Passed")` line.

## Answer 9 – How can a single `case` match multiple values?
You can list multiple comma‑separated values in a single `case`:
```go
switch grade {
case "A", "B", "C", "D":
    fmt.Println("Passed")
case "F":
    fmt.Println("Failed")
default:
    fmt.Println("Undefined")
}
```
This pattern simplifies code when several distinct values should be handled identically.

## Answer 10 – What does it mean to switch “without a condition”?
A `switch` without a condition omits the tag expression:
```go
switch {
case score < 50:
    grade = "F"
case score < 60:
    grade = "D"
case score < 70:
    grade = "C"
case score < 80:
    grade = "B"
default:
    grade = "A"
}
```
Each `case` then holds its own boolean condition, making this a concise alternative to a long chain of `if`/`else if` range checks.

