# Encoding and Decoding JSON – Questions

## Question 1 – What role does JSON play in modern Go applications?
Explain why JSON is important for Go programs and where you are most likely to encounter it.

## Question 2 – How is JSON structured conceptually?
Describe the core JSON types (object, array, string, number, boolean, null) and how they map to Go types.

## Question 3 – How do you decode JSON into a struct?
Outline the steps to take a JSON document and unmarshal it into a strongly typed Go struct.

## Question 4 – How do you decode JSON into slices or arrays?
Explain how to handle JSON arrays and map them into Go slices, including nested or embedded structures.

## Question 5 – How do you work with embedded objects in JSON?
Describe how to model nested JSON objects using Go structs and how decoding handles those relationships.

## Question 6 – What are struct tags and why are they important for JSON?
Explain how struct tags control JSON field names, optional fields, and omitting empty values.

## Question 7 – How can you decode unstructured or partially known JSON?
Discuss strategies for handling JSON with unknown fields, such as using `map[string]interface{}` or `interface{}`.

## Question 8 – How do you encode Go values back into JSON?
Describe how to marshal Go structs, slices, and interfaces into JSON and control the output format.

## Question 9 – What are common pitfalls when working with JSON and Go types?
Identify typical issues such as numeric type mismatches, missing fields, and pointer versus value considerations.

## Question 10 – How can you design your Go types for future JSON changes?
Explain how to structure your types and tags so your code is resilient to evolving JSON schemas.

