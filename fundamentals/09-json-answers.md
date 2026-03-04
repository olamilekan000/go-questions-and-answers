# Encoding and Decoding JSON – Answers

## Answer 1 – What role does JSON play in modern Go applications?
JSON is the de facto standard for exchanging data between services, browsers, and back‑end APIs. Go programs frequently consume JSON from web APIs, configuration files, and message queues, and they often produce JSON responses in HTTP handlers. Strong JSON support in the standard library makes it straightforward to integrate Go services into modern microservice and web architectures.

## Answer 2 – How is JSON structured conceptually?
JSON is built from a small set of types:
- **Object**: an unordered set of key‑value pairs, typically mapped to Go structs or `map[string]T`.
- **Array**: an ordered list of values, mapped to Go slices or arrays.
- **String**: text values, mapped to Go `string`.
- **Number**: numeric values, commonly mapped to `float64` or specific integer/float types.
- **Boolean**: `true` or `false`, mapped to Go `bool`.
- **null**: an explicit absence of a value, which can map to `nil` pointers or omitted fields.

## Answer 3 – How do you decode JSON into a struct?
The usual flow is:
1. Define a Go struct type whose fields correspond to the JSON attributes.
2. Add JSON struct tags to control field names and behavior if needed.
3. Read JSON bytes from a source (file, HTTP response, etc.).
4. Call `json.Unmarshal(data, &dst)` where `dst` is a pointer to your struct.
The JSON library populates the struct fields that match the JSON keys, handling type conversions where possible.

## Answer 4 – How do you decode JSON into slices or arrays?
When the JSON root or a field is an array, you use a slice or array type as the target:
```go
var items []Item
if err := json.Unmarshal(data, &items); err != nil {
    // handle error
}
```
Nested arrays or embedded objects are modeled as slices or structs within your top‑level types, and the decoder recurses through the structure automatically.

## Answer 5 – How do you work with embedded objects in JSON?
You represent nested JSON objects with nested structs:
```go
type Address struct {
    City string `json:"city"`
}

type User struct {
    Name    string  `json:"name"`
    Address Address `json:"address"`
}
```
When you unmarshal into `User`, the `address` object from JSON is decoded into the `Address` field. Embedded or anonymous structs can also be used to flatten some hierarchies, depending on tagging.

## Answer 6 – What are struct tags and why are they important for JSON?
Struct tags are annotations that sit after field declarations, for example:
```go
Name string `json:"full_name,omitempty"`
```
They tell the `encoding/json` package which JSON key to map to a field, whether to omit empty values (`omitempty`), and how to handle special cases like `-` (never encode/decode this field). Tags are essential when JSON field names don’t match Go naming conventions or when you need to customize serialization.

## Answer 7 – How can you decode unstructured or partially known JSON?
For highly dynamic or unknown schemas, you can decode into:
- `map[string]interface{}` for JSON objects.
- `[]interface{}` for JSON arrays.
You then perform type assertions on the resulting values. This approach trades compile‑time safety for flexibility and is useful when dealing with arbitrary payloads or when you only care about a subset of the fields.

## Answer 8 – How do you encode Go values back into JSON?
To go from Go values to JSON, you use `json.Marshal`:
```go
data, err := json.Marshal(v)
```
or `json.MarshalIndent` for pretty‑printed output. The encoder respects struct tags, exported field names, and zero values, producing a JSON representation compatible with most clients.

## Answer 9 – What are common pitfalls when working with JSON and Go types?
Typical issues include:
- Mapping JSON numbers to overly specific Go numeric types and encountering overflow or parsing errors.
- Forgetting to export struct fields (lowercase names), which prevents them from being encoded/decoded.
- Misusing `interface{}` and failing type assertions at runtime.
- Ignoring errors from `json.Unmarshal` or `json.Marshal`, which can hide malformed data problems.

## Answer 10 – How can you design your Go types for future JSON changes?
To make your code resilient:
- Prefer structs for well‑understood, stable parts of the schema.
- Use `omitempty` for optional fields and pointers or `*T` when you need to distinguish “unset” from a zero value.
- Reserve some flexible fields (such as a `map[string]interface{}` for extras) when you expect evolving payloads.
- Write decoding logic that ignores unknown fields rather than failing on them, which Go’s JSON package does by default.
