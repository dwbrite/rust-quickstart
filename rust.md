# Rust Quickstart

Rust is great language, if a bit unorthodox. The coolest part is that in Safe Rust, you will never encounter undefined behaviour.

This document explains the core concepts of Rust, but puts the responsibility of intuitive understanding on you. For example, you can read about "borrowing" and understand the concept, but you may not *intuitively understand it* until you've worked with it in Rust for a while. Good luck.

I'm creating this file to document how *I* learned Rust, from a fairly advanced programmer's perspective. I hope that it can be of use to you, whomever you are, for learning Rust without having to wade through the original books.

This book also assumes that you've started with Cargo and already have a test project set up. If you haven't done that, check out the chapters on Cargo in the official Rust books.

## Variables

#### let
Use `let x = ...` or `let x: type =...` to declare a variable.

#### mut
Variables **must** be declared `mut`able if you want to modify them at any point.

#### const
Not to be confused with not including `mut`, a constant cannot be shadowed.

#### shadowing
You can "shadow" over another variable of the same name. This is useful in converting Strings to ints without making a second variable.

```rust
let x: i32 = 5;
let x: String = x.to_string();
```
---

## Functions
Parameters are written `var_name: type`, one after the other.
Used like:

```rust
fn add_and_print(x: i32, y: i32) {
println!("{} + {} = {}", x, y, x + y);
}
```


To specify a return type, use `->` after the parameters, like so:

```rust
fn double(x: i32) -> i32 {
return x + x;
}
```
---

## Data Types
Rust allows for the native use of arrays and tuples (Compound data types) alongside scalar data types.

### Scalar
Single values, basically what you'd expect. Ints, floats, chars, and single objects.

