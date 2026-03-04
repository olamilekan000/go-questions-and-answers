# Time and Dates – Questions

## Question 1 – How does Go represent points in time?
Explain the `time.Time` type and what information it stores about a moment.

## Question 2 – How do you obtain the current time and basic components?
Show how to get “now” and extract fields such as year, month, day, hour, and location.

## Question 3 – What is a `time.Duration` and how is it used?
Describe how durations are represented, how you construct them, and how they relate to operations like sleeping or time differences.

## Question 4 – How do you add and subtract time values?
Explain how to add durations to a `time.Time` and how to compute the elapsed time between two instants.

## Question 5 – How do you format and parse time values?
Describe Go’s layout‑based approach to formatting and parsing times, including the special reference time.

## Question 6 – How are time zones and locations handled?
Explain how `Location` works, how to convert between UTC and local time, and how to load or use specific time zones.

## Question 7 – How do you work with timers and tickers?
Describe how to use `time.After`, `time.NewTimer`, and `time.NewTicker` in concurrent programs.

## Question 8 – How can you implement timeouts for operations?
Explain patterns for combining `time.After` or `Context` with channels or blocking calls to enforce time limits.

## Question 9 – What are common pitfalls with time comparisons and equality?
Discuss issues such as location differences, truncation, and monotonic vs. wall‑clock time when comparing `time.Time` values.

## Question 10 – How should you design time‑related APIs for testability?
Outline strategies like injecting a clock interface or using configurable “now” sources to make time‑dependent code easier to test.

