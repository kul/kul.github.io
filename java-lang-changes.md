```java
/// 10-Oct-2024:
/// This example intends to cover recent language changes to java
/// which I have not been fully aware of until recently and have neve
/// used in production code as well.
/// This comment itself shows `markdown comment` features we can use with
/// comments which start with `///`.
/// #### Notable language features covered here:
/// * record
/// * switch expression
/// * easy-main - implicit class and instance level main method
/// * pattern matching
/// * unnamed variables
/// * text blocks

void main() {                                    // implicit classes and instance level main

    enum Type {RECTANGLE, POINT, SQUARE}         // local enums

    record Geometric(int x, int y, Type type) {  // records
        public Geometric {                       // compact record constructor
            if (type == Type.SQUARE)
                assert x == y;
        }

        public Geometric(int x, int y) {
            Type defaultType = Type.RECTANGLE;        // statements before super
            this(x, y, defaultType);
        }
    }

    var g = new Geometric(1, 2);                      // local var inference

    int area = switch (g) {                                                // switch expression (no fall through)
        case Geometric(int x, int y, Type t) when t!=Type.POINT -> x*y;    // case guard and case pattern match
        case Object x -> {
            if (x instanceof Geometric(_, _, Type t) && t == Type.POINT) { // unnamed variables, instance of pattern match
                println("No area for point.");
            } else {
                println("Non geometric shape.");
            }
            yield 0;                                                       // yield keyword
        }
    };
    println(String.format("""
            area = %s
            """, area)); // text blocks and print helpers with implicit import
}
```
