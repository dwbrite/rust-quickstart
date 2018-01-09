#### Style
snake_case for var and function names.

#### Stack
Where fixed length data is stored, for example, string literals and primitive types. It is a Last-In-First-Out (LIFO) collection, meaning access is significantly faster.

#### Heap
A collection of data of variable length. Data must be allocated here. When data is allocated, a pointer, length, and capacity is stored in the stack.

#### Integer Literals
|                |                |
|:---            |:---            |
| Decimal        | `98_222`       |
| Hex            | `0xff`         |
| Octal          | `0o77`         |
| Binary         | `0b1111_0000`  |
| Byte (u8 only) | `b'A'`         |


---


## Cargo
Rust's package manager.

#### Crates
Libraries, basically. IMO they're *unusually* small compared to other languages -- Perhaps due to the ease of use of crates and dependencies. I really like this.

#### crates.io
An online collection of crates.

---


#### Expressions
You can run expressions inside a block. You can even include statements in the block, so long as the end of the block actually returns a value. Like so:

```rust
let y = {
    let x = 3;
    x + 1
};
```

#### Range
A range is an expression of numbers. It uses the `x..y` syntax which represents a slice of values from x to y. It's often used


---



#### Comments
Single-line comments with `// Comment`,
Multi-line comments with `/* Comment ... */`


#### Documentation
`/// Comment` for normal documentation
`//! Comment` for "module" documentation
`/** ... */`  for multi-line documentation
markdown is used for documentation comments. You can even use three backticks for writing code in documentation, like so:

````rust
///```
///fn func() {
///    ...
///}
///```
````

---

#### Expressions
In Rust, you can write a standalone expression without a semicolon. If you write a standalone expression at the end of a block (be it a function or other), it will return the result of that expression. Be aware that the `return` statement still exists.

---

#### Namespaces
A namespace is a uh... space... which has a name to refer to... and which will have stuff in it... You know what, you should know about namespaces already! I shouldn't have to tell you this!

---

## Vectors, Strings, and HashMaps, oh my!
TL;DR: push = "append"
#### Vectors

```rust
let v: Vec<i32> = Vec::new(); //or...
let v = vec![1, 2, 3]; //macros are lit
```

```rust
let mut v2 = Vec::new();
v2.push(5); //push a value into the vector
```

```rust
let v = vec![1, 2, 3, 4, 5]; // There's multiple ways to grab a value from a vec

let does_not_exist = &v[100]; // This is the built-in way, this will cause panics why you mess with it
let does_not_exist = v.get(100); // This method will return None when you mess with it
```

#### Strings
push_str for strings, push for chars

```rust
let mut s1 = String::from("foo");
let s2 = String::from("bar");
s1.push_str(&s2);
```
P.S.: `"Yeah "+"you "+"can ;)"`

```rust
let s1 = String::from("tic");
let s2 = String::from("tac");
let s3 = String::from("toe");

let s = s1 + "-" + &s2 + "-" + &s3; // or...
let s = format!("{}-{}-{}", s1, s2, s3);
```

#### HashMaps
This is all you really need to know:

```rust
use std::collections::HashMap;

let mut scores = HashMap::new();

scores.insert(String::from("Blue"), 10);
scores.insert(String::from("Blue"), 25);

println!("{:?}", scores);
```

---
