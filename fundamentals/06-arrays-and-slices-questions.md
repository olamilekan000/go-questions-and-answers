# Arrays and Slices – Questions

## Question 1 – How do arrays differ from slices in Go?
Compare arrays and slices in terms of size, mutability, and typical usage.

## Question 2 – How do you declare and initialize arrays?
Show different ways to declare arrays, including specifying length, using array literals, and working with multidimensional arrays.

## Question 3 – What are the key properties of slices?
Explain what a slice is, how it relates to an underlying array, and what its length and capacity represent.

## Question 4 – How do you create slices in different ways?
Describe how to create empty slices, slices with `make`, and slices derived from existing arrays or other slices.

## Question 5 – How does appending to a slice work under the hood?
Explain what happens when you use `append` on a slice, and what it means when capacity is exceeded.

## Question 6 – How do you slice and range over arrays and slices?
Show how to take sub‑slices using slice expressions and how to iterate over elements with `for ... range`.

## Question 7 – How can you copy slices safely?
Describe the `copy` function and how it can be used to duplicate slice data or copy between slices of different lengths.

## Question 8 – How do you insert an item into a slice at an arbitrary position?
Outline a strategy for inserting a new element into the middle of a slice while preserving order.

## Question 9 – How do you remove an item from a slice?
Explain a common idiom for removing an element from a slice by index while keeping the remaining elements in order.

## Question 10 – What are typical pitfalls when working with slices?
Discuss common mistakes around shared backing arrays, unexpected mutations, and capacity assumptions.

