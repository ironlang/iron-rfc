# RFC 002: Introduce the `async` keyword and `Future<T>`

Modern programming languages make it easy to write safe, concurrent code.

## Motivation

## Analysis of Prior Art

### Go: Goroutines

https://golangbot.com/goroutines/

### Swift: Completions + Concurrency Roadmap

https://forums.swift.org/t/swift-concurrency-roadmap/41611

## Proposal

### Introduce the `Future<T>` type to the standard library

Any function 

### Convert Iron expressions to `Future` with the `async` keyword 

```iron
// A blocking call to `io.print(line:)`
//
// The result of this function call is the "empty" type `()`.
io.print(line: "Hello, world!")
```

```iron
// A non-blocking call to io.print(line:)`
//
// The result of this function call is the `Future<T>` type, where `T` is the empty type `()`.
async io.print(line: "Hello, world!")
```
