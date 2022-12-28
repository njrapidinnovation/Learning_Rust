# Learning-Rust
Rust programming language Learnings


## Installation
### Linux or MacOS
```console
$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh
```

## Getting Started
### Code
```rs
fn main() {
	println!("Hello World!");
}
```

### Compile
```console
$ rustc hello.rs
```

### Output
```console
$ ./hello
```

### Learnings

#### Macro
Rust provides a powerful macro system that allows metaprogramming. As you've seen in previous chapters, macros look like functions, except that their name ends with a bang !, but instead of generating a function call, macros are expanded into source code that gets compiled with the rest of the program.

- `println!` is a macro that prints text to the console.
- `format!`  write formatted text to String
- `print!`   same as format! but the text is printed to the console (io::stdout).
- `eprint!` same as print! but the text is printed to the standard error (io::stderr).
- `eprintln!` same as eprint! but a newline is appended.

#### Formatting
- `{value of which decimals has to be controll:.number of decimals to control}` for example:
```console
    let _pi : f64 = 3.141592;
    let decimals : usize = 3;
    println!("Value of PI = {0} upto four decimals is {0:.1$}", _pi, decimals);
```

#### Primitives

##### Scalar Types
- `signed integers`: i8, i16, i32, i64, i128 and isize (pointer size)
- `unsigned integers`: u8, u16, u32, u64, u128 and usize (pointer size)
- `floating point`: f32, f64
- `char` Unicode scalar values like 'a', 'α' and '∞' (4 bytes each)
- `bool` either true or false
- and the unit type (), whose only possible value is an empty tuple: ()
- Despite the value of a unit type being a tuple, it is not considered a compound type because it    does not contain multiple values.

##### Compound Types
- `arrays` like [1, 2, 3]
- `tuples` like (1, true)

#### Generate Binary
- A binary can be generated using the Rust compiler: `rustc`.

## References
* [Rust by example](https://doc.rust-lang.org/stable/rust-by-example/)
* [Book: The Rust Programming Language](https://doc.rust-lang.org/book/)