# The Sync Toolbox – Questions

## Question 1 – What problems does the `sync` package address in Go?
Explain the general role of the `sync` package and when you would reach for its primitives instead of channels.

## Question 2 – How does `sync.WaitGroup` coordinate goroutines?
Describe the life cycle of a `WaitGroup`, including how `Add`, `Done`, and `Wait` are used together.

## Question 3 – When would you use `sync.Mutex` versus `sync.RWMutex`?
Compare these two lock types and give examples of when each is appropriate.

## Question 4 – What is `sync.Cond` and how is it used?
Explain the purpose of a condition variable, and outline a scenario where `Cond` is a good fit.

## Question 5 – How does `sync.Once` help with one‑time initialization?
Describe the behavior of `Once` and how it simplifies safe, concurrent initialization.

## Question 6 – What is `sync.Pool` designed for?
Explain the intended use case for `Pool` and what kind of performance issues it can alleviate.

## Question 7 – How do you ensure correct lock usage with `Mutex`?
Describe patterns for acquiring and releasing locks safely, including avoiding deadlocks and ensuring unlocks even in the face of panics.

## Question 8 – How do `atomic` operations relate to the `sync` toolbox?
Explain when you would prefer `sync/atomic` over `Mutex` and what trade‑offs that entails.

## Question 9 – How can you combine `sync` primitives with channels effectively?
Give an example where both locks and channels are used in the same subsystem and explain why both are needed.

## Question 10 – What are signs that you’re overusing locks in a Go program?
List red flags that suggest it might be better to refactor toward channels, confinement, or simpler designs.

