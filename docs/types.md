# Types
A type is a single-value item that exists under as some non-strict subset of a parent type. We can also apply
invariants on types. We can also turn any type into null/error-unions using the `?` and `!` qualifiers. 

### Defining Types
Let's look at two templates for creating a new type:
```
type <name> <range> where <invariant>;
type <name> is <parent> where <invariant> {}
```
Types can only have one parent, and there are a couple ways to set one. A parent could be an alias for another type, in that
case we use the `is` keyword.
For example:
```
type Byte is u8;
```
The following builtin types exist:
- u8, u16, u32, u64, uint
- i8, i16, i32, i64, int
- f8, f16, f32, f64, float
- str, char
- list, vec, map, set

These are all normal types, except for `uint`, `int`, and `float`. These values are unbounded and will grow to contain
larger values if need be (similar to vec).


Another type parent is `range`. Note we do not use the `is` keyword.
```
type Byte range 0 .. 255`
```
In the above example `Byte` is a numeric with the given range. We can range over types with sequential values like chars or enums.
If we read this Byte type we know for sure the range of it's possible values, and can write less guard code.

For more complicated constraints we can add invariants. Imagine we want a type that is only odd numbered values in the
range of a byte. We could write:
```
type OddByte is u8 where b -> b % 2 == 1;
```
The `where` clause expects a function with `(OddByte) -> bool`.

We can also add functions to our types that operate on the current value. For example:
```
type OddByte range 1 .. 255 where b -> b % 2 == 1 {

    fn inc(mut self) {
        self += 2;
    }
}

fn main() {
    mut b = OddByte(1).or_terminate();
    print(b);
    b.inc();
    print(b);
}
```
Note that the constructor for `OddByte` actually returns an `OddByte!`, that must be unwrapped. Here we use `.or_terminate()` but you
could also use a `match` block.
This program results in the following output:
```
1
3
```
