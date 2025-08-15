## 🔁 Stack vs Heap Memory (Simplified)

* Primitives (`int`, `double`) → Stored in **Stack**
* Objects (`Student`, `String`) → Stored in **Heap**
* Stack is **faster**. That’s why primitives are not stored in Heap.

---

# 🔁 Shallow Copy vs Deep Copy vs Clone

### 🔹 Shallow Copy

* Just **copies the reference**
* Both variables point to **same object**

```java
Student s1 = new Student("Sahi", 101, 90);
Student s2 = s1; // Shallow copy
s2.name = "Changed";
System.out.println(s1.name); // Output: Changed ✅
```

---

### 🔹 Deep Copy

* You copy **everything separately**

```java
Student s1 = new Student("Sahi", 101, 90);
Student s2 = new Student(s1.name, s1.roll, s1.marks);
s2.name = "Changed";
System.out.println(s1.name); // Output: Sahi ✅
```

---

### 🔹 Cloning

* Clone means copying the object with the help of `.clone()`
* Class must implement `Cloneable`

```java
class Student implements Cloneable {
    int roll;

    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}
```

---

# 🔐 `final` Keyword in Java

| Usage            | Meaning                                          |
| ---------------- | ------------------------------------------------ |
| `final variable` | You **cannot change** the value after assignment |
| `final method`   | Cannot override in a subclass                    |
| `final class`    | Cannot be extended (like `String`, `Math`)       |

```java
final int x = 10;
// x = 20; ❌ Error

final class A {}
// class B extends A {} ❌ Error
```

---

## 🔄 Swapping Primitives vs Objects

### Swapping Primitive

```java
int a = 10, b = 20;
int temp = a;
a = b;
b = temp;
// Works fine ✅
```

### Swapping Objects (Won’t work directly)

```java
void swap(Student a, Student b) {
    Student temp = a;
    a = b;
    b = temp;
}

Student s1 = new Student();
Student s2 = new Student();
swap(s1, s2);
// This will not swap outside because Java passes objects **by value of reference**
```

✅ To actually swap their **content**, you must swap fields, not references.

---

# 🧠 Final Summary

| Concept       | Easy Meaning                                               |
| ------------- | ---------------------------------------------------------- |
| Class         | Design or Blueprint                                        |
| Object        | Real-world instance of the class                           |
| Constructor   | Special function that sets values at object creation       |
| `new` keyword | Creates object in heap memory                              |
| Shallow Copy  | Two references to same object                              |
| Deep Copy     | Two separate objects with same values                      |
| `final`       | Means "Can't Change" - for variables, methods, and classes |
| Stack vs Heap | Primitive → Stack, Object → Heap                           |
| Clone         | Creates copy using `.clone()` method                       |

---


FINAL
=====

**final**
When u create a primitive data type with final keyword, immediate assign is must and it will give compile time error if it is not assigned.

If We create a Object with final keyword like 

final student s = new student();
U can add the value but not reassign the value 

s.name="sajthi" poosibe byt not again
s.name - will give runtme eror 

**👉 Yes, you can create an object for a `final` class!**

But...

### ❗ What does `final` mean when used with a class?

When you declare a class as `final`:

```java
final class Animal {
    void sound() {
        System.out.println("Some sound");
    }
}
```

It simply means:

> **No other class can extend (inherit from) `Animal`.**

So this is **❌ NOT allowed**:

```java
class Dog extends Animal {  // ❌ ERROR: Cannot inherit from final class
}
```

But this is **✅ allowed**:

```java
public class Main {
    public static void main(String[] args) {
        Animal a = new Animal();  // ✅ YES! You can create object
        a.sound();
    }
}
```

---

### 📌 Summary:

| Use of `final`        | What it does                            | Can you create object?     |
| --------------------- | --------------------------------------- | -------------------------- |
| `final` with class    | Prevents inheritance                    | ✅ Yes                      |
| `final` with method   | Prevents method overriding              | ✅ Yes                      |
| `final` with variable | Makes it a constant (cannot be changed) | ✅ Yes, but cannot reassign |

