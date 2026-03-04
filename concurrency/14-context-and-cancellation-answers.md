# Context and Cancellation – Answers

## Answer 1 – What problem does Go’s `context` package solve?
The `context` package provides a standard way to carry cancellation signals, deadlines, and request‑scoped values across API boundaries and between goroutines. It solves the problem of coordinating timeouts and cancellations in concurrent systems, especially servers handling many requests, without inventing ad‑hoc mechanisms in every package.

## Answer 2 – How is a `Context` typically passed through a Go program?
By convention, a `Context`:
- Is passed as the **first parameter** to functions and methods that need it, with the name `ctx`.
- Is treated as immutable; instead of modifying it, functions derive new contexts from an existing one.
- Flows from the top (such as an incoming request handler) down through the call stack, so all work associated with that request shares the same cancellation and deadline information.

## Answer 3 – What are the main ways to create derived contexts?
Key helper functions include:
- `context.WithCancel(parent)`: returns a child context and a `cancel` function; calling `cancel()` signals that the context is done.
- `context.WithTimeout(parent, d)`: like `WithCancel`, but also automatically cancels when the timeout `d` elapses.
- `context.WithDeadline(parent, t)`: like `WithTimeout`, but with an explicit deadline time.
These derived contexts allow callers to bound the lifetime of work or cancel it in response to external events.

## Answer 4 – How do you signal cancellation and how do goroutines observe it?
Callers signal cancellation by invoking the `cancel` function returned by `WithCancel`, `WithTimeout`, or `WithDeadline`. Goroutines that receive a `Context` should:
- Listen on `ctx.Done()` in a `select` statement.
- Check `ctx.Err()` to distinguish between timeout, explicit cancellation, or no error.
When `Done()` is closed, the goroutine should quickly stop work, clean up, and return.

## Answer 5 – What is stored in a `Context` and what should not be stored there?
`context.WithValue` allows attaching small, request‑scoped values such as authentication tokens, request IDs, or locale information. However, `Context` is **not** a general‑purpose key‑value store:
- Do not store large or optional data structures.
- Do not store values that are better passed as explicit parameters.
Overusing `WithValue` makes code harder to understand and test.

## Answer 6 – How do you combine `Context` with channels and goroutines?
A common pattern is:
```go
for {
    select {
    case <-ctx.Done():
        return
    case msg, ok := <-ch:
        if !ok {
            return
        }
        // process msg
    }
}
```
This structure lets a goroutine stop either when the channel is closed or when the context is cancelled or times out, preventing leaks.

## Answer 7 – How does `Context` integrate with HTTP servers and clients?
In HTTP servers:
- Each incoming `*http.Request` has a context accessible via `r.Context()`, which is cancelled when the client disconnects or the server times out.
In HTTP clients:
- You can attach a context to a request with `req = req.WithContext(ctx)`, so outbound calls respect deadlines and cancellations.
This ensures that timeouts and cancellation propagate naturally across service boundaries.

## Answer 8 – What are common mistakes when using `Context`?
Common mistakes include:
- Ignoring `ctx.Done()` and `ctx.Err()`, causing work to continue after callers have given up.
- Storing large data or long‑lived resources in the context.
- Creating background contexts inside libraries instead of accepting a context parameter from callers.
These patterns defeat the purpose of context and make cancellation ineffective.

## Answer 9 – How do you propagate deadlines across service boundaries?
You typically derive a context with a timeout or deadline near the edge of your system (for example, in an HTTP handler), then pass that context to all downstream operations and outbound calls. Each service respects the same or stricter deadline, so if the original request times out, downstream work is also cancelled and resources are freed in a coordinated way.

## Answer 10 – What best practices apply to designing context‑aware APIs?
Best practices include:
- Accept `context.Context` as the first parameter in any function that performs I/O, blocking operations, or long‑running work.
- Never store `Context` in a struct; pass it explicitly.
- Always call the `cancel` function you receive from `WithCancel`, `WithTimeout`, or `WithDeadline` when you’re done with the context.
- Propagate contexts downward instead of creating new background contexts.
Following these guidelines leads to APIs that are easier to compose, test, and reason about under load.
