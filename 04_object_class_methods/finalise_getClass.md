# 1. `getClass()` Method

### Purpose:

* Returns the runtime **Class object** that represents the actual class of the object.
* Used for **reflection**, **type checking**, or to get class metadata at runtime.

### Signature:

```java
public final Class<?> getClass()
```

### Key Points:

* The method is `final` — it **cannot be overridden**.
* Every object knows its exact runtime class.
* Useful for debugging, logging, or implementing generic behaviors based on class type.

### Example:

```java
class Animal {}

class Dog extends Animal {}

public class TestGetClass {
    public static void main(String[] args) {
        Animal a = new Dog();

        System.out.println("Runtime class: " + a.getClass().getName());
        // Output: Runtime class: Dog

        // Checking exact type
        if (a.getClass() == Dog.class) {
            System.out.println("Object is a Dog");
        } else {
            System.out.println("Object is NOT a Dog");
        }
    }
}
```

### Use Cases:

* To check the exact class of an object (not just its declared reference type).
* In debugging or logging to know which class instance is involved.
* Reflection API uses `getClass()` to inspect methods, fields, constructors of the class at runtime.

---

# 2. `finalize()` Method

### Purpose:

* Called by the **Garbage Collector (GC)** on an object when it determines that there are no more references to the object.
* Allows the object to clean up resources before being removed from memory.

### Signature:

```java
protected void finalize() throws Throwable
```

### Important Notes:

* It is `protected` and can be overridden by subclasses.
* Only called **once** by the JVM before garbage collection.
* **Deprecated since Java 9** due to unpredictability, performance issues, and alternatives like `try-with-resources` and explicit resource management.
* JVM **does not guarantee** when or even if `finalize()` will be called.
* Overriding `finalize()` is generally discouraged.

### Example:

```java
class Resource {
    @Override
    protected void finalize() throws Throwable {
        try {
            System.out.println("finalize called to clean up resource");
            // Close files, release resources, etc.
        } finally {
            super.finalize(); // Always call superclass finalize
        }
    }
}

public class TestFinalize {
    public static void main(String[] args) {
        Resource res = new Resource();
        res = null; // Eligible for garbage collection

        System.gc(); // Request GC (not guaranteed to run immediately)

        System.out.println("End of main method");
    }
}
```

### What happens:

* When the GC runs, it may call `finalize()` on `res` before reclaiming its memory.
* Output may include `"finalize called to clean up resource"`, but no guarantees on timing or execution.

### Why `finalize()` is discouraged:

* **Unpredictable:** You cannot rely on `finalize()` to execute promptly or at all.
* **Performance:** Adds overhead to GC.
* **Better alternatives:** Use explicit cleanup methods or `AutoCloseable` with try-with-resources.

---

### Summary

| Method       | Purpose                        | Overridable? | Usage Notes                                               |
| ------------ | ------------------------------ | ------------ | --------------------------------------------------------- |
| `getClass()` | Get runtime class info         | No           | Useful for reflection, debugging, and type checks         |
| `finalize()` | Cleanup before GC (deprecated) | Yes          | Avoid using; unreliable and replaced by better techniques |

---
