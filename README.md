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
- `format!` write formatted text to String
- `print!` same as format! but the text is printed to the console (io::stdout).
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
- Despite the value of a unit type being a tuple, it is not considered a compound type because it does not contain multiple values.

##### Compound Types

- `arrays` like [1, 2, 3]
- `tuples` like (1, true)

#### Custom Types

- `structs` like

```
    // A struct with two fields
    #[derive(Debug)]
    struct Point {
        x: f32,
        y: f32,
    }
```

- `to_owned()` creates an owned `String` from a string slice.
  ```
  "my text".to_owned() //similar to String::from("my text");
  ```
- `use` The use declaration can be used so manual scoping isn't needed.

  ```
  enum Work {
    Civilian,
    Soldier,
  }

  fn main() {
    use crate::Work::*; or use crate::Work::{ Civilian, Soldier};

    // Equivalent to `Work::Civilian`.
    let work = Civilian;
  }
  ```

  `enum can also be used as C-like enums.`

  ```
    // enum with implicit discriminator (starts at 0)
    enum Number {
        Zero,
        One,
        Two,
    }

    // enum with explicit discriminator
    enum Color {
        Red = 0xff0000,
        Green = 0x00ff00,
        Blue = 0x0000ff,
    }

    println!("one is {}", Number::One as i32);

    println!("roses are #{:06x}", Color::Red as i32);

  ```

- `constants`

  - `const`: An unchangeable value (the common case).
  - `static`: A possibly mutable variable with 'static lifetime. The static lifetime is inferred and does not have to be specified. Accessing or modifying a mutable static variable is unsafe.`

  ```

   // Globals are declared outside all other scopes.
   static LANGUAGE: &str = "Rust";
   const THRESHOLD: i32 = 10;

   fn is_big(n: i32) -> bool {
   // Access constant in some function
   n > THRESHOLD
   }

   fn main() {
   let n = 16;

   // Access constant in the main thread
   println!("This is {}", LANGUAGE);
   println!("The threshold is {}", THRESHOLD);
   println!("{} is {}", n, if is_big(n) { "big" } else { "small" });

   // Error! Cannot modify a `const`.
   THRESHOLD = 5;
   // FIXME ^ Comment out this line
   }

  ```

- `Variable Bindings`

  - Rust provides type safety via static typing.
  - Variable bindings can be type annotated when declared.
  - In most cases, the compiler will be able to infer the type of the variable from the context, heavily reducing the annotation burden.
  - `Mutability`
    - Variable bindings are immutable by default, but this can be overridden using the mut modifier.
  - `Scope and Shadowing`
    - Variable bindings have a scope, and are constrained to live in a block. A block is a collection of statements enclosed by braces {}.
  - `Declare First`

    - It's possible to declare variable bindings first, and initialize them later. However, this form is seldom used, as it may lead to the use of uninitialized variables.

    ```

    fn main() {
        // Declare a variable binding
        let a_binding;

        {
            let x = 2;

            // Initialize the binding
            a_binding = x * x;
        }

        println!("a binding: {}", a_binding);
    }
    ```

  - `Freezing`

    - When data is bound by the same name immutably, it also freezes. Frozen data can't be modified until the immutable binding goes out of scope.

    ```
    fn main() {
        let mut _mutable_integer = 7i32;

        {
            // Shadowing by immutable `_mutable_integer`
            let _mutable_integer = _mutable_integer;

            // Error! `_mutable_integer` is frozen in this scope
            _mutable_integer = 50;
            // FIXME ^ Comment out this line

            // `_mutable_integer` goes out of scope
        }

        // Ok! `_mutable_integer` is not frozen in this scope
        _mutable_integer = 3;
    }
    ```

- `FOR LOOP`

  - `a..b` - end exlusive
  - `a..=b` - end inlusive
  - `iter (no ownership destroyed)` - This borrows each element of the collection through each iteration. Thus leaving the collection untouched and available for reuse after the loop.
  - `into_iter (ownership destroyed)` - This consumes the collection so that on each iteration the exact data is provided. Once the collection has been consumed it is no longer available for reuse as it has been 'moved' within the loop.
  - `iter_mut` - This mutably borrows each element of the collection, allowing for the collection to be modified in place.

- `ASSOCIATED FUNCTIONS AND METHODS`

  - Some functions are connected to a particular type.
    These come in two forms: 

    - `Associated` functions are functions that are defined on a type generally
      
      ```console
      Point::new(3.0, 4.0)
      ```
      
    - `Methods` are `associated functions` that are called on a particular instance of a type.

      ```console
      let rectangle = Rectangle {
        // Associated functions are called using double colons
        p1: Point::origin(),
        p2: Point::new(3.0, 4.0),
      };

      // Methods are called using the dot operator
      // Note that the first argument `&self` is implicitly passed, i.e.
      // `rectangle.perimeter()` === `Rectangle::perimeter(&rectangle)`
      println!("Rectangle perimeter: {}", rectangle.perimeter());
      ```

#### Random Info

- Rust provides pattern matching via the `match` keyword, which can be used like a C `switch`. The first matching arm is evaluated and all possible values must be covered.

#### Generate Binary

- A binary can be generated using the Rust compiler: `rustc`.

## References

- [Rust by example](https://doc.rust-lang.org/stable/rust-by-example/)
- [Book: The Rust Programming Language](https://doc.rust-lang.org/book/)
