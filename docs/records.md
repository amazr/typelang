# Records
Records are a way to hold a collection of different values, and to define behaviour on that same collection. You
can think of them like structs from C, but with functions attached. We can define a simple `User` record like so:
```
record User {
    str userId;
    str userName;
    Date birthday;

    pub fn isBirthday(self, Date date) bool {
        ret date == self.birthday;
    }
}
```
This `User` has an id, a name, and a birthday. It also has a public function to check if it's the users birthday.
We can also use `where` clauses on records.
```
record Square where w == h {
    uint w;
    uint h;

    pub fn area(self) uint {
        ret w * h;
    }
}

fn main() {
    let s = Square(10, 10).or_terminate();
    print("Square has area {s.area()}");
}
```
We can also do use null/error unions for types. Here's a contrived example:
```
record MaybeHolder {
    uint? x;
}
record Example {
    MaybeHolder? maybeHolder;
    uint! y;
}

fn main() {
    let example = Example(MaybeHolder(1), 2);
    let x = example.maybeHolder.x.or_terminate();
    print("X is {x}")
}
```
