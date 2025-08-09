## **Inheritance in Java**

### **1. What is Inheritance?**

Inheritance lets you create a new class (subclass/child) that reuses, extends, or modifies the behavior of an existing class (superclass/parent).

```java
class SubclassName extends SuperclassName {
    // body of subclass
}
```

**Key rules:**

* A class can only **extend one** superclass (Java does **not** support multiple inheritance of classes).
* You can have a **hierarchy**: a subclass can be a superclass for another subclass.
* A class **cannot be a superclass of itself**.
* A subclass inherits **all non-private members** of its superclass.
* Private members of the superclass are **not accessible** directly.

---

### **2. Superclass Reference to Subclass Object**

* **Rule:** The type of the **reference variable**, not the object, determines what members are accessible.

Example:

```java
Superclass ref = new Subclass(); // Allowed
ref.superclassMethod(); // ✅
ref.subclassMethod();   // ❌ Compile-time error
```

```
plainbox   = weightbox;
(super)      (subclass)
```

---

### **3. The `super` Keyword**

The `super` keyword refers to the immediate superclass.

#### **Uses of `super`:**

1. **Call superclass constructor**

```java
class BoxWeight extends Box {
    double weight;
    BoxWeight(double w, double h, double d, double m) {
        super(w, h, d); // calls Box constructor
        weight = m;
    }
}
```

* Even if you pass a `BoxWeight` object to `super()`, it will call the appropriate superclass constructor (`Box`).

2. **Access hidden members**

```java
class Parent {
    int value = 10;
}
class Child extends Parent {
    int value = 20;
    void show() {
        System.out.println(value);       // 20
        System.out.println(super.value); // 10
    }
}
```

**Important:**

* In multi-level inheritance, `super()` always calls the **closest superclass constructor**.
* If the superclass has a parameterized constructor, **all subclasses** must pass parameters “up the chain”.
* If you don’t use `super()`, the **default (no-arg) constructor** of the superclass is called.

---

### **4. Why Superclass Constructors Run First**

* Superclass initialization must be completed before subclass initialization.
* Superclass has **no knowledge** of subclass fields, so its own setup happens first.

---

### **5. `final` with Inheritance**

#### **a. Create constants**

```java
final double PI = 3.14159;
```

#### **b. Prevent method overriding**

```java
class Parent {
    final void show() {
        System.out.println("Can't be overridden");
    }
}
```

* **Performance tip:** Final methods can be **inlined** by the compiler (early binding).

### 1. **Normal method calls (late binding / dynamic dispatch)**

* By default, Java uses *late binding* for non-`final`, non-`static`, non-`private` methods.
* This means the actual method implementation to be called is determined **at runtime** based on the object type.
* Late binding allows **polymorphism** but comes with a small performance cost because the JVM must look up the method in a virtual method table (vtable) each time.

---

### 2. **Final methods (early binding / inlining)**

* When you declare a method `final`, the compiler **knows** it cannot be overridden.
* Because of this guarantee, the compiler (or JIT at runtime) can:

  * **Early bind** — Resolve the call at **compile time** instead of runtime.
  * **Inline** the method — Directly copy the method’s bytecode into the caller’s code, removing the method call overhead.

---

### 3. **Inlining example**

```java
class Test {
    final int square(int x) {
        return x * x;
    }

    int sumOfSquares(int a, int b) {
        // Instead of calling square(a) and square(b), 
        // the compiler can replace them with a*a and b*b directly.
        return square(a) + square(b);
    }
}
```

With inlining, the JVM may optimize `sumOfSquares` to:

```java
return a * a + b * b;
```

No method call overhead.

---

### 4. **Key Notes**

* Inlining is **not guaranteed** just because a method is `final` — it’s an *option* the compiler or JIT may choose.
* The real performance benefit is usually noticeable only for **small, frequently called methods** (e.g., getters, setters, math operations).
* Other cases of early binding:

  * `private` methods (can’t be overridden)
  * `static` methods
  * `final` classes (all methods can be early bound)

---


#### **c. Prevent inheritance**

```java
final class MyClass { }
```

* A `final` class cannot be extended.
* `abstract` and `final` together are **illegal**.

---

### **6. Static Methods & Inheritance**

* Static methods can be **inherited** but **not overridden** (method in parent always runs).
* Static interface methods must have a **body** and cannot be inherited.
* **Reason:** Static methods are resolved at compile time (no polymorphism).

  In Java, **static methods cannot truly be overridden** — they can only be **hidden**.
Here’s what happens:

---

### 1️⃣ **Static Method Hiding**

* If a **subclass** defines a static method **with the same signature** as one in its superclass, the subclass **hides** the superclass method.
* The method that gets executed depends on the **reference type**, not the object type.

Example:

```java
class Parent {
    static void show() {
        System.out.println("Parent static method");
    }
}

class Child extends Parent {
    static void show() { // method hiding
        System.out.println("Child static method");
    }
}

public class Main {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show(); // Output: Parent static method

        Child c = new Child();
        c.show(); // Output: Child static method
    }
}
```

