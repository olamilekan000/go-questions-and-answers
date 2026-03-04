# Arrays and Slices – Answers

## Answer 1 – How do arrays differ from slices in Go?
Arrays have a fixed length that is part of their type (e.g., `[3]int`), so they cannot be resized. Slices are lightweight views over arrays that track a length and capacity and can grow or shrink logically. Arrays are useful when size is known and constant; slices are preferred for most dynamic collections.

## Answer 2 – How do you declare and initialize arrays?
You can declare arrays in several ways:
```go
var a [3]int              // zero‑initialized
b := [3]int{1, 2, 3}      // array literal
c := [...]int{1, 2, 3, 4} // length inferred
var m [2][3]int           // multidimensional array
```
Multidimensional arrays are arrays of arrays and can be indexed with multiple indices like `m[i][j]`.

## Answer 3 – What are the key properties of slices?
A slice describes a contiguous segment of an underlying array. It has:
- A **length** (`len(s)`), the number of elements it currently exposes.
- A **capacity** (`cap(s)`), the maximum length it can grow to before requiring a new array.
Multiple slices can share the same backing array, which means changes through one slice may be visible through another.

For example:
```go
arr := [5]int{10, 20, 30, 40, 50}

s1 := arr[1:4] // elements at indices 1,2,3 -> {20, 30, 40}
s2 := arr[2:]  // elements at indices 2,3,4 -> {30, 40, 50}

fmt.Println(len(s1), cap(s1)) // 3 4  (from index 1 to end of arr)
fmt.Println(len(s2), cap(s2)) // 3 3  (from index 2 to end of arr)

// Modify through s1:
s1[1] = 999 // changes element at arr[2]

fmt.Println(arr) // [10 20 999 40 50]
fmt.Println(s1)  // [20 999 40]
fmt.Println(s2)  // [999 40 50]
```
Here both `s1` and `s2` share the same backing array `arr`. Updating `s1[1]` updates `arr[2]`, which is also visible as the first element of `s2`. The `len` and `cap` values show how far each slice can see into that shared array.

## Answer 4 – How do you create slices in different ways?
Common patterns include:
```go
var s []int                // nil slice
s = []int{1, 2, 3}         // slice literal
t := make([]int, 5)        // length 5, capacity 5
u := make([]int, 0, 10)    // length 0, capacity 10
v := a[1:4]                // slice referencing part of array a
```
These approaches let you control initial length and capacity or derive slices from existing arrays.

## Answer 5 – How does appending to a slice work under the hood?
When you call `append`, Go tries to put new elements into the existing backing array if there is capacity. If `len(s) < cap(s)`, it extends the length and adds elements. If capacity is insufficient, Go allocates a new, larger array, copies the existing elements, and returns a new slice referring to that new array. As a result, the returned slice may not share a backing array with the original.

For example:
```go
// start with capacity to grow
s := make([]int, 0, 4)
fmt.Printf("s: len=%d cap=%d ptr=%p\n", len(s), cap(s), s)

s = append(s, 1, 2)
fmt.Printf("s: len=%d cap=%d ptr=%p\n", len(s), cap(s), s)

// append without exceeding capacity: same backing array
s = append(s, 3)
fmt.Printf("s: len=%d cap=%d ptr=%p\n", len(s), cap(s), s)

// append beyond capacity: Go allocates a new backing array
s = append(s, 4, 5, 6)
fmt.Printf("s: len=%d cap=%d ptr=%p\n", len(s), cap(s), s)
```

Sample output (addresses will differ on your machine):
```text
s: len=0 cap=4 ptr=0x11aab83b4000
s: len=2 cap=4 ptr=0x11aab83b4000
s: len=3 cap=4 ptr=0x11aab83b4000
s: len=6 cap=8 ptr=0x11aab83b6040
```
Notice how the pointer (`ptr`) stays the same while `len` grows from 0 → 2 → 3, but once we append past capacity (from length 3 with capacity 4 and add three more elements), the capacity grows and the pointer changes. That indicates `append` moved the slice to a new backing array.

You can picture this roughly as:

```text
// Before growing past capacity:

underlying array (cap = 4)
      index:   0    1    2    3
              +----+----+----+----+
              |  1 |  2 |  3 |  ? |
              +----+----+----+----+
slice s
  len = 3
  cap = 4
  ptr ────────────────┘  (points at element 0 of this array)


// After append pushes len past old cap:

old array (no longer used by s)
      index:   0    1    2    3
              +----+----+----+----+
              |  1 |  2 |  3 |  ? |
              +----+----+----+----+

new underlying array (cap = 8, contents copied)
      index:   0    1    2    3    4    5    6    7
              +----+----+----+----+----+----+----+----+
              |  1 |  2 |  3 |  4 |  5 |  6 |  ? |  ? |
              +----+----+----+----+----+----+----+----+
slice s
  len = 6
  cap = 8
  ptr ──────────────────────────┘  (now points at element 0 of the new array)

```

The important points are:
- As long as `len <= cap`, `append` reuses the same backing array (pointer unchanged).
- When `len` would exceed `cap`, Go allocates a bigger array, copies elements over, and makes the slice point to this new array.

## Answer 6 – How do you slice and range over arrays and slices?
You can create sub‑slices with expressions like:
```go
sub := s[1:4]  // from index 1 up to, but not including, 4
head := s[:3]  // from start to index 3
tail := s[2:]  // from index 2 to the end
```
To iterate:
```go
for i, v := range s {
    fmt.Println(i, v)
}
```
This yields the index and value for each element.

## Answer 7 – How can you copy slices safely?
The built‑in `copy` function copies elements from a source slice to a destination slice:
```go
dst := make([]int, len(src))
n := copy(dst, src)
```
It returns the number of elements copied, which is the minimum of `len(dst)` and `len(src)`. Using `copy` avoids sharing the same backing array when you need independent slices.

For example:
```go
src := []int{1, 2, 3}

// Shared backing array:
alias := src
alias[0] = 999
fmt.Println("src  :", src)   // [999 2 3]
fmt.Println("alias:", alias) // [999 2 3]

// Independent copy:
dst := make([]int, len(src))
copy(dst, src)
dst[0] = 42
fmt.Println("src  :", src)   // [999 2 3]  (unchanged)
fmt.Println("dst  :", dst)   // [42  2 3]  (separate slice)
```
Assigning `alias := src` makes both slices point at the same backing array, so changes through one are visible through the other. Using `copy` populates `dst` with the same values but a different backing array, so subsequent changes to `dst` do not affect `src`.

## Answer 8 – How do you insert an item into a slice at an arbitrary position?
One idiom is:
```go
idx := 2
value := 99
s = append(s, 0)             // grow by one
copy(s[idx+1:], s[idx:])     // shift elements to the right
s[idx] = value
```
This preserves order by making room at `idx` and then assigning the new element.

## Answer 9 – How do you remove an item from a slice?
To remove the element at index `idx` while preserving order:
```go
idx := 2
s = append(s[:idx], s[idx+1:]...)
```
This concatenates the elements before and after the removed element into a new slice that may share part of the original backing array.

## Answer 10 – What are typical pitfalls when working with slices?
Common pitfalls include:
- **Unexpected sharing**: Two slices derived from the same array can affect each other’s data.
- **Capacity surprises**: Appending can reallocate and break sharing assumptions or, conversely, accidentally overwrite data you thought was independent.
- **Using `len` instead of `cap`**: Misunderstanding the difference can lead to indexing errors or inefficient growth strategies.
Being aware of length, capacity, and sharing behavior helps avoid subtle bugs.
