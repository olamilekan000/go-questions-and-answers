# Concurrency at Scale – Answers

## Answer 1 – How should errors be propagated in concurrent systems?
In concurrent systems, many goroutines may fail independently. Common patterns include:
- Giving each goroutine a shared **errors channel** where they send failures for aggregation.
- Using `errgroup.Group` (from `golang.org/x/sync/errgroup`) to run functions concurrently and capture the first error.
- Wrapping errors with enough context (operation, parameters) to make debugging possible.
The key is to avoid silently dropping errors and to centralize reporting so callers can make decisions based on combined outcomes.

## Answer 2 – How do you implement timeouts and cancellation across a tree of goroutines?
You derive a `Context` with a timeout or deadline at the top of the tree and pass it down to all functions and goroutines involved. Each goroutine selects on `ctx.Done()` and stops work when cancellation is signaled. This allows a single timeout or explicit cancel call to fan out and halt an entire sub‑tree of work.

## Answer 3 – What are heartbeats and why are they useful?
Heartbeats are **periodic signals** emitted by a process or goroutine to indicate that it is still alive and making progress. In Go they are often implemented with a `time.Ticker` and sent over a channel. Supervisors or monitoring components consume these signals; if they stop arriving within a window, they infer that the component is stuck or dead.

## Answer 4 – How do replicated requests improve reliability and latency?
Replicated requests involve sending the same logical request to multiple backends or instances and using the first successful response, cancelling the rest. This can dramatically improve tail latency in the presence of slow or flaky instances and increase reliability when some backends fail outright. The trade‑off is increased overall load and the need to carefully manage cancellation of the slower replicas.

## Answer 5 – How do you implement rate limiting in Go?
One idiom is a **token bucket** implemented with a ticker and a buffered channel:
- A goroutine uses a `time.Ticker` to periodically send tokens into a buffered channel up to capacity.
- Callers must receive a token from that channel before performing a rate‑limited action.
This constrains throughput to a configurable rate. The standard library’s `time.Ticker` and the `x/time/rate` package provide building blocks for such schemes.

## Answer 6 – How can you detect and heal unhealthy goroutines?
You can:
- Use heartbeats or progress messages to detect stalls.
- Set timeouts on operations; if a goroutine fails to respond in time, assume it is unhealthy.
- Encapsulate long‑lived goroutines behind supervisory loops that restart them if they exit unexpectedly or become unresponsive.
The goal is to avoid situations where a stuck goroutine silently holds resources or blocks progress indefinitely.

## Answer 7 – How do you design APIs that remain responsive under load?
Responsive APIs:
- Apply **backpressure** via bounded queues and limited worker counts so that queuing doesn’t grow without bound.
- Implement **load shedding** by rejecting or dropping excess requests once limits are reached instead of timing them out slowly.
- May prioritize certain types of work by giving them dedicated resources or higher quotas.
These techniques keep latency predictable and protect the system from cascading failures under overload.

## Answer 8 – How does context‑aware cancellation interact with external services?
When making outbound calls (HTTP, RPC, database), you:
- Attach the current context to each call so that timeouts and cancellations propagate.
- Ensure clients respect `Context`—for example, by using `http.NewRequestWithContext`, `db.QueryContext`, or gRPC methods that take a context.
This way, when your service times out a request, the work in downstream services is also cancelled instead of continuing unnecessarily.

## Answer 9 – How do you approach observability for concurrent systems?
You invest in:
- **Metrics**: counts of goroutines, queue lengths, error rates, latencies, and success rates.
- **Structured logs**: with request IDs, goroutine roles, and timing information.
- **Tracing**: distributed traces that follow requests across goroutines and services.
Together, these tools let you understand how concurrent components behave under real workloads and debug issues that only appear at scale.

## Answer 10 – What trade‑offs arise when scaling concurrency across machines versus within a single process?
Scaling within a process (more goroutines, more cores) is simpler in terms of communication and coordination but is limited by a single machine’s resources and failure domain. Scaling across machines adds network latency, partial failures, and distributed systems complexity, but it increases capacity, availability, and fault isolation. Go supports both modes well: goroutines and channels shine within a process, while context, RPC, and concurrency patterns help when coordinating across services.