---

### 2️⃣ **Why No Polymorphism for Static Methods?**

* **Polymorphism** works only with **instance methods** because they are resolved **at runtime** (dynamic binding).
* **Static methods** are resolved **at compile time** (static binding).
* That’s why method hiding is chosen over overriding.

---

### 3️⃣ **Key Points**

* **Overriding** → Allowed only for **instance methods**.
* **Hiding** → Happens when a static method in a subclass has the same signature as in a superclass.
* The **method call** depends on **reference type**, not the actual object.
* `@Override` annotation **cannot** be used for static methods — it will give a compile-time error.

---


---

### **7. Polymorphism Limitation**

* Polymorphism applies to **methods**, **not** to instance variables.

```java
class A { int x = 10; }
class B extends A { int x = 20; }
A obj = new B();
System.out.println(obj.x); // 10, not 20
```

---

### **8. Quick Example Combining All Concepts**

```java
class Box {
    private double width, height, depth;
    Box(double w, double h, double d) {
        width = w; height = h; depth = d;
    }
    Box(Box ob) {
        width = ob.width;
        height = ob.height;
        depth = ob.depth;
    }
}

class BoxWeight extends Box {
    double weight;
    BoxWeight(double w, double h, double d, double m) {
        super(w, h, d);
        weight = m;
    }
    BoxWeight(BoxWeight ob) {
        super(ob);
        weight = ob.weight;
    }
}

final class SecureBox extends BoxWeight {
    SecureBox(double w, double h, double d, double m) {
        super(w, h, d, m);
    }
}

public class Main {
    public static void main(String[] args) {
        BoxWeight bw = new BoxWeight(2, 3, 4, 5);
        Box b = bw; // superclass ref to subclass object
        System.out.println(((BoxWeight)b).weight); // access after casting
    }
}
```

---

This arrangement makes it very clear: **what inheritance is, how `super` works, how `final` fits in, static method rules, and pitfalls** — without losing a single detail from your original text.

If you want, I can now also prepare a **visual diagram** of the inheritance + `super()` calling chain so it’s even easier to grasp. That would fit really well here.


Here’s a **clear and structured breakdown** of **types of inheritance** in Java — including the rules, diagrams, and key points you should know for interviews.

---

## **Types of Inheritance in Java**

Java supports **different types of inheritance** based on how classes and interfaces relate to each other.

### **1. Single Inheritance**

* **Definition:** A class inherits from only one superclass.
* **Example:**

```java
class Parent {
    void display() {
        System.out.println("Parent method");
    }
}

class Child extends Parent {
    void show() {
        System.out.println("Child method");
    }
}

public class Test {
    public static void main(String[] args) {
        Child c = new Child();
        c.display();
        c.show();
    }
}
```

* **Diagram:**

```
Parent → Child
```

---

### **2. Multilevel Inheritance**

* **Definition:** A class is derived from another class, which is also derived from another class.
* **Example:**

```java
class GrandParent {
    void method1() { System.out.println("GrandParent"); }
}

class Parent extends GrandParent {
    void method2() { System.out.println("Parent"); }
}

class Child extends Parent {
    void method3() { System.out.println("Child"); }
}
```

* **Diagram:**

```
GrandParent → Parent → Child
```

---

### **3. Hierarchical Inheritance**

* **Definition:** Multiple classes inherit from the same parent class.
* **Example:**

```java
class Parent {
    void display() { System.out.println("Parent method"); }
}

class Child1 extends Parent { }
class Child2 extends Parent { }
```

* **Diagram:**

```
       Parent
       /   \
  Child1   Child2
```

---

### **4. Multiple Inheritance (through Interfaces)**

* \*\*Java does not support multiple inheritance with **classes** to avoid the **diamond problem**,
  but it supports it through **interfaces**.
* **Example:**

```java
interface A {
    void methodA();
}

interface B {
    void methodB();
}

class C implements A, B {
    public void methodA() { System.out.println("From A"); }
    public void methodB() { System.out.println("From B"); }
}
```

* **Diagram:**

```
A     B
 \   /
   C
```

---

### **5. Hybrid Inheritance** (Combination)

* **Definition:** Combination of two or more types of inheritance.
* **Java supports hybrid inheritance only through interfaces.**
* **Example:**

```java
interface A { void methodA(); }
interface B extends A { void methodB(); }
interface C { void methodC(); }

class D implements B, C {
    public void methodA() { System.out.println("A"); }
    public void methodB() { System.out.println("B"); }
    public void methodC() { System.out.println("C"); }
}
```

---

✅ **Key Points to Remember**

* **Class Inheritance:** Only **single** or **multilevel** allowed.
* **Interface Inheritance:** Multiple & hybrid are allowed.
* **Reason multiple class inheritance is not allowed:** To prevent **ambiguity** in method resolution (diamond problem).

---
