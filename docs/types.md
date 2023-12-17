# Types
A type is a single-value item that exists under as some non-strict subset of a parent type. We can also apply
invariants on types. We can also turn any type into null/error-unions using the `?` and `!` qualifiers. 

### Defining Types
Let's look at two templates for creating a new type:
```
type <name> is <parent> where <invariant>;
type <name> is <parent> where <invariant> {}
```
Types can only have one parent, and there are a couple ways to set one. A parent could be an alias for another type.
For example:
```
type Byte is u8;
```
Another type parent is `range`.
```
type Byte is range(0, 255)`
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
type OddByte is range(1, 255, 2) {
    fn inc(mut OddByte self) {
        self += 2;
    }
}

fn main() {
    mut b = OddByte(1);
    print(b);
    b.inc();
    print(b);
}
```
This program results in the following output:
```
1
3
```
