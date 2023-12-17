# Enums
Enums in typelang define a set of potential values, where only one is selected at a given time. The potential values
can be samely typed or differently typed. Let's just jump into some examples:
```
enum Day {
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday;
}
```
Pretty straightforward. Say we want to create some code that set's an alarm once per day, but at different times
depending on if the current day is a weekday or a weekend. We could encapsulate our day filtering using enums:
```
enum Weekday range Day.Monday .. Days.Friday;
enum Weekend range Day.Saturday .. Days.Sunday;

fn main() {
    let curr = Day.Monday;
    match curr {
        Weekday -> print("Alarm set for 8am");
        Weekend -> print("Alarm set for 10am");
    }
}
```

We can also create differently typed enums, similar to unions from C.
```
enum Event {
    Create(str id),
    Update(str id, u64 updatedAt),
    Delete(str id);
}

fn handleEvent(Event e) {
    match e {
        Create -> print("Creating {e.id}");
        Update -> print("Updating {e.id} at {e.updatedAt}");
        Delete -> print("Deleting {e.id}");
    }
}
```

Enums also support runtime invariants. For example, say we want subtype our `Day` enum to only our favorite days:
```
enum FavoriteDay is Day where isFavorite {
    fn isFavorite(self) bool {
        ret match self {
            Day.Wednesady -> true;
            Day.Friday -> true;
            Day.Saturday -> true;
            def -> false;
        }
    }
}

fn main() {
    let curr = Day.Monday;
    match curr {
        FavoriteDay -> print("Yay");
        def -> print(":(");
    }
}
```
