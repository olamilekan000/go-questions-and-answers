# Concurrency Fundamentals – Questions

## Question 1 – What common pitfalls make concurrency hard in practice?
List classic problems such as race conditions, deadlocks, livelocks, and starvation, and briefly define each.

## Question 2 – What is a data race, and how might it appear in Go code?
Describe how a simple read–modify–write sequence on shared state can lead to a data race with goroutines.

## Question 3 – What is atomicity and why does context matter?
Explain what it means for an operation to be atomic and how the context (program, OS, hardware) affects whether something is truly atomic.

## Question 4 – How does memory access synchronization relate to correctness?
Describe why unsynchronized reads and writes across goroutines can produce surprising results, even if there are no crashes.

## Question 5 – How do deadlocks, livelocks, and starvation differ?
Give concrete examples or scenarios that distinguish these three liveness problems.

## Question 6 – Why is “sleep for a bit” not a valid fix for race conditions?
Explain why adding `time.Sleep` calls to “wait” for goroutines rarely produces logically correct concurrent programs.

## Question 7 – How can you systematically reason about whether code is concurrency‑safe?
Outline an approach to checking whether operations on shared state are safe when run from multiple goroutines.

## Question 8 – What role do Go’s tools play in finding concurrency bugs?
Describe how the race detector and other tooling can help you find problems that are hard to reproduce manually.

## Question 9 – How do you decide what must be serialized and what can be parallelized?
Explain how Amdahl’s law and the nature of your workload influence where concurrency is worthwhile.

## Question 10 – How can you simplify a design that is too concurrency‑heavy?
Describe strategies for refactoring complex concurrent code into simpler, more maintainable components.

