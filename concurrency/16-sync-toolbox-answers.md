# The Sync Toolbox – Answers

## Answer 1 – What problems does the `sync` package address in Go?
The `sync` package provides low‑level primitives for coordinating access to shared memory and managing goroutines. It is used when you need mutual exclusion, condition signaling, one‑time initialization, or pooling of objects, and when modeling these concerns with channels alone would be awkward or inefficient.

## Answer 2 – How does `sync.WaitGroup` coordinate goroutines?
`WaitGroup` maintains a counter of active goroutines:
- You call `wg.Add(n)` before starting `n` goroutines.
- Each goroutine calls `defer wg.Done()` when it starts work.
- The caller invokes `wg.Wait()` to block until the counter returns to zero.
This pattern ensures the caller knows when all goroutines have finished without needing separate channels for each.

## Answer 3 – When would you use `sync.Mutex` versus `sync.RWMutex`?
Use `sync.Mutex` when you need simple exclusive access to a critical section. Use `sync.RWMutex` when you have many readers and relatively few writers; it allows multiple readers to hold the lock concurrently but only one writer at a time. However, `RWMutex` is more complex and can be slower under contention, so it’s not always an automatic win.

## Answer 4 – What is `sync.Cond` and how is it used?
`sync.Cond` is a condition variable that lets goroutines wait for or signal changes in shared state. A typical pattern is:
1. Guard shared state with a mutex.
2. Goroutines call `cond.Wait()` while a predicate on the shared state is false.
3. When another goroutine changes the shared state, it calls `cond.Signal()` or `cond.Broadcast()` to wake waiting goroutines.
This is useful for implementing queues, pools, or other coordination patterns where goroutines must block until a condition holds.

## Answer 5 – How does `sync.Once` help with one‑time initialization?
`sync.Once` guarantees that a function is executed at most once, even when called concurrently from multiple goroutines:
```go
var once sync.Once

func initConfig() {
    once.Do(func() {
        // perform initialization
    })
}
```
Subsequent calls to `initConfig` will return immediately, ensuring that initialization logic is run exactly once and is concurrency‑safe.

## Answer 6 – What is `sync.Pool` designed for?
`sync.Pool` is designed as a cache of temporary objects that may be reused to reduce allocation and garbage collection overhead. It is most effective for short‑lived, frequently allocated objects like buffers. The runtime is free to drop items from the pool at any time, so it cannot be used as a reliable cache or for correctness.

## Answer 7 – How do you ensure correct lock usage with `Mutex`?
Common best practices include:
- Always pair a `Lock` with a `defer Unlock()` immediately after acquiring the lock.
- Keep critical sections as small as possible to reduce contention.
- Establish a consistent lock acquisition order across your codebase to avoid deadlocks when multiple locks are involved.
- Avoid calling into untrusted or long‑running code while holding a lock.

## Answer 8 – How do `atomic` operations relate to the `sync` toolbox?
The `sync/atomic` package provides low‑level, lock‑free operations on numeric values and pointers. It can be preferable to a mutex for simple counters, flags, or pointer swaps where you only need to update a single value atomically. However, atomics are easy to misuse and harder to reason about, so they are usually reserved for performance‑critical, well‑encapsulated code.

## Answer 9 – How can you combine `sync` primitives with channels effectively?
For example, you might have:
- A shared in‑memory cache protected by a `Mutex` or `RWMutex`.
- A background goroutine that listens on a channel for invalidation or update messages and then updates the cache under the lock.
Here, channels handle communication and signaling, while locks protect the internal state of the cache. This separation keeps each concern simple.

## Answer 10 – What are signs that you’re overusing locks in a Go program?
Red flags include:
- Deeply nested locking logic that is hard to follow.
- Frequent deadlocks or timeouts around critical sections.
- Locks that are held for long periods doing I/O or heavy computation.
- Multiple locks guarding related state, leading to complex acquisition order rules.
In such cases, it may be better to restructure designs to use channels, confinement, or simpler data flows to reduce the need for fine‑grained locking.

