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

