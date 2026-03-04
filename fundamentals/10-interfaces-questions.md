# Interfaces and Method Signatures – Questions

## Question 1 – What is an interface in Go and what problem does it solve?
Explain how interfaces describe behavior and why they are central to abstraction in Go.

## Question 2 – How do you declare an interface and implement it?
Show the syntax for defining an interface type and how a concrete type comes to satisfy it.

## Question 3 – What does it mean that interface satisfaction is implicit?
Describe how Go’s approach differs from languages that require explicit “implements” declarations.

## Question 4 – How can interfaces be used to decouple code?
Explain how accepting interfaces instead of concrete types in function signatures improves flexibility and testability.

## Question 5 – What is the `Stringer` interface and why is it useful?
Describe the purpose of the `fmt.Stringer` interface and how implementing it affects printing.

## Question 6 – How can you determine at runtime whether a value implements an interface?
Explain how to use type assertions and type switches with interfaces.

## Question 7 – What does it mean to implement multiple interfaces?
Discuss how a single type can satisfy more than one interface and why this is powerful.

## Question 8 – What is the empty interface and when is it appropriate?
Describe `interface{}` and the kinds of scenarios where you might use it in real code.

## Question 9 – How do you add methods to a type that doesn’t originally satisfy an interface?
Explain how you can wrap or adapt types so they conform to a desired interface.

## Question 10 – What are good design practices around interfaces in Go?
Outline guidelines for choosing interface granularity, naming, and where to define them.

