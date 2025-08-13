# ☕ Java: Classes and Objects

---

## ✅ Step 1: Storing Data Without a Class

Let’s say you want to store **5 numbers**:

```java
int[] arr = new int[5];
```

Easy! But now…

## 🧑‍🎓 What if you want to store **Student data**?

Every student has:

* A **name**
* A **roll number**
* Marks

So instead of storing 3 different arrays, it’s better to make a **class**.

---

## 🔷 What is a Class?

👉 A **Class** is like a **blueprint** or **custom data type**.

> 💬 Think of it like a design of a car:
> You can design one car, and make hundreds of cars from that design.

```java
class Student {
    String name;
    int roll;
    int marks;
}
```

---

## 🧱 What is an Object?

👉 An **Object** is a **real thing** made from that class.

```java
Student s1 = new Student(); // Object created
```

---

### 🔍 Breakdown:

| Term         | Meaning                        |
| ------------ | ------------------------------ |
| **State**    | The values (name, roll, marks) |
| **Identity** | Where it is stored in memory   |
| **Behavior** | What it can do (methods)       |

---

### 🎯 Instance Variable

```java
s1.name = "Sahi";
s1.roll = 101;
s1.marks = 90;
```

* `s1` is the reference variable.
* `s1.name` is how we **access instance variables**.
* All data saved inside the object.

---

## ⚙️ Object Creation – Compile Time vs Runtime

```java
Student s = new Student();  // 👈 NEW keyword is used
```

* **Compile Time**: Java checks if class exists.
* **Runtime**: Object is actually created with `new`.

> ✅ `new` keyword = dynamic memory allocation
> (Memory is given in **heap** at runtime)

---

## 🎯 Primitives vs Objects

| Type                          | Stored In | Default Value |
| ----------------------------- | --------- | ------------- |
| `int`, `double`, `boolean`    | Stack     | 0, 0.0, false |
| Object types (like `Student`) | Heap      | `null`        |

---
