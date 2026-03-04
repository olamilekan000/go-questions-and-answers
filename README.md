# Go Questions & Answers

This repository is a comprehensive study guide designed to take you from Go fundamentals straight into the Go's powerful features.

## What's Inside

A number of crafted Questions and Answers, have been curated and broken down into digestable topics. This is to help with retaining Go's knowledge as well as it serving as a reference and revision guide when needed.

### Part 1: Go Fundamentals

These chapters cover the essential building blocks of the language.

- **[01 - Hello, Go!](fundamentals/01-hello-go-questions.md)**: Environment setup, `main.go`, and cross-compilation.
- **[02 - Working with Different Data Types](fundamentals/02-working-with-different-data-types-questions.md)**: Variables, constants, type conversions, and strings.
- **[03 - Making Decisions](fundamentals/03-making-decisions-questions.md)**: `if/else`, short-circuiting, and `switch` statements.
- **[04 - Using Loops](fundamentals/04-using-loops-questions.md)**: `for` loop variations, `range`, and `continue`/`break`.
- **[05 - Functions](fundamentals/05-functions-questions.md)**: Passing by value/pointer, multiple returns, variadics, and closures.
- **[06 - Arrays & Slices](fundamentals/06-arrays-and-slices-questions.md)**: Fixed-size arrays vs dynamic slices, appending, and copying.
- **[07 - Structs](fundamentals/07-structs-questions.md)**: Defining custom types, methods, and struct comparisons.
- **[08 - Maps](fundamentals/08-maps-questions.md)**: Key-value relationships, map iteration, and sorting.
- **[09 - JSON](fundamentals/09-json-questions.md)**: Encoding and decoding structured data to/from JSON.
- **[10 - Interfaces](fundamentals/10-interfaces-questions.md)**: Defining method signatures, implementation, and the empty interface.

### Part 2: Concurrency & System Design

These chapters dives into Go's concurrency primitives.

- **[11 - Goroutines](concurrency/11-goroutines-questions.md)**: Introduction to lightweight threads and the Go scheduler.
- **[12 - Channels](concurrency/12-channels-questions.md)**: Buffered vs unbuffered channels, `select` statements, and communication.
- **[13 - Time and Dates](concurrency/13-time-and-dates-questions.md)**: Handling the `time` package in concurrent contexts and tickers.
- **[14 - Context and Cancellation](concurrency/14-context-and-cancellation-questions.md)**: Managing timeouts, cancellation signals, and request scopes.
- **[15 - Concurrency Fundamentals](concurrency/15-concurrency-fundamentals-questions.md)**: Atomicity, data races, deadlocks, livelocks, and starvation.
- **[16 - Sync Toolbox](concurrency/16-sync-toolbox-questions.md)**: How and when to use `sync.WaitGroup`, `sync.Mutex`, and `sync.Pool`.
- **[17 - CSP and Concurrency Modeling](concurrency/17-csp-and-concurrency-modeling-questions.md)**: Understanding Communicating Sequential Processes.
- **[18 - Concurrency Patterns](concurrency/18-concurrency-patterns-questions.md)**: Implementing robust Worker Pools, Fan-out/Fan-in, and Pipelines.
- **[19 - Concurrency at Scale](concurrency/19-concurrency-at-scale-questions.md)**: Error propagation, rate limiting, and heartbeats.
- **[20 - Runtime and Scheduler](concurrency/20-runtime-and-scheduler-questions.md)**: A look under the hood at Go's work-stealing scheduler.

## How to Use This Repository

Each chapter is split into two markdown files:

1. `*-questions.md`: Use these to test your knowledge. Try to answer them out loud or on a whiteboard.
2. `*-answers.md`: Check your understanding. The answers are written to be clear and concise, with concrete, runnable Go code snippets.

## Requirements

- Go 1.18+
- A basic understanding of programming concepts.

## References & Recommended Reading

The structure and core themes of these Q&A sets draw heavy inspiration from these foundational texts, alongside other highly recommended resources:

1. **Go Programming Language For Dummies** (Wei-Meng Lee)
2. **Concurrency in Go** (Katherine Cox-Buday)
3. **The Go Programming Language** (Alan A. A. Donovan & Brian W. Kernighan)
4. **100 Go Mistakes and How to Avoid Them** (Teiva Harsanyi)
5. **Effective Go** (Official Go Documentation)

Happy learning!
