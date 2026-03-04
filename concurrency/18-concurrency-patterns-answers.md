# Concurrency Patterns – Answers

## Answer 1 – How does a worker pool pattern work in Go?

A worker pool consists of:

- A **jobs channel** carrying tasks to be processed.
- Multiple **worker goroutines** that read from the jobs channel, process tasks, and optionally send results.
- An optional **results channel** for outputs.
  The pool lets you cap concurrency (number of workers) while feeding an arbitrary number of tasks, improving resource utilization and controlling load.

A minimal sketch looks like this:

```go
jobs := make(chan int)    // tasks to do
results := make(chan int) // processed outputs

// start a few workers
for w := 0; w < 3; w++ {
    go func(id int) {
        for j := range jobs {
            results <- j * 2 // pretend processing
        }
    }(w)
}

// send jobs
go func() {
    for j := 1; j <= 5; j++ {
        jobs <- j
    }
    close(jobs) // signal no more work
}()

// collect results
for k := 0; k < 5; k++ {
    fmt.Println(<-results)
}
```

Here `jobs` is the input queue, each worker pulls from `jobs` and pushes to `results`, and the main goroutine controls how many workers exist, which is how you cap concurrency.

## Answer 2 – How would you implement a configurable worker pool?

One common structure is:

1. Create a `jobs` channel and a `results` channel.
2. Start `n` worker goroutines, each looping over `range jobs`, processing tasks, and sending to `results`.
3. In a separate goroutine, send all tasks into `jobs` then `close(jobs)` when done.
4. Use a `WaitGroup` inside the pool to `Wait` for all workers to finish, then close `results` so downstream consumers can stop.

Here is a full example:

```go
func worker(id int, jobs <-chan int, results chan<- int, wg *sync.WaitGroup) {
    defer wg.Done()
    for j := range jobs {
        results <- j * 2 // process the job
    }
}

func startPool(numWorkers int, jobs <-chan int) <-chan int {
    results := make(chan int)
    var wg sync.WaitGroup

    // Start workers
    for i := 0; i < numWorkers; i++ {
        wg.Add(1)
        go worker(i, jobs, results, &wg)
    }

    // Wait and close results in a background goroutine
    go func() {
        wg.Wait()
        close(results)
    }()

    return results
}
```

This pattern allows you to vary `n` based on CPU count or workload characteristics.

## Answer 3 – What is `sync.WaitGroup` and when do you use it?

`sync.WaitGroup` is a counter used to wait for a collection of goroutines to finish. The typical pattern is:

```go
var wg sync.WaitGroup
wg.Add(numGoroutines)
for i := 0; i < numGoroutines; i++ {
    go func() {
        defer wg.Done()
        // work
    }()
}
wg.Wait()
```

You use it when the main goroutine (or a coordinator) must block until all spawned goroutines have completed their work.

## Answer 4 – How can `WaitGroup` and channels be combined?

You can have workers call `wg.Done()` when they exit, and after `wg.Wait()` returns, close a results channel to signal completion:

```go
go func() {
    wg.Wait()
    close(results)
}()
```

Downstream code can then `range` over `results` until it is closed, ensuring no goroutines remain producing values after the channel is closed.

## Answer 5 – What problem does `sync.Pool` solve?

`sync.Pool` provides a concurrency‑safe pool of temporary objects to reduce allocation pressure and garbage collection overhead. It is useful when you frequently allocate short‑lived objects (like buffers) and want to reuse them across operations instead of constantly allocating and freeing.

## Answer 6 – What are the limitations and pitfalls of `sync.Pool`?

Important caveats:

- The runtime is free to discard pooled items at any time (for example, during GC), so you cannot depend on them staying cached.
- It is not a general cache or a mechanism for limiting resource use; it is purely an optimization.
- You must fully reinitialize objects obtained from a pool before reuse to avoid data leakage between requests.
  Because of these constraints, `sync.Pool` should only be used when profiling shows allocations as a bottleneck.

## Answer 7 – How do you design backpressure in concurrent pipelines?

Backpressure can be achieved by:

- Using **unbuffered channels** so producers block until consumers are ready.
- Using **bounded buffered channels** so producers block when buffers fill up.
- Limiting the number of worker goroutines so work cannot grow unbounded.
  These mechanisms propagate load constraints upstream, preventing operations from overwhelming memory or external dependencies.

## Answer 8 – How can you shut down a worker pool gracefully?

Two common techniques are:

- **Close the jobs channel**: workers loop over `range jobs` and exit when the channel is closed; a `WaitGroup` can then detect when all workers are done and close results channels.
- **Use a cancellation context**: workers select on `ctx.Done()` alongside the jobs channel, exiting promptly when the context is cancelled or times out.
  Both approaches ensure workers finish in‑flight work (or stop early if required) and that all goroutines eventually terminate.

## Answer 9 – How do you choose between mutexes, channels, and other synchronization tools?

- Use a **mutex** (`sync.Mutex` or `sync.RWMutex`) when you have a well‑defined critical section around shared data and want minimal overhead.
- Use **channels** when modeling hand‑off, queues, or pipelines where communication is more natural than shared state.
- Use **atomic operations** for simple counters or flags that need lock‑free updates.
- Use **`sync.Map`** for highly concurrent, write‑rare/read‑often maps where the contention of a mutex‑protected map would be a bottleneck.
  The choice depends on whether you are primarily synchronizing access to state or orchestrating the flow of messages.

## Answer 10 – How do you test and debug concurrent Go code?

Effective techniques include:

- Running with the race detector (`go test -race` or `go run -race`) to catch data races.
- Adding timeouts to tests that use channels or goroutines to avoid hangs.
- Designing components with clear boundaries so you can test them with deterministic inputs (for example, using fake channels or clocks).
- Using logging or tracing sparingly to understand scheduling‑dependent behavior.
  These practices help surface concurrency bugs that might otherwise appear only under rare timing conditions.
