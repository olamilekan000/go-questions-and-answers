# Channels and Communication – Questions

## Question 1 – What is a channel in Go and why is it important?
Explain how channels support communication between goroutines and the “share memory by communicating” philosophy.

## Question 2 – How do you create and use channels?
Show how to declare, initialize, send on, and receive from a channel.

## Question 3 – What is the difference between unbuffered and buffered channels?
Describe how buffering changes send/receive behavior and when you might choose each kind.

## Question 4 – How do you close a channel and what does closing mean?
Explain what `close(ch)` does, how receivers detect closure, and what operations are forbidden after closing.

## Question 5 – How do you iterate over values coming from a channel?
Describe how to use `for ... range` with channels and when that pattern is appropriate.

## Question 6 – How can `select` be used with channels?
Explain the `select` statement, its role in waiting on multiple channel operations, and how to use a `default` case.

## Question 7 – How do you avoid goroutine leaks with channels?
Discuss patterns for ensuring goroutines terminate properly when channels are no longer used or when work is cancelled.

## Question 8 – How can you fan‑out work and fan‑in results with channels?
Outline a pattern where multiple worker goroutines process tasks from an input channel and send results to an output channel.

## Question 9 – How do you use buffered channels for asynchronous work?
Describe scenarios where a buffered channel helps decouple producers and consumers and what trade‑offs it introduces.

## Question 10 – What are best practices for designing channel‑based APIs?
Summarize guidelines for naming, ownership, closing responsibility, and error signaling when channels appear in function signatures.

