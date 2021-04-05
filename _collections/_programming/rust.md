---
title: Rust
---

# Rust

Do you like C++, but wish it's compiler would yell at you everytime you do something stupid? Have I got a language for you....

Rust is a systems programming language that excels at memory safety. It does this through strict rules enforced at compile time concerning the use of resources. For instance, every variable has "ownership." If you call a function that uses that variable as input, and doesn't "borrow" it, you won't be able to use it again after the function call.

Sounds kind of irritating? It can be when the compiler keeps yelling at you.

Thankfully though, the compiler has some of the best warning and error messages I've seen, and guides you towards correctness.

The net effect? Your code ends up getting designed "safer" from the ground up. 

Some programmers hate this idea of the compiler telling them what to do ("but in C I could just..."), and believe that enforcing strict rules like this will not necessarily eliminate bad code written by bad programmers. But you know what? It couldn't hurt.

## Cool Things Rust Has:

* Immutable data by default (must explicitly declare data will be altered)
* Traits. Essentially, interfaces. Structures are told what operations they support, rather than what classes they inherit from.
* Large standard library.
* Cargo, the package manager.
  * Built in testing utilities (god I love this).
  * Dependency management.
  * Linting.
  * Documentation generation.

## Less Cool Things Rust Has:

* Complexity
* Compiler that hates you (but secretly loves you)
* Kinda reads/writes like C++
