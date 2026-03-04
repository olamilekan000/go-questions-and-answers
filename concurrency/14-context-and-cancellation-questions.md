# Context and Cancellation – Questions

## Question 1 – What problem does Go’s `context` package solve?
Explain why `context.Context` exists and what kinds of concerns it is designed to address in concurrent and server applications.

## Question 2 – How is a `Context` typically passed through a Go program?
Describe the conventions for where `Context` appears in function signatures and how it flows through call chains.

## Question 3 – What are the main ways to create derived contexts?
Explain the differences between `context.WithCancel`, `context.WithTimeout`, and `context.WithDeadline`, and give an example use case for each.

## Question 4 – How do you signal cancellation and how do goroutines observe it?
Describe how cancellation is triggered from the caller side and how callee code should respond to it.

## Question 5 – What is stored in a `Context` and what should not be stored there?
Explain the intended use of `context.WithValue` and why it should be used sparingly.

## Question 6 – How do you combine `Context` with channels and goroutines?
Show how to use `select` to listen to both a work channel and `ctx.Done()` in a goroutine.

## Question 7 – How does `Context` integrate with HTTP servers and clients?
Describe how `Context` is used with `http.Request`, `http.Server`, and outbound HTTP calls.

## Question 8 – What are common mistakes when using `Context`?
Identify anti‑patterns such as storing large data or ignoring `ctx.Err()` and `ctx.Done()`.

## Question 9 – How do you propagate deadlines across service boundaries?
Explain how timeouts and deadlines are carried from incoming requests to downstream calls using `Context`.

## Question 10 – What best practices apply to designing context‑aware APIs?
Summarize guidelines for function signatures, cancellation handling, and error reporting when using `Context`.

