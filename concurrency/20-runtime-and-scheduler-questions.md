# Runtime and Scheduler – Questions

## Question 1 – How does Go schedule goroutines onto OS threads?
Explain the high‑level model the Go runtime uses to multiplex many goroutines over a smaller number of operating system threads.

## Question 2 – What are GOMAXPROCS and the G–M–P model?
Describe what `GOMAXPROCS` controls and summarize the roles of G (goroutine), M (machine/OS thread), and P (processor) in Go’s scheduler.

## Question 3 – How does work stealing help balance load between OS threads?
Explain the idea of work stealing and how it improves utilization in Go’s runtime.

## Question 4 – How does the runtime handle blocking system calls?
Describe what happens when a goroutine performs a blocking operation such as file I/O or a network call.

## Question 5 – What is the difference between user‑level scheduling and OS‑level scheduling?
Compare Go’s cooperative, runtime‑managed scheduling of goroutines with the OS’s scheduling of threads and processes.

## Question 6 – How do goroutine stacks differ from traditional thread stacks?
Explain how goroutine stacks grow and shrink and why this matters for scalability.

## Question 7 – How can you influence concurrency and parallelism from Go code?
Discuss how `runtime.GOMAXPROCS`, environment variables, and design choices affect how much parallel work your program performs.

## Question 8 – What are potential sources of contention in the Go runtime?
List runtime‑level resources or mechanisms that can become bottlenecks and how they might show up in performance profiles.

## Question 9 – How do garbage collection and concurrency interact in Go?
Describe how Go’s garbage collector operates alongside goroutines and what impact it can have on latency and throughput.

## Question 10 – How can you observe and debug the scheduler’s behavior?
Outline tools and techniques (traces, profiles, metrics) for understanding how the scheduler is running your goroutines in production.

