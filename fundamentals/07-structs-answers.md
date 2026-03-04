# Structs and Data Blueprints – Answers

## Answer 1 – What is a struct in Go and when would you use one?
A struct is a composite type that groups together fields, each with a name and type. It is used to model entities that have multiple related attributes, such as a user, product, or configuration. Structs are preferred over maps and loose collections of variables when you want compile‑time type checking, clearer structure, and better tooling support.

## Answer 2 – How do you declare and initialize structs?
You declare a struct type like this:
```go
type Person struct {
    Name string
    Age  int
}
```
You can then create values in several ways:
```go
p1 := Person{"Alice", 30}                 // positional
p2 := Person{Name: "Bob", Age: 25}        // named fields
var p3 Person                             // zero‑value fields
p4 := &Person{Name: "Carol"}              // pointer to struct
```
Named field initialization is often clearer and more robust to changes.

## Answer 3 – How does copying a struct work?
Assigning one struct variable to another copies all its fields by value:
```go
p1 := Person{Name: "Alice", Age: 30}
p2 := p1 // copy
```
Subsequent changes to `p2`’s fields do not affect `p1`. In contrast, copying a pointer (e.g., `p2 := &p1`) copies the address, and both pointers refer to the same underlying value.

## Answer 4 – How do you define methods on struct types?
You attach methods using a receiver:
```go
func (p Person) Greet() string {
    return "Hello, " + p.Name
}

func (p *Person) HaveBirthday() {
    p.Age++
}
```
Value receivers receive a copy of the struct and are suitable when you don’t need to mutate it. Pointer receivers are used when methods modify the struct or when you want to avoid copying large values.

## Answer 5 – How can structs be nested or composed?
Structs can contain other structs as fields:
```go
type Address struct {
    City  string
    State string
}

type Employee struct {
    Person
    Address
    ID string
}
```
This supports composition, allowing you to build rich models by combining simpler building blocks.

## Answer 6 – How do you compare structs for equality?
Two struct values of the same type can be compared with `==` if all their fields are comparable (for example, basic types and other comparable structs). If any field is not comparable (such as a slice or map), the struct itself cannot be compared directly. Equality is checked field by field.

## Answer 7 – What is the relationship between structs and JSON or database models?
Structs often serve as in‑memory representations of JSON objects or database rows. With JSON, struct field names and tags control how data is encoded and decoded. For databases, struct fields map to columns, making it convenient to marshal data to and from external systems in a type‑safe way.

## Answer 8 – How do you update fields on a struct?
You modify fields with dot notation:
```go
p := Person{Name: "Alice", Age: 30}
p.Age = 31
```
If you have a pointer to a struct, you can still use the same syntax (`p.Age`), and Go automatically dereferences the pointer. Methods and helper functions can encapsulate updates to keep invariants in one place.

## Answer 9 – How can structs work with interfaces?
Any struct type that implements all the methods of an interface implicitly satisfies that interface. There is no explicit `implements` declaration. This allows you to define behavior in terms of interfaces while backing it with concrete struct types tailored to specific use cases.

## Answer 10 – What design considerations apply when defining struct types?
Good practices include:
- Choosing clear, domain‑appropriate names.
- Using exported field names (capitalized) when you need access from other packages or from encoding/decoding libraries.
- Keeping structs focused on a single responsibility rather than gathering unrelated data.
- Avoiding overly deep or tangled nesting that makes models hard to understand.
