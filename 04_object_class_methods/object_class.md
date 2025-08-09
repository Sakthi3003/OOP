# Java `Object` Class — In Depth

---

## What is the `Object` Class?

- The `Object` class is the **root superclass** of all Java classes.
- Every class you create implicitly extends `java.lang.Object` if you do not explicitly extend another class.
- This means **every object in Java inherits the methods defined in `Object`**.

---

## Why is `Object` Important?

- It provides a **common interface** for all objects.
- Defines core methods that are **available to every object** like `toString()`, `equals()`, and `hashCode()`.
- Enables Java features like polymorphism and collections to work uniformly.
- Allows references of type `Object` to hold any Java object.

---

## Package

- The `Object` class is in the package: `java.lang`.
- This package is imported by default, so you never need to import it explicitly.

---

## Key Characteristics

- `Object` is a **concrete class** — it can be instantiated (though rarely directly used).
- You typically **override its methods** in your own classes to provide class-specific behavior.
- Provides the foundation for many important Java concepts like cloning, synchronization, and garbage collection hooks.

---

## How It Fits Into Java

- Any variable of type `Object` can hold a reference to any class instance.
- Useful in generic programming or APIs which work with unknown types.
- Serves as the base for Java's reflection and serialization mechanisms.

---

## Example Usage

```java
public class MyClass {
    // MyClass implicitly extends Object
}

public class Test {
    public static void main(String[] args) {
        MyClass obj = new MyClass();

        // All objects can be assigned to Object type
        Object reference = obj;

        // Object class method calls are available
        System.out.println(reference.toString());  // prints something like MyClass@1b6d3586
    }
}
```
| Point         | Detail                                                                   |
| ------------- | ------------------------------------------------------------------------ |
| Class Name    | `java.lang.Object`                                                       |
| Role          | Root superclass of all Java classes                                      |
| Package       | `java.lang` (imported by default)                                        |
| Inheritance   | Every class implicitly extends `Object`                                  |
| Purpose       | Provide common methods to all objects                                    |
| Typical Usage | Override its methods like `toString()`, `equals()` to customize behavior |

### **Methods**


| Method Signature                                             | Purpose & Summary                                                                                                               |
| ------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------- |
| `public String toString()`                                   | Returns a string representation of the object. Default: `ClassName@HashCode` — often overridden to provide meaningful info.     |
| `public boolean equals(Object obj)`                          | Compares this object to another for logical equality. Default: compares references (`==`). Override to compare contents.        |
| `public int hashCode()`                                      | Returns an integer hash code for the object. Must be consistent with `equals()`. Used in hash-based collections like `HashMap`. |
| `protected Object clone() throws CloneNotSupportedException` | Creates and returns a copy of the object. Requires implementing `Cloneable` interface.                                          |
| `protected void finalize() throws Throwable`                 | Called by the garbage collector before object is destroyed (deprecated, avoid using).                                           |
| `public final Class<?> getClass()`                           | Returns the runtime class of the object. Useful for reflection.                                                                 |
| `public final void wait()`, `wait(long)`, `wait(long, int)`  | Causes the current thread to wait until notified or timeout expires (used for thread synchronization).                          |
| `public final void notify()`                                 | Wakes up one thread waiting on this object's monitor.                                                                           |
| `public final void notifyAll()`                              | Wakes up all threads waiting on this object's monitor.                                                                          |
