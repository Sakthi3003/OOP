### ** INTERFACE **

### **1. Why Interfaces?**

Java does not support **multiple inheritance** for classes.
If two parent classes have the **same method signature** but different implementations, Java wouldn’t know which one to inherit — this is the *ambiguity problem*.

To solve this, Java provides **interfaces**, which allow multiple inheritance **of type** without inheriting conflicting state or implementations.

---

### **2. What is an Interface?**

An **interface** is like a completely abstract contract:

* Specifies **what** a class must do, not **how** it does it.
* Similar to an abstract class, but more restricted.
* By default:

  * **Methods** are `public` and `abstract` (unless marked `default` or `static`).
  * **Variables** are `public static final` (constants).
* Cannot have instance variables or constructors.

---

### **3. Key Differences: Class vs Interface**

| Feature            | Class               | Interface                                   |
| ------------------ | ------------------- | ------------------------------------------- |
| Instance variables | Allowed             | Not allowed (only constants)                |
| Constructors       | Allowed             | Not allowed                                 |
| Method type        | Concrete + abstract | Abstract (plus default/static since Java 8) |
| Inheritance        | Single              | Multiple allowed                            |
| State              | Can maintain state  | Cannot maintain state                       |

---

### **4. Benefits of Interfaces**

* **Full abstraction** — separates specification from implementation.
* **Multiple inheritance of type** — a class can implement multiple interfaces.
* **Polymorphism** — one interface, many implementations.
* **Decoupling** — caller code depends on interface, not concrete class.
* **Dynamic method resolution at runtime** — method calls are bound to the actual object type at runtime.

---

### **5. Syntax**

```java
interface Vehicle {
    int MAX_SPEED = 120; // public static final

    void start(); // public abstract
    void stop();

    default void honk() { // default method
        System.out.println("Beep Beep!");
    }

    static void service() { // static method
        System.out.println("Vehicle servicing");
    }
}
```

---

### **6. Implementation**

```java
class Car implements Vehicle {
    public void start() { System.out.println("Car starting..."); }
    public void stop() { System.out.println("Car stopping..."); }
}
```

---

### **7. Multiple Interfaces**

A class can implement more than one interface:

```java
interface Engine { void engineType(); }
interface Wheels { void wheelCount(); }

class Bike implements Engine, Wheels {
    public void engineType() { System.out.println("Petrol Engine"); }
    public void wheelCount() { System.out.println("Two Wheels"); }
}
```

---

### **8. Nested Interfaces**

An interface can be declared inside another interface or class (called a **member interface**).

#### Inside a Class:

```java
class A {
    public interface NestedIF {
        boolean isNotNegative(int x);
    }
}

class B implements A.NestedIF {
    public boolean isNotNegative(int x) {
        return x >= 0;
    }
}

class Demo {
    public static void main(String[] args) {
        A.NestedIF nif = new B();
        System.out.println(nif.isNotNegative(10)); // true
    }
}
```

**Access Modifiers:**

* Nested interfaces in a class can be `public`, `private`, or `protected`.
* Top-level interfaces can only be `public` or package-private.

---

### **9. Interface Inheritance**

Interfaces can extend other interfaces:

```java
interface A { void methodA(); }
interface B extends A { void methodB(); }
```

A class implementing `B` must implement both `methodA()` and `methodB()`.

---

### **10. Default Methods (Java 8+)**

Default methods let you add new methods to an interface **without breaking existing implementations**.

Example:

```java
interface MyInterface {
    default String getString() { return "Default String"; }
}
```

**Rules & Conflicts:**

* A **class implementation** always takes priority over a default method.
* If two interfaces provide the same default method and a class implements both, the class must override it (diamond problem resolution).
* If one interface extends another and both have the same default method, the child interface’s version wins.

---

### **11. Static Methods in Interfaces**

* Must have a body.
* Cannot be abstract.
* Not inherited by implementing classes or subinterfaces.

---

### **12. Method Overriding Rules**

* Overriding methods must be `public`.
* Signature must match exactly.
* Access modifiers can be the same or more accessible.

---

### **13. Using Interfaces as References**

You can declare variables as interface references:

```java
Vehicle v = new Car();
v.start(); // Calls Car's implementation
```

The correct method is determined **at runtime** (dynamic dispatch).

---

### **14. Performance Consideration**

Dynamic lookup of interface methods is slightly slower than normal method calls.
Avoid excessive use in **performance-critical** code.
---

## **1. Class Implementation Priority**

**Rule:**
If a class **implements an interface** that has a default method **and** the class provides its own implementation of that method, the **class version wins** — even if the interface has a default.

**Why?**
Because in Java’s inheritance rules, **class members take precedence over interface default methods**.
Java assumes that if you implemented it in the class, you want your version.

**Example:**

```java
interface A {
    default void show() {
        System.out.println("Default from A");
    }
}

class MyClass implements A {
    @Override
    public void show() {
        System.out.println("From MyClass");
    }
}

public class Demo {
    public static void main(String[] args) {
        A obj = new MyClass();
        obj.show(); // Output: From MyClass
    }
}
```

Even though interface `A` has a default, `MyClass`'s version is used.

---

## **2. Two Interfaces with the Same Default Method (Diamond Problem)**

**Rule:**
If a class implements **two interfaces** that define the **same default method signature**, Java forces you to override it in the class — otherwise, compilation fails.

**Why?**
This is the **diamond problem**:
Java doesn't know **which default** to choose, so it makes you explicitly decide.

**Example:**

```java
interface A {
    default void show() {
        System.out.println("Default from A");
    }
}

interface B {
    default void show() {
        System.out.println("Default from B");
    }
}

class MyClass implements A, B {
    @Override
    public void show() {
        // You can choose one default explicitly
        A.super.show(); // or B.super.show();
        System.out.println("Resolved in MyClass");
    }
}

public class Demo {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.show();
    }
}
```

If you remove `@Override show()` in `MyClass`, you’ll get:
`Error: class MyClass inherits unrelated defaults for show() from types A and B`.

---

## **3. Interface Inheritance with Conflicting Defaults**

**Rule:**
If one interface **extends another** and both define a **default method with the same signature**, the **child interface's version overrides the parent’s version**.

**Why?**
Interface inheritance works like class inheritance — the most specific (lowest-level) version wins.

**Example:**

```java
interface Parent {
    default void greet() {
        System.out.println("Hello from Parent");
    }
}

interface Child extends Parent {
    @Override
    default void greet() {
        System.out.println("Hello from Child");
    }
}

class MyClass implements Child {
    // No need to override — Child's default is used
}

public class Demo {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.greet(); // Output: Hello from Child
    }
}
```

Here:

* `Parent` has a default `greet()`
* `Child` extends `Parent` and overrides it
* Any class implementing `Child` **automatically gets Child’s version**

---

### **Summary Table**

| Scenario                                              | Which version is used?                     |
| ----------------------------------------------------- | ------------------------------------------ |
| Class defines method                                  | Class version wins                         |
| Two interfaces have same default                      | Must override in class to resolve conflict |
| One interface extends another, both have same default | Child interface version wins               |