##### Slices
A slice is a binary representation, on the stack, of part of an object on the heap. In this way, a String literal is actually just a slice of a String - a reference + range on the stack. A slice [borrows](#Borrowing) from the original object to ensure safety.

### Compound

##### Tuples
Hold multiple values of varying types, and can be accessed with a "tuple index":

```rust
//&str is a string literal
let tup: (i32, f64, &str) = (500, 6.4, "Rust is 🔥🔥🔥");
println!("{}", tup.2); //Rust is 🔥🔥🔥
```
> Rust is 🔥🔥🔥
##### Arrays
Hold multiple values of single types. This is what an array looks like in Rust:

```rust
let arr = ["Rust", "is"];
let ar2 = ["🔥"; 3];
println!("{} {} {}{}{}", arr[0], arr[1], ar2[0], ar2[1], ar2[2]);
```
> Rust is 🔥🔥🔥

### Functions and Closures
A function can also be a data type. You can store a function in a variable for later use. There are two ways to store functions: as functions, or "closures".

```rust
fn foo(x: i32) -> i32 { x+x }
let a: fn(i32) -> i32 = foo; // This stores a function
let b = |x: i32| -> i32 { x+x }; // This is a closure
a(2);
b(2);
```

Both functions, a and b, return 4.

---

## Control Flow (1/3)
Rust doesn't require parentheses around boolean expressions in Control flow. In fact, Rust warns *against* it. Note that `while` and `if` work as expected.

#### for
`for` in Rust looks like "for element in elements": `for number in 1..4 { ... }`. This lends itself easily to the most common use-cases and increases legibility.

#### loop
A `loop` is simply a section of code that will repeat indefinitely until stopped by a `break`. There are no conditionals built into this control structure.

```rust
let mut n = 0;
loop {
    n += 1;
    if n > 5 { break; }
    else { print!("{}", n); }
}
```
> 12345

---

## Ownership

#### Rules of Ownership
1. Each value in Rust has a variable that’s called its owner.
2. There can only be one owner at a time.
3. When the owner goes out of scope, the value will be dropped.

#### Rules of References
1. At any given time, you can have either but not both of:
    * One mutable reference.
    * Any number of immutable references.
2. References must always be valid.

#### Borrowing
If you want to pass a value to a function without forfeiting ownership, you'll have to use a reference.

Here's an example of reading a value from a borrowed object.

```rust
fn main() {
    let s = String::from("🔥🔥🔥");
    println!("{}", count(&s));
}

fn count(s: &String) -> usize {
    return s.len()/4; // length in bytes divided by 4 (bytes per char)
}
```
> 3

And here's an example of modifying a borrowed object. Note the extra `mut` identifiers:

```rust
fn main() {
    let mut s = String::from("Rust is ");
    add_lit(&mut s);
    println!("{}", s.to_string());
}

fn add_lit(s: &mut String) {
    s.push_str("🔥🔥🔥");
}
```
> Rust is 🔥🔥🔥



---

## Structs
Structs are structs, as you would probably expect. Fields in a struct do not *require* a name. It's recommended to generally avoid borrowed types (like slices) for fields, because this can lead to issues with lifetimes.

```rust
struct Person {
    first_name: String, //this is a field, which is different from a var
    last_name: String,
}

let cool_cat = Person {
    first_name: String::from("Julien"),
    last_name: String::from("Cherry"),
};
```

#### Shorthand init with a builder function
The interesting part is the ability to fill fields of the same name with shorthand:

```rust
fn build_cherry(first_name: String) -> Person {
    Person {
        first_name, //Ain't that just the neatest?
        last_name: String::from("Cherry"),
    }
}
```

#### Shorthand init filling in from an initialized struct
You can use the `..` syntax to fill in data from an existing struct.

```rust
let cool_cat_2 = Person {
    first_name: String::from("Jimmy"),
    ..cool_cat //cool_cat is the struct var we made two code snippets up ^
};
```

#### Struct Methods
Use an `impl` block to define methods.

```rust
impl Person {
    fn full_name(&self) -> String {
        return format!("{} {}", self.first_name, self.last_name);
    }
}
```

##### Associated Methods
An associated method is a method that doesn't run on an instance of an object - similar to a static method.

```rust
impl Person {
    fn build_person(first_name: String, last_name: String) -> Person {
        Person { first_name, last_name }
    }
}
```

___

## Enums
You know enums already, but look at what you can do with Rust's enums!
(P.S. Enums have methods, too, with `impl`!)

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
```

#### Option
Rust uses an enum called Option to deal with non-existent values (null, in other langs). Use methods in the Option enum to check values.

---

## Control Flow (2/3)

#### match
Match is like "switch", but way more powerful, and doesn't fallthrough. In match, you use *patterns* instead of just values.

```rust
match var {
    pattern1 => {
                    do_something1();
                    return something;
                },
    pattern2 => something_else,
    _ => (),
}
```
(Remember, `()` is an empty tuple)

#### if let

```rust
`if let` takes a pattern and an expression separated by an =.

if let Some(3) = some_u8_value {
    println!("three");
}
```

---

## Modules

A module is a collection of vars, functions, and even sub-modules, which can be accessed using it's namespace.

- The `mod` keyword declares a new module. Code within the module appears either immediately following this declaration within curly braces or in another file.
- By default, functions, types, constants, and modules are private. The `pub` keyword makes an item public and therefore visible outside its namespace.
- The `use` keyword brings modules, or the definitions inside modules, into scope so it’s easier to refer to them.

#### Rules of Module Filesystems
- If a module named foo has no submodules, you should put the declarations for foo in a file named foo.rs.
- If a module named foo does have submodules, you should put the declarations for foo in a file named foo/mod.rs.

#### Example Module Configs
```
├── src
│   ├── main.rs
│   ├── lib.rs
│   ├── baz.rs
│   └── foo
│       ├── mod.rs
│       └── bar.rs
```

In the above config, the files would look like this:

```rust
// lib.rs
mod baz;
mod foo;
```

```rust
// baz.rs
pub fn hello() { ... }
```

```rust
// foo/mod.rs
pub mod bar;
pub fn ohej() { ... }
```

```rust
// foo/bar.rs
pub fn hi() { ... }
```

Or you could put it all into one file like this (This way gets ***messy*** in larger projects):

```rust
// lib.rs
pub mod baz {
    pub fn hello() { ... }
}

pub mod foo {
    pub fn ohej() { ... }
    pub mod bar {
        pub fn hi() { ... }
    }
}
```

Note how `pub` is used to make all of this data accessible to *other crates*. You will not want that in many cases, but for this example we do.

#### Using Modules
Using modules is actually pretty easy! Just make sure that you have your dependencies sorted out and use `extern crate ...` to import a crate. You can use `use` to add a namespace so accessing modules is easier.

```rust
extern crate some_crate;

use some_crate::foo::bar;
use some_crate::*;
/* Note: This   ^ is the same as
use some_crate::foo; and
use some_crate::baz;
*/

fn main() {
    baz::hello();
    some_crate::baz::hello();
    bar::hi();
    foo::ohej();
}
```

> hello
> hello
> hi
> o hej

##### Super and :: ...
`super` and `::` are used for relative namespace locations in modules. `::` brings you to the root of the crate, while `super` brings you up one level.

```rust
    // foo/bar.rs
    ...
    pub fn hello_ohej() {
        ::baz::hello();
        super::ohej();
    }
```
> hello
> o hej

---

## Error Handling
Rust distinguishes between recoverable and non-recoverable user errors. It has the value Result<T, E> for recoverable errors and the panic! macro that stops execution when it encounters unrecoverable errors.

#### panic! macro

```rust
fn main() {
    if oh_god_something_horrible_happened {
        panic!("the Disco");
    }
}
```
> `thread 'main' panicked at 'the Disco', src/main.rs:3`

#### Result
Result<T, E> is an enum that looks like this:

```rust
enum Result<T, E> {
    Ok(T),
    Err(E),
}
```
Here's how you might use Result to handle an error

```rust
use std::fs::File;

fn main() {
    let file = File::open("hello.txt");

    let file = match file {
        Ok(f) => f, // returns the file
        Err(error) => {
            panic!("There was a problem opening the file: {:?}", error)
        },
    };
}
```
#### Propogating errors
There are propogating an error is really just a fancy way to say "return Err(error)". It can be pretty annoying to write "if error return error" over and over, so Rust gives us a nice shorthand: the `?` operator!

```rust
use std::io::Error;
use std::io::Read;
use std::fs::File;

fn main() {
    match read_username_from_file() {
        Ok(username) => { println!("{}", username); },
        Err(error) => { println!("Whoops!"); panic!(error); },
    }
}

//If you propogate an error, you have to return a Result<T, E>
//otherwise you should be panicking or handling the error.
fn read_username_from_file() -> Result<String, Error> {
    let mut s = String::new();
    File::open("hello.txt")?.read_to_string(&mut s)?;
    Ok(s)
}
```

#### When to panic!
* The bad state is not something that’s expected to happen occasionally
* Code after this point needs to rely on not being in this bad state
* There’s not a good way to encode this information in the types you use

--- (To be continued)

##### TODO
Lifetimes, multithreading, Arc, Mutex, and all of that ~~complicated~~ *fun* stuff.
