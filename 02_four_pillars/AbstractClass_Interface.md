## **1️⃣ What is an Abstract Class?**

* An **abstract class** is a class you **cannot directly instantiate** (no `new` keyword on it) because it might contain **incomplete behavior**.
* Declared with the `abstract` keyword:

  ```java
  abstract class Shape {
      abstract void draw(); // No implementation here
  }
  ```

---

## **2️⃣ What can an Abstract Class contain?**

An abstract class is *halfway* between a normal class and an interface. It can have:

| Feature                         | Allowed? | Notes                                          |
| ------------------------------- | -------- | ---------------------------------------------- |
| **Abstract methods**            | ✅        | Must be implemented by first concrete subclass |
| **Concrete methods**            | ✅        | Can be inherited or overridden                 |
| **Instance variables (fields)** | ✅        | Unlike interfaces, can store state             |
| **Constructors**                | ✅        | Used when subclass objects are created         |
| **Static methods**              | ✅        | Belong to class, not instance                  |
| **Final methods**               | ✅        | Cannot be overridden                           |
| **Private methods**             | ✅        | For internal use only                          |
| **Any access modifier**         | ✅        | `public`, `protected`, `default`, `private`    |

---

## **3️⃣ Why Can’t We Create Objects of It?**

* Abstract means *incomplete* — at least one method has no body.
* If Java allowed creating objects, calling that unimplemented method would be impossible.
* The compiler enforces subclassing to complete the definition.

---

## **4️⃣ Uses of Abstract Classes**

Here’s where abstract classes shine:

### **a) Enforcing a Contract + Reusing Code**

* You can **force** subclasses to implement required methods *and* share common logic.

  ```java
  abstract class Employee {
      abstract double calculatePay();
      void workHours() {
          System.out.println("Works 9 to 5");
      }
  }
  ```

### **b) Template Method Pattern**

* Abstract class defines **the skeleton of an algorithm**, leaving some steps abstract.

  ```java
  abstract class Game {
      final void play() { start(); run(); end(); }
      abstract void start();
      abstract void run();
      abstract void end();
  }
  ```

### **c) Partial Implementation**

* You might not be ready to give a full implementation but want to give **default behavior**.

### **d) When You Need State + Inheritance**

* Interfaces can’t hold instance fields (except constants), but abstract classes can.
* Use this if you want shared fields + partial implementations.

---

## **5️⃣ Differences from Interfaces**

| Feature              | Abstract Class          | Interface                     |
| -------------------- | ----------------------- | ----------------------------- |
| Fields               | Yes (instance + static) | Only `public static final`    |
| Constructors         | Yes                     | No                            |
| Multiple inheritance | No (only one)           | Yes                           |
| Default impl         | Yes (concrete methods)  | Yes (Java 8+ default methods) |
| State                | Can store state         | No instance state             |

---

## **6️⃣ Special Things to Know**

* **Abstract class can have no abstract methods** — sometimes used to prevent instantiation but allow subclassing.
* You **can** have an abstract subclass — then *its* subclasses must implement methods.
* **Anonymous classes** can instantiate abstract classes inline:

  ```java
  Shape s = new Shape() {
      void draw() { System.out.println("Circle drawn"); }
  };
  ```
* You **cannot** mark an abstract method as `private`, `final`, or `static` — these contradict overriding rules.

---

## **7️⃣ When to Prefer Abstract Class Over Interface**

Use an abstract class when:

1. You need **shared code** across subclasses.
2. You want **instance variables**.
3. You want to **control constructor calls**.
4. The “is-a” relationship is clear (e.g., `Dog` is-an `Animal`).

---

## **8️⃣ Real-World Example**

```java
abstract class PaymentProcessor {
    double amount;
    PaymentProcessor(double amount) {
        this.amount = amount;
    }
    abstract void process();
    void logTransaction() {
        System.out.println("Transaction logged: " + amount);
    }
}

class CreditCardPayment extends PaymentProcessor {
    CreditCardPayment(double amount) { super(amount); }
    void process() {
        System.out.println("Processing credit card payment of " + amount);
    }
}

public class Main {
    public static void main(String[] args) {
        PaymentProcessor pp = new CreditCardPayment(500);
        pp.process();
        pp.logTransaction();
    }
}
```

This gives you:

* Common **logging code** in the abstract class.
* Forced `process()` implementation in subclasses.

---


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




