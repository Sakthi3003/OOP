Alright — I’ll give you **full, in-depth notes** that combine everything you mentioned:

* Overriding rules
* What happens when a method is *not* overridden
* Dynamic Method Dispatch
* Special case: `static` methods
* Covariant return types
* Overloading vs overriding
* Example walkthroughs

---

## **Method Overriding – Complete Notes**

---

### **Definition**

Method overriding happens when **a subclass provides its own implementation** of a method that is **already defined** in its superclass **with the same method name, parameters, and return type (or covariant return type)**.

**Example:**

```java
class Parent {
    void display() {
        System.out.println("Parent display");
    }
}

class Child extends Parent {
    @Override
    void display() {
        System.out.println("Child display");
    }
}
```

---

### **Rules for Overriding**

1. **Method name & parameter list must be identical** to the superclass method.

   * If different → it’s **overloading**, not overriding.
2. **Return type must be the same** (or a *covariant* type — explained later).
3. **Access level** cannot be more restrictive than the overridden method.
4. **Only inherited methods** can be overridden.
5. **`final` methods cannot be overridden**.
6. **`static` methods cannot be overridden** (but can be hidden).
7. **Private methods** are not inherited → cannot be overridden.
8. **Constructors** cannot be overridden.

---

### **When a method is NOT overridden**

If a subclass does **not** provide its own implementation, the inherited method from the superclass is used, even if the reference points to a child object.

**Example:**

```java
class Parent {
    void area() {
        System.out.println("Parent area");
    }
}

class Child extends Parent {
    // No override
}

public class Test {
    public static void main(String[] args) {
        Parent obj = new Child(); // Upcasting
        obj.area(); // Parent's version runs
    }
}
```

**Output:**

```
Parent area
```

---

### **Dynamic Method Dispatch (Run-Time Polymorphism)**

* **Definition:** The process by which a call to an **overridden** method is resolved **at runtime**, based on the *actual object type*, not the reference type.
* **How:**

  * At compile time → compiler checks **reference type** for method existence.
  * At runtime → JVM looks up the method in the object's **vtable** and calls the actual implementation.

**Example:**

```java
class Parent {
    void show() {
        System.out.println("Parent show");
    }
}

class Child extends Parent {
    @Override
    void show() {
        System.out.println("Child show");
    }
}

public class Demo {
    public static void main(String[] args) {
        Parent ref = new Child(); // upcasting
        ref.show(); // Runtime: calls Child's method
    }
}
```

**Output:**

```
Child show
```

---

### **Covariant Return Types**

* Introduced in Java 5.
* An overridden method can return a **subtype** of the return type declared in the superclass.

**Example:**

```java
class Animal {}
class Dog extends Animal {}

class Parent {
    Animal getAnimal() {
        return new Animal();
    }
}

class Child extends Parent {
    @Override
    Dog getAnimal() { // Covariant return type
        return new Dog();
    }
}
```

---

### **Overloading vs Overriding**

| Feature         | Overloading                  | Overriding                         |
| --------------- | ---------------------------- | ---------------------------------- |
| Parameters      | Must be different            | Must be same                       |
| Return type     | Can be different             | Same or covariant                  |
| Access modifier | Can change freely            | Cannot reduce visibility           |
| Static methods  | Can be overloaded            | Cannot be overridden (only hidden) |
| Binding         | Compile-time (early binding) | Runtime (late binding)             |

---

### **Static Methods – Hiding vs Overriding**

* **Static methods** belong to the class, not the object.
* They are **not polymorphic** — method calls are resolved at **compile time**.
* If a subclass defines a static method with the same signature, it **hides** the superclass version, but which one gets called depends on the **reference type**, not the object type.

**Example:**

```java
class Parent {
    static void display() {
        System.out.println("Parent static");
    }
}

class Child extends Parent {
    static void display() {
        System.out.println("Child static");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.display(); // Parent static (compile-time binding)
    }
}
```

---

### **Vtable in Method Dispatch**

* **Vtable (Virtual Method Table):**
  A table maintained by JVM for each class to store addresses of overridden methods.
* When you call a method, JVM uses the vtable of the actual object's class to find the method to execute.

---

### **Important Points Recap**

* **Instance methods** → polymorphic (dynamic dispatch).
* **Static, private, final methods** → not polymorphic.
* If no override → parent version runs even for child objects (when upcast).
* **Covariant returns** allow more specific return types in overrides.
* **Dynamic Method Dispatch** is the key to runtime polymorphism.

---

### **Quick Real-Life Example**

```java
class Shape {
    void draw() { System.out.println("Drawing shape"); }
}

class Circle extends Shape {
    @Override
    void draw() { System.out.println("Drawing circle"); }
}

class Square extends Shape {
    @Override
    void draw() { System.out.println("Drawing square"); }
}

public class TestShapes {
    public static void main(String[] args) {
        Shape s;

        s = new Circle(); // upcasting
        s.draw(); // Drawing circle

        s = new Square();
        s.draw(); // Drawing square
    }
}
```

---

