# Goroutines and Concurrency – Questions

## Question 1 – What is a goroutine and how does it differ from an OS thread?
Explain the concept of a goroutine and why it is considered lightweight.

## Question 2 – How do you start a goroutine in Go?
Show the syntax for launching a goroutine and describe what happens when you do.

## Question 3 – What kinds of problems are goroutines well suited to solve?
Give examples of use cases where goroutines can significantly simplify or speed up code.

## Question 4 – How do shared resources create issues in concurrent code?
Explain what can go wrong when multiple goroutines access shared state without coordination.

## Question 5 – What tools does Go provide to protect shared resources?
Discuss the roles of mutexes and atomic operations in preventing data races.

## Question 6 – How can you synchronize goroutines so the main program waits for them?
Describe techniques for waiting for goroutines to finish, such as using channels or `sync.WaitGroup`.

## Question 7 – How does Go’s scheduler impact goroutine behavior?
Explain how Go schedules goroutines onto OS threads and why you usually don’t manage threads directly.

## Question 8 – What are common concurrency patterns using goroutines?
Describe patterns such as fan‑out/fan‑in, worker pools, or pipelining built on top of goroutines.

## Question 9 – How do you detect and avoid data races in Go programs?
Discuss the use of the `-race` flag and design approaches that minimize race conditions.

## Question 10 – How should you think about structuring concurrent code in Go?
Outline general principles for designing clear, maintainable concurrent programs using goroutines.

