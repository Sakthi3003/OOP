## **Method Overloading in Java**

**Definition**
In Java, you can define **two or more methods with the same name** in the same class as long as their **parameter lists differ** (different number of parameters or different types).
This is called **method overloading**.

---

### **Key Rules**

1. **Same method name** but **different parameter list** (number, order, or type).
2. **Return type alone cannot distinguish methods** — parameter list must be different.
3. Can have different **access modifiers**.
4. Can have different **exception lists**.
5. **Overloading happens at compile time** → This is **compile-time polymorphism**.

---

### **Example 1 — Parameter Difference**

```java
class MathOps {
    void add(int a, int b) {
        System.out.println("Sum of ints: " + (a + b));
    }
    void add(double a, double b) {
        System.out.println("Sum of doubles: " + (a + b));
    }
}

class Test {
    public static void main(String[] args) {
        MathOps m = new MathOps();
        m.add(3, 4);        // calls add(int, int)
        m.add(3.5, 4.5);    // calls add(double, double)
    }
}
```

---

### **Example 2 — Automatic Type Conversion**

If no exact match is found, Java tries **widening conversion** (e.g., `int` → `long` → `float` → `double`).

```java
class OverloadDemo {
    void test(double a) {
        System.out.println("Inside test(double) a: " + a);
    }
}

class Overload {
    public static void main(String[] args) {
        OverloadDemo ob = new OverloadDemo();
        int i = 88;
        ob.test(i);       // int → double, calls test(double)
        ob.test(123.2);   // exact match for double
    }
}
```

**Flow here**:

1. Compiler looks for `test(int)` — not found.
2. Tries widening: `int → double`.
3. Calls `test(double)`.

If `test(int)` existed, it would have been chosen instead — **exact match wins**.

---

### **Why is overloading compile-time polymorphism?**

Because **method selection** is decided by the compiler based on the **reference type and argument list**, not at runtime.

---

## **Returning Objects from Methods**

### **Definition**

A method in Java can return an **object reference**.
Since objects are created on the heap using `new`, they persist after the method finishes — until no references point to them (then garbage collected).

---

### **Example**

```java
class Test {
    int a;

    Test(int i) {
        a = i;
    }

    Test incrByTen() {
        Test temp = new Test(a + 10);
        return temp;
    }
}

class RetOb {
    public static void main(String[] args) {
        Test ob1 = new Test(2);
        Test ob2;

        ob2 = ob1.incrByTen();  // returns a new Test object

        System.out.println("ob1.a: " + ob1.a); // 2
        System.out.println("ob2.a: " + ob2.a); // 12
    }
}
```

---

### **Key Points**

* **Heap allocation**: Objects created with `new` live until no references point to them.
* **Garbage collection**: Automatically reclaims memory when an object is unreachable.
* Returning objects is safe — they **don’t vanish** when the method ends, unlike local variables.
* Common use case: **Factory methods** that create and return new objects.

---

## **1. Normal Inheritance (No Overriding)**

Here, there’s **no method overriding**, so the compiler already knows exactly which method to call based on the **reference type**.

**Example:**

```java
class Parent {
    void show() {
        System.out.println("Parent show");
    }
}

class Child extends Parent {
    void display() {
        System.out.println("Child display");
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show();    // ✅ Allowed - chosen based on Parent reference type
        // p.display(); // ❌ Not allowed - Parent reference has no display() method
    }
}
```

📌 **Why reference type matters here:**
The compiler checks **at compile-time** what methods are available using the **reference type** (Parent).
Since `display()` is not in `Parent`, you get a compile error.

---

## **2. Polymorphism (Method Overriding)**

When a **method is overridden** in the child class, Java uses **Dynamic Method Dispatch** — which means the method that runs depends on the **actual object type** at runtime, **not** the reference type.

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

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        p.show(); // OUTPUT: "Child show"
    }
}
```

📌 **Why object type matters here:**
At **runtime**, the JVM looks at the actual object (`Child`) and calls its overridden `show()` method, even though the reference type is `Parent`.

---

### **Core Difference**

| Feature               | Normal Inheritance (No Override)   | Polymorphism (Override)                                                             |
| --------------------- | ---------------------------------- | ----------------------------------------------------------------------------------- |
| Method Selection Time | **Compile-time**                   | **Runtime**                                                                         |
| Depends On            | **Reference type**                 | **Object type**                                                                     |
| Allowed Method Calls  | Only methods in the reference type | Only methods in the reference type (but overridden behavior comes from object type) |
| Example               | `p.show()` (fixed at compile time) | `p.show()` (decided at runtime)                                                     |

---

💡 **Why?**

* In normal inheritance without overriding → Method binding is **static binding** (compile-time).
* In polymorphism with overriding → Method binding is **dynamic binding** (runtime).

---

