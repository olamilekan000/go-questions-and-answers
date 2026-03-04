# Concurrency Patterns – Questions

## Question 1 – How does a worker pool pattern work in Go?
Describe the core components of a worker pool (job queue, workers, results) and how goroutines and channels fit together.

## Question 2 – How would you implement a configurable worker pool?
Outline how to write a function that spins up N workers, feeds them tasks via a channel, and collects results, including how to shut the pool down cleanly.

## Question 3 – What is `sync.WaitGroup` and when do you use it?
Explain how a `WaitGroup` coordinates a set of goroutines and show the typical pattern of `Add`, `Done`, and `Wait`.

## Question 4 – How can `WaitGroup` and channels be combined?
Describe a pattern where `WaitGroup` ensures all workers finish and a channel is closed at the right time to stop a downstream consumer.

## Question 5 – What problem does `sync.Pool` solve?
Explain the purpose of `sync.Pool`, when it can improve performance, and what kinds of objects are good candidates for pooling.

## Question 6 – What are the limitations and pitfalls of `sync.Pool`?
Discuss why pooled objects may be dropped at any time, and why you can’t rely on `sync.Pool` for correctness or long‑term caching.

## Question 7 – How do you design backpressure in concurrent pipelines?
Explain how channel capacities, blocking sends/receives, and worker counts can be used to control throughput and prevent overload.

## Question 8 – How can you shut down a worker pool gracefully?
Describe techniques for signalling workers to stop (for example via context cancellation or closing a jobs channel) and draining remaining work safely.

## Question 9 – How do you choose between mutexes, channels, and other synchronization tools?
Compare scenarios where you would prefer `sync.Mutex`, channels, `sync.RWMutex`, `sync.Map`, or atomic operations.

## Question 10 – How do you test and debug concurrent Go code?
Outline practices such as using the race detector, adding timeouts, and writing deterministic tests for concurrent components.

