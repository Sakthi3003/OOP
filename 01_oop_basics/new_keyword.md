
## 1️⃣ What is `new` in Java?

* `new` is a **Java keyword** used to **create objects** dynamically at runtime.
* It **allocates memory** for the object in the **heap**.
* It **returns a reference** to that memory.
* It **calls the constructor** of the class to initialize the object.

---

## 2️⃣ Syntax

```java
ClassName obj = new ClassName(arguments);
```

**Example**

```java
Test t = new Test();
```

* `Test` → class name
* `t` → reference variable (on stack)
* `new Test()` → creates object in heap & calls constructor

---

## 3️⃣ Step-by-Step of What Happens When `new` is Used

### Code:

```java
Test t = new Test();
```

### Behind the scenes:

1. **Class loading**

   * If `Test` class is not already loaded, the **Class Loader** loads it into **Metaspace**.
   * Class metadata (methods, static vars, constant pool) stored in Metaspace.

2. **Memory allocation in Heap**

   * JVM allocates enough space for all **instance variables** of `Test`.
   * All fields get **default values** (0, false, null).

3. **Constructor call**

   * `Test()` constructor is invoked to initialize the object.
   * If not explicitly defined, Java calls the **default constructor**.

4. **Reference assignment**

   * `new Test()` returns a **reference** (memory address) to the created object.
   * That reference is stored in variable `t` (in **stack**).

---
