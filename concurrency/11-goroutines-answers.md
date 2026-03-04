# Goroutines and Concurrency – Answers

## Answer 1 – What is a goroutine and how does it differ from an OS thread?
A goroutine is a function running concurrently with other goroutines in the same address space. It is much lighter than an OS thread: thousands or even millions of goroutines can be multiplexed onto a smaller pool of OS threads by the Go runtime. Goroutines grow and shrink their stacks dynamically, which keeps memory usage low.

## Answer 2 – How do you start a goroutine in Go?
You start a goroutine by prefixing a function call with the `go` keyword:
```go
go doWork()
```
or with an anonymous function:
```go
go func() {
    // concurrent work
}()
```
The call returns immediately, and the function body runs asynchronously in a separate goroutine.

## Answer 3 – What kinds of problems are goroutines well suited to solve?
Goroutines shine in tasks that involve:
- Handling many concurrent I/O operations (such as network connections).
- Breaking CPU‑bound work into independent pieces that can run in parallel.
- Modeling concurrent activities like background tasks, timers, or event processing.
They make it straightforward to express concurrent workflows that would be verbose or error‑prone using raw threads.

## Answer 4 – How do shared resources create issues in concurrent code?
When multiple goroutines read and write shared state without proper synchronization, you can get data races, inconsistent views of memory, and hard‑to‑reproduce bugs. For example, incrementing a shared counter or appending to a shared slice from multiple goroutines concurrently can corrupt data unless access is coordinated.

A minimal racy example:
```go
var counter int

func main() {
    for i := 0; i < 2; i++ {
        go func() {
            for j := 0; j < 1000; j++ {
                counter++ // unsynchronized write
            }
        }()
    }
    time.Sleep(100 * time.Millisecond)
    fmt.Println("counter:", counter)
}
```
Sometimes you’ll see `counter: 2000`, but other times a smaller number like `counter: 1734`. That’s because two goroutines are reading and writing `counter` at the same time, and their operations interleave in a way that occasionally “loses” increments.

You can picture the race like this:
```text
G1: load counter (42)
G2: load counter (42)
G1: increment -> 43
G1: store 43
G2: increment -> 43
G2: store 43   // one increment effectively lost
```
Without a mutex, atomic operation, or channel‑based coordination, there is nothing to prevent both goroutines from working with the same stale value, so the final result depends on timing rather than logic.

## Answer 5 – What tools does Go provide to protect shared resources?
Go offers:
- **Mutexes** to control access to critical sections:
  - `sync.Mutex`: a simple mutual‑exclusion lock; at most one goroutine can hold it at a time, for both reads and writes.
  - `sync.RWMutex`: a read–write lock; many goroutines can hold the read lock simultaneously (`RLock`) as long as no one holds the write lock (`Lock`), but only one writer can hold the write lock, and it blocks new readers.
- **Atomic operations** (`sync/atomic`) for lock‑free updates of individual variables like counters and flags.
These mechanisms help you enforce safe access to shared data and avoid races.

## Answer 6 – How can you synchronize goroutines so the main program waits for them?
Common techniques include:
- Using a `sync.WaitGroup` to track a set of goroutines and call `wg.Wait()` before exiting:
  ```go
  var wg sync.WaitGroup

  func worker(id int) {
      defer wg.Done()
      fmt.Println("worker", id, "done")
  }

  func main() {
      for i := 0; i < 3; i++ {
          wg.Add(1)
          go worker(i)
      }
      wg.Wait() // blocks until all workers have called Done
      fmt.Println("all workers finished")
  }
  ```

- Using channels to signal completion (each goroutine sends a value when done, and the receiver waits for all values):
  ```go
  func worker(id int, done chan<- struct{}) {
      fmt.Println("worker", id, "done")
      done <- struct{}{} // signal completion
  }

  func main() {
      const n = 3
      done := make(chan struct{}, n) // buffered so sends don't block forever

      for i := 0; i < n; i++ {
          go worker(i, done)
      }

      for i := 0; i < n; i++ {
          <-done // wait for each worker
      }
      fmt.Println("all workers finished")
  }
  ```

If the main function exits before goroutines finish, the program terminates, so explicit synchronization is essential.

## Answer 7 – How does Go’s scheduler impact goroutine behavior?
The Go runtime includes a scheduler that maps many goroutines onto a fixed number of OS threads. It preempts and reschedules goroutines as needed, particularly around blocking operations like system calls and channel operations. As a result, you focus on structuring concurrent work with goroutines and channels rather than manually creating and managing threads.

## Answer 8 – What are common concurrency patterns using goroutines?
Patterns include:
- **Fan‑out/fan‑in**: launching multiple worker goroutines to process tasks and collecting results through channels.
- **Worker pools**: a fixed set of goroutines pulling work from a shared task queue.
- **Pipelines**: chaining stages of computation where each stage is a goroutine connected by channels.
These patterns help organize complex concurrent logic into manageable, reusable components.

## Answer 9 – How do you detect and avoid data races in Go programs?
You can run your program with the race detector:
```bash
go run -race main.go
```
or
```bash
go test -race ./...
```
It reports race conditions at runtime. To avoid races, prefer communication over shared memory (using channels), minimize shared mutable state, and protect necessary shared variables with mutexes or atomics.

## Answer 10 – How should you think about structuring concurrent code in Go?
Good concurrent Go code:
- Uses goroutines to represent independent activities.
- Uses channels and synchronization primitives to coordinate and communicate.
- Keeps shared state limited and well‑encapsulated.
- Is designed so that each component has a clear responsibility and simple invariants.
This approach makes concurrent programs easier to reason about, test, and evolve.
