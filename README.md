# typelang
Type programming language v2

## Planned Features
This section is going to list a set of planned features in no particular order, lightly documented for my personal reference while developing.
Once a feature is complete I'll move it from planned to implemented with accurate documentation.

### What is type?
Type is a meant to be a hodgepodge of some of my favorite language features. Some languages that are inspiring type are Rust, Ada, Python, Java, Zig, etc.
I hope to pull out some of the best from all, and leave behind the worst, lol. To that here's my mental model for what I want type to look like.

Type is a statically and strongly typed language, although not as strong as Ada. Memory is semi-manually managed, you won't ever need to call `malloc` or `free`
but there are rules of memory management that you must follow, as not everything lives on the heap. The main point of the language is to encapsulate as many constraints in your
program as possible in the type system by creating new types and extending existing ones. If you view as much of your program from a "read-only" perspective
this offers quite a lot of safety, so types are immutable by default. Full immutability can hurt performance, and feel suffocating sometimes, so all types can be made mutable
with the use of the `mut` keyword. Immutability (or not) is baked into the type system, so you cannot pass an immutable instance of a variable to a function that expects a mutable
one, or vice versa. If you need a variable to live on the heap then one can be allocated using the `new` keyword, and it will be freed at the end of the owner function. Memory ownership
can be passed between functions, but must be done so explicity using the `give` (gives ownership of a heap allocated variable) and `take` (takes ownership of a heap allocated variable) keywords.
Sometimes a variable on the heap must be shared between threads, to do so you can use the `shared` keyword instead of the `new` keyword, and this will maintain a count of owners and will only free
until all owners free. Type also has built-in `optional` and `error` types (type is "exception by value" vs "exception by throwing"). Optional types can be made on the fly using a `?` and errors 
can be made using a `!`. Those are some of the basic concepts of type. There's much more to go over, but I'll document the rest in it's own section below.

### Statements
The following high-level constructs will be added:
- `type`
- `enum`
- `record`
- `interface`
- `fn`
Each of these will behave and be used slightly differently, have different options, etc.

#### Types
A `type` is a single consistently typed value, as opposed to a `record` (a collection of types) or an `enum` (a union of types).
There are two main "types" of `type`: `inherited` and `generic`. An `inherited` type is one that "is" another type.
These `inherited` types can act as an alias (is the same as) or constrain (is some subset of) a parent type. Type aliasing is easy:
```
// u8 is a built-in unsigned 8-bit integer
type Byte is u8;
```
however constraining a type is a little more involved. The first type of type constraint is done using the `range` keyword:
```
type Byte1 is range 0 .. 255;
type Byte2 is range 0 .. (2 ^ 8) - 1;
type SignedByte is range -128 .. 127;
type EveryOtherLowerCase is range "a" .. "z", 2;
```
the range keyword works with `char` and other `enum` types as well! The second way to constrain types is using the `where` keyword:
```
type OddByte is u8 where self % 2 == 1;
```
The above `where` clause on `OddByte` will be checked at runtime when setting or updating this type.
Note the use of the `self` word, you can also use the type name `OddByte` to refer to the value of the `type`.

There are also `generic` types, which do not inherit any other types. But I'm not quite sure what to do here yet!

We can also attach functions to a type like so:
```
type FlippableBool is bool {
  pub fn flip(mut self) {
    self = !self;
  }

  pub fn flip(self) self {
    return !self;
  }
}
```
#### Enum
An `enum` is a type union, it may or may not inherit another. Let's define one to start:
```
enum Event is
  str Create,
  str Update,
  Delete {

  fn process(self) {
    // do something with the event
  }
}
```
When creating an `enum` on it's own we use the `is` keyword followed by a comma seperated list of potential values. Values can by typed (they contain some extra value) or they can be untyped.
We can also contrain or alias an `enum` just like we can with regular types:
```
enum Day is
  Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday;

enum Weekday is range Day.Monday .. Day.Friday;
enum Weekend is range Day.Saturday .. Day.Sunday;
```

#### Record
A `record` is close to a struct, but it can have functions. A `record` and it's fields are immutable by default. If a field in a `record` is marked mutable then that field will be mutable even if the `record`
is immutable. Let's create a `record`:
```
record User {
  str userId;
  pub str userName;
}
```
We can then create and use our `record` like so:
```
alex = User("1", "alex");
print(alex.userName);
```
We can attach functions to a `record` like so:
```
record User {
  str userId;
  str userName;

  fn getUserName() str {
    return self.userName;
  }
}
```

#### Interface
todo
An `interface` is a way to write an API contract on `type`, `enum`, `record`, or `fn`. For example we could have the following `interface` for a File:
```
interface LocalFile {
  fn save(self);
  fn load(str path) self;
}

record File is LocalFile {
  fn save(self) { ... }
  fn load(str path) self { ... }
}
```
We can apply interfaces to other things:
```
interface BinOp {
  fn apply(u8 l, u8 r);
}

fn add(u8 l, u8 r) { return l + r; }
```










