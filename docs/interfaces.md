# Interfaces
Interfaces are way to collect and group behaviours, and then apply to them records to ensure they have the correct
operations. You can also seal interfaces, allowing only an explicity defined set of records to implement.

```
interface Shape {
    fn area(self) uint;
}

record Rectangle is Shape {
    uint h;
    uint w;

    pub fn area(self) uint {
        ret h * w;
    }

    pub fn is_square(self) Square? {
        ret h == w ? Square(h) : null;
    }
}

record Square is Shape {
    uint l;

    pub fn area(self) uint {
        ret l * l;
    }
}
```
and sealed interfaces:
```
sealed Seal permits Sealion, Walrus, ElephantSeal;

record Sealion;
record Walrus;
record ElephantSeal;
```
