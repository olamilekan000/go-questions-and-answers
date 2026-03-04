# Time and Dates – Answers

## Answer 1 – How does Go represent points in time?
Go uses the `time.Time` type to represent an instant. A `Time` value stores a date, clock time, and location information (time zone), and internally also tracks a monotonic component for measuring elapsed time. This allows it to represent both calendar concepts (year, month, day) and precise instants suitable for comparisons and arithmetic.

## Answer 2 – How do you obtain the current time and basic components?
You call:
```go
now := time.Now()
year, month, day := now.Date()
hour, min, sec := now.Clock()
loc := now.Location()
```
`time.Now` returns the current local time, and methods like `Year()`, `Month()`, `Day()`, and `Hour()` let you extract individual components.

## Answer 3 – What is a `time.Duration` and how is it used?
`time.Duration` is a signed integer count of nanoseconds and represents a length of time. You can construct durations using constants such as `time.Second`, `time.Millisecond`, and `time.Hour`, for example:
```go
d := 5 * time.Second
```
Durations are used for sleeping (`time.Sleep(d)`), timeouts, and adding or subtracting from `time.Time` values (`t.Add(d)` or `t.Sub(u)`).

## Answer 4 – How do you add and subtract time values?
To shift a time by a duration:
```go
later := t.Add(2 * time.Hour)
earlier := t.Add(-30 * time.Minute)
```
To compute elapsed time between two instants:
```go
elapsed := t2.Sub(t1) // returns a time.Duration
```
`time.Since(t)` is a convenience wrapper for `time.Now().Sub(t)`.

## Answer 5 – How do you format and parse time values?
Go’s `time` package uses a layout string based on a reference time:
`Mon Jan 2 15:04:05 MST 2006`. You format like:
```go
formatted := t.Format("2006-01-02 15:04:05")
```
and parse with:
```go
parsed, err := time.Parse("2006-01-02 15:04:05", s)
```
The digits and fields in the layout string specify how the corresponding parts of the time should appear.

## Answer 6 – How are time zones and locations handled?
Each `time.Time` has an associated `*time.Location`. You can convert between locations with:
```go
utc := t.UTC()
local := t.Local()
loc, _ := time.LoadLocation("America/New_York")
nyTime := t.In(loc)
```
Using UTC for storage and internal logic, and converting to local time only at the edges (I/O, display) is a common best practice.

## Answer 7 – How do you work with timers and tickers?
Timers fire once after a delay:
```go
timer := time.NewTimer(5 * time.Second)
<-timer.C // wait for the timer
```
Tickers deliver events at a regular interval:
```go
ticker := time.NewTicker(1 * time.Second)
defer ticker.Stop()
for range ticker.C {
    // periodic work
}
```
`time.After(d)` is a convenience that returns a channel which receives a single time value after `d`.

## Answer 8 – How can you implement timeouts for operations?
A common pattern is:
```go
select {
case result := <-ch:
    // use result
case <-time.After(2 * time.Second):
    // timeout
}
```
Or, with `context.Context`, deriving a context with a timeout and selecting on `ctx.Done()` in goroutines. Both approaches limit how long an operation is allowed to block.

## Answer 9 – What are common pitfalls with time comparisons and equality?
Pitfalls include:
- Comparing times in different locations without normalizing (e.g., not converting both to UTC).
- Ignoring the monotonic component when using `Equal` vs `==`.
- Comparing truncated times (such as seconds only) and assuming exact nanosecond equality.
Using `t1.Equal(t2)` and normalizing to a common location, or truncating both times to the same precision before comparison, helps avoid subtle bugs.

## Answer 10 – How should you design time‑related APIs for testability?
Rather than calling `time.Now()` directly everywhere, you can:
- Inject a `Clock` interface with a `Now()` method into components.
- Pass `time.Time` or `time.Duration` as parameters from higher‑level code.
This makes it easy to write tests that control “current time” and verify behavior around boundaries like timeouts and expirations without relying on real wall‑clock time.
