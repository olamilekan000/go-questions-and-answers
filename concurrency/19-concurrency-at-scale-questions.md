# Concurrency at Scale – Questions

## Question 1 – How should errors be propagated in concurrent systems?
Describe patterns for aggregating and reporting errors that occur in multiple goroutines.

## Question 2 – How do you implement timeouts and cancellation across a tree of goroutines?
Explain how to design a system where a single timeout or cancellation request cleanly stops all related work.

## Question 3 – What are heartbeats and why are they useful?
Define a heartbeat in the context of concurrent systems and describe how they can be used to monitor liveness.

## Question 4 – How do replicated requests improve reliability and latency?
Explain the idea of sending the same request to multiple backends and how to balance improved tail latency against increased load.

## Question 5 – How do you implement rate limiting in Go?
Describe token‑bucket or leaky‑bucket style rate limiting patterns and how to express them with goroutines and channels.

## Question 6 – How can you detect and heal unhealthy goroutines?
Discuss indicators that a goroutine is stuck or misbehaving and patterns for restarting or replacing it.

## Question 7 – How do you design APIs that remain responsive under load?
Explain backpressure, shedding load, and prioritization in the context of concurrent services.

## Question 8 – How does context‑aware cancellation interact with external services?
Describe how you ensure that timeouts and cancellations in your service propagate to downstream calls.

## Question 9 – How do you approach observability for concurrent systems?
Outline what you log or measure (traces, metrics, structured logs) to understand behavior of goroutines and channels in production.

## Question 10 – What trade‑offs arise when scaling concurrency across machines versus within a single process?
Compare vertical scaling (more goroutines, more cores) with horizontal scaling (more processes/instances) and how Go fits into both.

