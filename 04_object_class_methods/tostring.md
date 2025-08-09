## **`toString()` in Java — Overriding vs Overloading**

### 1️⃣ **What is `toString()`?**

* Defined in **`java.lang.Object`**.
* By default it is public
* Reducing the visibility will cause compile time error
* Returns a string representation of the object.
* Default implementation:

  ```java
  getClass().getName() + "@" + Integer.toHexString(hashCode())
  ```

---

### 2️⃣ **Overriding `toString()`**

* Change **how** the object is represented as a String.
* **Same** method signature as in `Object`:

  ```java
  public String toString()
  ```
* Use `@Override` for safety.

**Example:**

```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Sahi", 22);
        System.out.println(p); // Calls overridden toString()
    }
}
```

**Output:**

```
Person{name='Sahi', age=22}
```

---

### 3️⃣ **Overloading `toString()`**

* Same method **name** but **different parameters**.
* This does **not** replace the default `toString()` — it’s just another method.

**Example:**

```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Overridden
    @Override
    public String toString() {
        return "Person{name='" + name + "', age=" + age + "}";
    }

    // Overloaded
    public String toString(String prefix) {
        return prefix + " -> " + name + ", " + age;
    }
}

public class Main {
    public static void main(String[] args) {
        Person p = new Person("Sahi", 22);

        System.out.println(p);                  // overridden toString()
        System.out.println(p.toString("Info")); // overloaded toString(String)
    }
}
```

**Output:**

```
Person{name='Sahi', age=22}
Info -> Sahi, 22
```

---

### 4️⃣ **Compile-Time Error When Misusing `@Override`**

If you try to **overload** but mistakenly use `@Override`, you’ll get an error because method signature doesn’t match the original.

**Example:**

```java
class Person {
    String name;
    int age;

    Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // ❌ Wrong - This is overloading, not overriding
    @Override
    public String toString(String prefix) { // different parameter
        return prefix + " -> " + name + ", " + age;
    }
}
```

**Error:**

```
method does not override or implement a method from a supertype
```

---

✅ **Key Takeaways**

| Feature     | Overriding `toString()`              | Overloading `toString()`                  |
| ----------- | ------------------------------------ | ----------------------------------------- |
| Signature   | `public String toString()`           | `public String toString(<parameters>)`    |
| Purpose     | Change default string representation | Provide extra variations for custom needs |
| `@Override` | ✔️ Allowed (recommended)             | ❌ Causes compile-time error               |
| Called by   | `System.out.println(obj)`            | Must be called manually with parameters   |

---
