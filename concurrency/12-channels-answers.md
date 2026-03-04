# Channels and Communication – Answers

## Answer 1 – What is a channel in Go and why is it important?

A channel is a typed **pipe** that lets one goroutine send values to another. Channels embody Go’s concurrency philosophy: “Do not communicate by sharing memory; instead, share memory by communicating.” They allow you to structure concurrency around explicit message passing rather than ad‑hoc shared state.

## Answer 2 – How do you create and use channels?

You declare a channel type with `chan T` and create one with `make`:

```go
ch := make(chan int)
```

You send values with `ch <- x` and receive with `y := <-ch`. Sends and receives on an unbuffered channel block until the other side is ready, providing a synchronization point between goroutines.

## Answer 3 – What is the difference between unbuffered and buffered channels?

**Unbuffered channels**

An unbuffered channel has **no storage** or **capacity**; it only holds a value at the exact moment it is being transferred. Every send blocks until some goroutine is ready to receive, and every receive blocks until some goroutine is ready to send:

```go
ch := make(chan int) // unbuffered

go func() {
    ch <- 1          // blocks here until main is ready to receive
}()

v := <-ch            // blocks until the goroutine above sends
fmt.Println(v)       // 1
```

This gives you **strict hand‑off synchronization**: send and receive meet at the same time.

**Buffered channels**

A buffered channel has a **finite capacity** and can store up to `cap(ch)` values:

```go
buf := make(chan int, 2) // capacity 2

buf <- 1 // does not block
buf <- 2 // does not block
// buf <- 3 // would block here: buffer is full

fmt.Println(<-buf) // 1
fmt.Println(<-buf) // 2
// now the buffer is empty; further receives would block
```

With a buffer, sends block **only when the buffer is full**, and receives block **only when the buffer is empty**. Buffered channels add decoupling between producers and consumers at the cost of more moving parts to reason about.

## Answer 4 – How do you close a channel and what does closing mean?

Closing a channel with `close(ch)` signals that no more values will be sent on it. Receivers can detect closure using the “comma ok” form:

```go
v, ok := <-ch
```

When `ok` is `false`, the channel is closed and drained. You must never send on a closed channel (it panics), but it is safe to receive from a closed channel until it is empty.

## Answer 5 – How do you iterate over values coming from a channel?

You can use `for ... range`:

```go
for v := range ch {
    fmt.Println(v)
}
```

This loop continues receiving values until the channel is closed and all buffered values are consumed. It is a natural pattern for consumers that should process a stream of data and then exit cleanly.

## Answer 6 – How can `select` be used with channels?

The `select` statement lets you wait on multiple channel operations. A very common pattern is using `select` for timeouts:

```go
select {
case res := <-resultChan:
    fmt.Println("Received result:", res)
case <-time.After(2 * time.Second):
    fmt.Println("Operation timed out!")
default:
    // Optional: runs immediately if no channels are ready
    fmt.Println("No results yet, moving on...")
}
```

`select` picks a ready case at random if several are ready, or executes `default` if no case is ready and a default is provided. This is essential for timeouts, cancellation, and multiplexing I/O.

## Answer 7 – How do you avoid goroutine leaks with channels?

To prevent goroutines from getting stuck forever on channel operations, you should:

- Ensure senders or receivers eventually close channels or signal completion.
- Use `select` with a `done` or `ctx.Done()` channel to support cancellation.
- Design pipelines so that when upstream stops sending, downstream stages can detect closure and exit.
  Careful ownership and lifecycle management of channels and goroutines is key to avoiding leaks.

## Answer 8 – How can you fan‑out work and fan‑in results with channels?

In a fan‑out/fan‑in pattern:

1. A producer sends tasks on an input channel.
2. Multiple worker goroutines receive from that channel, process tasks, and send results on an output channel.
3. A coordinator goroutine collects results from the output channel.
   This pattern allows you to parallelize work while keeping task distribution and result aggregation simple and explicit.

## Answer 9 – How do you use buffered channels for asynchronous work?

Buffered channels let producers send a limited number of messages without blocking immediately, which can smooth out short‑term imbalances between production and consumption rates. For example, a logger goroutine can read from a buffered channel while application code sends log messages without being blocked on every write. The trade‑off is deciding an appropriate buffer size and handling cases where the buffer fills up.

## Answer 10 – What are best practices for designing channel‑based APIs?

Good practices include:

- Making it clear which side is responsible for closing a channel (usually the sender).
- Avoiding exposing bidirectional channels in APIs; instead, use `chan<- T` or `<-chan T` for send‑only or receive‑only channels.
- Using well‑named channels (like `done`, `results`, `errors`) to clarify intent.
- Documenting whether callers should expect the channel to close and how errors are reported (separate error channels, sentinel values, or struct wrappers).