---


CLARIFICATION
==============
Great question! Let's break it down clearly and correctly.

---

### ✅ `final` with **object references** (like `final Student s`):

```java
final Student s = new Student();
```

* ✅ You **can modify the internal state** of the object (change values of fields):

  ```java
  s.name = "Sahi";  // ✅ Allowed
  ```

* ❌ But you **cannot reassign** the reference to point to another object:

  ```java
  s = new Student();  // ❌ Compile-time error: cannot assign a value to final variable 's'
  ```

---

### 💡 Why?

* `final` means **the reference variable `s` cannot point to a new object** after it's been initialized.
* But the **object itself is still mutable**, unless its class is made immutable (like `String`, or custom immutable classes).

---

### 💥 What happens if you reassign?

You get a **compile-time error**:

```java
final Student s = new Student();
s = new Student(); // ❌ ERROR: cannot assign a value to final variable 's'
```

---

### ✅ But you can do this:

```java
s.name = "Sahi";   // Allowed – you're modifying the contents of the object, not the reference itself
```

---

### 🧠 Summary

| Operation               | Allowed? | Reason                           |
| ----------------------- | -------- | -------------------------------- |
| `final Student s = ...` | ✅        | Final reference assigned once    |
| `s = new Student();`    | ❌        | Can't reassign a final reference |
| `s.name = "Sahi";`      | ✅        | Mutating internal state is fine  |
Sure! Here's your **exact same content**, fully **rewritten with proper formatting, spacing, and code blocks** for clarity — nothing is changed in the explanation or examples, just made easier to read and more structured.

---

# ✅ KUNAL NOTES

---

## 🔹 Class and Object Basics

A class is a template for an object, and an object is an instance of a class.
A class creates a new data type that can be used to create objects.

When you declare an object of a class, you are creating an instance of that class.
Thus, a class is a logical construct. An object has physical reality.
(That is, an object occupies space in memory.)

Objects are characterized by three essential properties:

* **State**

* **Identity**

* **Behavior**

* The **state** of an object is a value from its data type.

* The **identity** of an object distinguishes one object from another.
  It is useful to think of an object’s identity as the place where its value is stored in memory.

* The **behavior** of an object is the effect of data-type operations.

---

## 🔸 Dot Operator (`.`)

The dot operator links the name of the object with the name of an instance variable.
Although commonly referred to as the dot operator, the formal specification for Java categorizes the `.` as a **separator**.

---

## 🔸 `new` Keyword

The `new` keyword dynamically allocates (that is, allocates at run time) memory for an object & returns a reference to it.
This reference is, more or less, the address in memory of the object allocated by `new`.
This reference is then stored in the variable.
Thus, in Java, all class objects must be dynamically allocated.

```java
Box mybox;          // declare reference to object
mybox = new Box();  // allocate a Box object
```

* The first line declares `mybox` as a reference to an object of type `Box`.
* At this point, `mybox` does not yet refer to an actual object.
* The next line allocates an object and assigns a reference to it to `mybox`.
* After the second line executes, you can use `mybox` as if it were a `Box` object.
  But in reality, `mybox` simply holds, in essence, the memory address of the actual `Box` object.

> ☑️ The key to Java’s safety is that you **cannot manipulate references** as you can actual pointers.

Thus, you **cannot cause an object reference to point to an arbitrary memory location** or manipulate it like an integer.

---

## 🔸 A Closer Look at `new`

```java
classname class-var = new classname();
```

* `class-var` is a variable of the class type being created.
* `classname` is the name of the class being instantiated.
* The class name followed by parentheses specifies the **constructor** for the class.

> A constructor defines what occurs when an object of a class is created.

---

## ❓ Why not use `new` for primitives?

