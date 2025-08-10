# What *abstraction* really is (conceptual)

* **Abstraction = hiding complexity, exposing what’s essential.**
  You show *what* an object can do, not *how* it does it.
* It’s a design tool to reduce cognitive load, improve modularity, and enable changing internals without breaking callers.
* Two levels:

  * **High-level (API/contract):** What operations exist (e.g., `List.add()`).
  * **Low-level (implementation):** How those ops work (array vs linked list).

# Abstraction in Java — two language mechanisms

1. **Abstract classes** — partial abstraction: can have abstract methods *and* concrete methods/fields.
2. **Interfaces** — pure contract historically; since Java 8 they can include `default` and `static` methods (so they can carry some behavior).

Both let you program to a name (type) rather than concrete implementation — key for polymorphism and loose coupling.

# Java: abstract class — anatomy & examples

```java
// Abstract class with state + behavior
abstract class Shape {
    protected String color;                 // shared state
    public Shape(String color) { this.color = color; } // constructor allowed
    public abstract double area();          // abstract method (no body)
    public void describe() {                // concrete method
        System.out.println("A " + color + " shape");
    }
}
class Circle extends Shape {
    private double r;
    public Circle(String color, double r) { super(color); this.r = r; }
    @Override
    public double area() { return Math.PI * r * r; }
}
```

* You **cannot** do `new Shape(...)` → compile error: `Shape is abstract; cannot be instantiated`.
* Abstract class **can** have constructors, static methods, private helper methods, fields, `final` methods, etc.
* **Abstract methods cannot be `private`, `final`, or `static`** because they must be overridable instance methods.

# Why Java enforces “no objects of abstract class”

* Abstract class may contain unimplemented behavior. Instantiating it would allow calling methods with *no body* — undefined behavior. Java forces a concrete subclass to provide implementations.

# Key rules & corner cases (memorize these)

* Abstract class can have **0 or more** abstract methods. (You can declare class `abstract` even without abstract methods — to prevent instantiation.)
* A subclass must either implement all abstract methods or be declared `abstract` itself.
* An abstract method cannot be `private`, `static`, or `final`.
* You **can** have `main()` inside abstract class (useful for testing).
* A class cannot be `abstract` and `final` at the same time — contradictory.
* Anonymous inner classes can instantiate an abstract class inline by implementing missing methods.

# Comparison: abstract class vs interface (practical decision points)

When to prefer an **abstract class**:

* You need to share **state** (instance fields) or protected members.
* You want to provide **partial implementation** to avoid code duplication.
* You want to define **constructors** to control initialization.
* You want to add future concrete helper methods without breaking subclasses (but interfaces with default partly fix this).

When to prefer an **interface**:

* You want **multiple inheritance** of type (a class can implement many interfaces).
* You only need to define a **contract** (no state).
* Use functional interface when you want lambda-friendly single abstract method types.

# How abstraction is used in design (patterns & examples)

* **Template Method** (classic use of abstract class)
  Abstract class defines algorithm skeleton; subclasses supply steps.

  ```java
  abstract class Game {
      final void play() { start(); playTurn(); end(); } // skeleton
      abstract void start(); abstract void playTurn(); abstract void end();
  }
  ```
* **Strategy pattern** — often uses interfaces (or abstract classes) so algorithms are interchangeable.
* **Factory method** — abstract class declares factory method that subclasses implement.

# Practical examples from Java standard library (conceptual)

* `java.io.InputStream` (abstract): declares `read()`; subclasses implement specific I/O details.
* `AbstractList` (abstract): provides partial implementation for `List` so concrete lists only implement core methods.
  (Reason: provide common logic, reduce code duplication.)

# Advanced / design considerations

* **Versioning:** Adding a method to an interface used to break all implementers; Java 8 `default` methods reduced that problem. Abstract classes give you more freedom to evolve behavior without breaking subclasses.
* **Protected vs private:** Use `protected` for extension points but avoid exposing internal mutable state to subclasses unless necessary (it couples subclass to internal representation).
* **Composition over inheritance:** Don’t reach for an abstract class when you only need to reuse behavior — prefer composing helper objects (less coupling).
* **Open/Closed Principle:** Abstraction helps — write code that’s open for extension (subclasses) but closed for modification.

# Common pitfalls & anti-patterns

* **God abstract class**: Putting too much shared logic/state into one base class makes hierarchy rigid.
* **Leaky abstraction**: Protected fields expose representation; subclasses depend on them; later changes break many classes.
* **Deep inheritance chains**: Hard to reason about and maintain.
* **Using abstract class where interface would be lighter**: prevents multiple type inheritance and reduces flexibility.

# Interview-style short Qs & answers

* Q: *Can an abstract class have a constructor?* — **Yes**. It runs when subclass is instantiated.
* Q: *Can abstract class be final?* — **No** (contradiction).
* Q: *Can abstract method be static?* — **No**.
* Q: *Can you instantiate an abstract class using anonymous inner class?* — **Yes** (by implementing missing methods inline).
* Q: *Interface vs abstract class — big difference?* — Interfaces are contracts (multiple inheritance), abstract classes can hold state and partial impl.

# Concrete practice exercises (try these)

1. Create an abstract `Logger` class with `abstract void write(String msg)` and a concrete `log(String msg)` that timestamps + calls `write`. Implement `FileLogger` and `ConsoleLogger`.
2. Implement Template Method: abstract `DataParser` with `parseAndSave()` calling `read()`, `process()`, `save()`; implement CSV and JSON parsers.
3. Convert a `Strategy` that uses an interface into one that uses an abstract base class — observe state differences.

# Best practices checklist (copy into resume/study notes)

* Use abstract classes for shared **state** + behavior.
* Keep abstract APIs small and well-documented.
* Prefer `protected` for extension points, but keep invariants private and expose controlled methods.
* Avoid exposing internal mutable fields.
* Favor composition when you need to reuse behavior without creating a tight inheritance relationship.
* When exposing hooks (abstract methods), document pre/postconditions so subclasses know responsibilities.

---
