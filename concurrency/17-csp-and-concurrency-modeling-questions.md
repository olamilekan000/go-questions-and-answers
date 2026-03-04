# CSP and Concurrency Modeling – Questions

## Question 1 – How do you explain the difference between concurrency and parallelism?
Give concise definitions and an example of a program that is concurrent but not parallel.

## Question 2 – What is Communicating Sequential Processes (CSP) and why does it matter for Go?
Summarize the core idea behind CSP and how it influences Go’s concurrency design.

## Question 3 – How would you model a real‑world problem using CSP‑style thinking?
Pick a simple scenario (for example, a checkout line or job processing system) and describe it in terms of independent processes and communication channels.

## Question 4 – What is process confinement and how does it help avoid data races?
Explain the idea of confining state to a single goroutine and how other goroutines interact with it.

## Question 5 – How does Go encourage “don’t communicate by sharing memory; share memory by communicating”?
Connect this philosophy to specific language features and idioms.

## Question 6 – What is a `for`–`select` loop and where is it useful?
Describe the pattern of looping with `select` over multiple channels and give a concrete use case.

## Question 7 – How can you reason about correctness of concurrent Go code using CSP ideas?
Discuss how thinking in terms of processes and channels can make it easier to reason about ordering, blocking, and liveness.

## Question 8 – How do you detect and avoid deadlocks, livelocks, and starvation at the modeling level?
Outline how modeling reveals these problems early and how you might refactor a design to fix them.

## Question 9 – How do you model cancellation and “done” conditions in CSP‑style Go code?
Explain patterns for propagating cancellation and completion signals through a network of goroutines and channels.

## Question 10 – What are signs that your concurrency model is too complex?
List red flags in a CSP‑style design that suggest it should be simplified or refactored.