You might be wondering why you do not need to use `new` for such things as integers or characters.
The answer is that Java’s **primitive types are not implemented as objects**.
Rather, they are implemented as “normal” variables.
This is done in the interest of **efficiency**.

---

## 📌 Reference Assignment

```java
Box b1 = new Box();
Box b2 = b1;
```

* `b1` and `b2` will both refer to the **same object**.
* The assignment of `b1` to `b2` **did not allocate any memory** or copy any part of the original object.
  It simply makes `b2` refer to the **same object** as `b1`.

✅ Thus, any changes made to the object through `b2` will affect the object to which `b1` is referring.

> When you assign one object reference variable to another, you are **not creating a copy** of the object, only a copy of the reference.

---

## 🔸 Method Parameters and Arguments

```java
int square(int i){
    return i * i;
}
```

* A **parameter** is a variable defined by a method that receives a value when the method is called.
  For example, in `square(int i)`, `i` is a parameter.

* An **argument** is a value that is passed to a method when it is invoked.
  For example, `square(100)` passes `100` as an argument.
  Inside `square()`, the parameter `i` receives that value.

---

## 📝 NOTE:

```java
Bus bus = new Bus();
```

* LHS (reference i.e. `bus`) is looked by **compiler**
* RHS (object i.e. `new Bus()`) is looked by **JVM**

---

## 🔹 `this` Keyword

Sometimes a method will need to refer to the object that invoked it.
To allow this, Java defines the `this` keyword.

* `this` can be used inside any method to refer to the **current object**.
* That is, `this` is always a reference to the object on which the method was invoked.

---

## 🔹 `final` Keyword

* A field can be declared as `final`.
  Doing so **prevents its contents from being modified**, making it essentially a constant.
  This means that you **must initialize** a `final` field when it is declared.

```java
final int FILE_OPEN = 2;
```

> It is a common coding convention to choose all **uppercase identifiers** for final fields.

### ⚠️ Important Note:

`final` guarantees immutability only when instance variables are **primitive types**, not reference types.

If a reference type has the `final` modifier:

* The **reference** to an object **will never change**.
* But the **value** of the object itself **can change**.

---

## 🔹 The `finalize()` Method

Sometimes an object will need to perform some action when it is destroyed.
To handle such situations, Java provides a mechanism called **finalization**.

By using finalization, you can define specific actions that will occur when an object is about to be **reclaimed by the garbage collector**.

```java
protected void finalize() {
    // finalization code here
}
```

* The Java run time calls this method whenever it is about to recycle an object of that class.

---

## 🔹 Constructors

Once defined, the **constructor** is automatically called when the object is created, **before** the `new` operator completes.

* Constructors have **no return type**, not even `void`.
* This is because the **implicit return type** of a class’ constructor is the class type itself.

```java
Box mybox1 = new Box();  // new Box() is calling the Box() constructor
```

---

## 🔸 Inheritance and Constructors in Java

In Java, **constructor of base class with no argument gets automatically called** in derived class constructor.

### Example:

**Output:**

```
Base Class Constructor Called
Derived Class Constructor Called
```

```java
// filename: Main.java
class Base {
  Base() {
    System.out.println("Base Class Constructor Called ");
  }
}

class Derived extends Base {
  Derived() {
    System.out.println("Derived Class Constructor Called ");
  }
}

public class Main {
  public static void main(String[] args) {
    Derived d = new Derived();
  }
}
```

---

* Any class will have a **default constructor**, whether we declare it or not.
* If we **inherit a class**, the derived class must call its superclass constructor.
* It is done **by default** in the derived class.

If the derived class does not have a constructor, **JVM will invoke the default constructor** and call the superclass constructor.

If we have a **parameterized constructor** in the derived class, still it **calls the default superclass constructor** by default.

> If the superclass does **not have a default constructor**, and only a parameterized one,
> then the derived class constructor should **explicitly call** the superclass constructor.

