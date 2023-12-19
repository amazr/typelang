# Loops
Type has two loop constructs: the `for` and `while` loop. The `while` loop can be used to loop conditionally,
and the `for` loop is used with Types zero-cost iterators. Here's an example of both:
```
fn main() {
    mut i = 0;
    while i < 10 {
        print("i is {i}");
        i++;
    }

    for i in 0 .. 10 {
        print("i is {i}");
    }
}
```

We can also implement custom iterators for types. Say we wish to iterator over all of the odd numbers in a single byte
range:
```
type OddByteItr is u8 {
    pub fn next(mut self) self? {
        let next = self + 2;
        ret next >= u8.last() ? empty : self := next;
    }
}


fn main() {
    for i in OddByteItr
}
```
