---
title: Rust - Initial Impressions
date: '2023-02-04T06:31:14-08:00'
---
![Robot Crab](/img/blog/krabs.jpg)

As part of my New Year's resolution I've been studying the Rust programming language with the ultimate goal of building a bot for the game StarCraft.  I've been reading through the [Rust Programming Language Guide](https://doc.rust-lang.org/book/title-page.html) and uploading my notes and intermediate projects [here](https://github.com/jamesjmtaylor/rust).

So far the language syntactically (if not semantically) is very similar to Swift.  You can use `if let` statements to unwrap options, just like in Swift:

```rust
let config_max = Some(3u8);
if let Some(max) = config_max {
    println!("The maximum is configured to be {}", max);
} else {
    println!("The maximum is null!");
}
```

Rust also has Swift-style parameters and return type syntax, i.e.:

```rust
fn plus_one(x: i32) -> i32
```

Rust ranges and iterators also use the same `for` loop syntax as Swift:

```rust
let mut sum = 0;
for n in 1..11 {
    sum += n;
}

// Or

let menu = &["apple", "cake", "coffee"];

for item in menu  {
    println!("I want{}.", item);
}
```

Finally, Rust has a struct keyword like Swift:

```rust
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    // Borrows immutable self w/reference; we don't want to write, just read.
    fn area(&self) -> u32 { // if you need to write, use `&mut self`
        self.width * self.height
    }
}
```

Unlike Swift structs are either entirely mutable or immutable.  They can't contain functions and instead must declare them in an `impl` block as show above.

The semantics of Rust are actually much more similar to the first programming language I learned, [Ada](https://en.wikipedia.org/wiki/Ada_(programming_language)).
