### What is `hashCode()`?

* `hashCode()` is a method defined in the `Object` class.
* It returns an `int` value — the **hash code** — that represents the object.
* The hash code is used primarily by **hash-based collections** like `HashMap`, `HashSet`, and `Hashtable`.
* It helps quickly locate objects in these collections by providing an efficient indexing mechanism.

---

### Why do we need `hashCode()`?

* Collections like `HashMap` and `HashSet` use the hash code to **store** and **retrieve** objects efficiently.
* When you add an object, its hash code determines the bucket/location where it should go.
* When you look up an object, the hash code narrows down the search to a specific bucket.

---

### Relationship Between `equals()` and `hashCode()`

Java’s contract between these two methods is very important:

1. **If two objects are equal according to `equals()`, then they must have the same hash code.**

2. **If two objects have the same hash code, they are not necessarily equal.** (Hash collisions can happen.)

3. **If `equals()` is overridden, then `hashCode()` must also be overridden** to maintain the contract.

---

### What happens if you don’t override `hashCode()` when overriding `equals()`?

* If you override `equals()` but **not** `hashCode()`, objects that are "equal" may return different hash codes.
* This causes inconsistent behavior in hash-based collections.
* For example, you might not be able to find an object in a `HashSet` even if an equal object was added.

---

### Default Behavior of `hashCode()`

* The default implementation in `Object` returns a hash code based on the **memory address** of the object.
* This means each object typically has a unique hash code unless overridden.

---

# How to Override `hashCode()`

Override `hashCode()` to compute a hash code based on the same fields used in `equals()`.

**Example:**

```java
@Override
public int hashCode() {
    int result = 17;
    result = 31 * result + (name == null ? 0 : name.hashCode());
    result = 31 * result + age;
    return result;
}
```

---

### Explanation:

* Start with a non-zero constant (e.g., 17).
* For each significant field:

  * Multiply the current result by a prime number (e.g., 31).
  * Add the hash code of the field.
* Use the same fields as in `equals()`.

---

### Example: `Person` class with `equals()` and `hashCode()`

```java
class Person {
    String name;
    int age;

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Person)) return false;
        Person other = (Person) obj;
        return age == other.age && (name == null ? other.name == null : name.equals(other.name));
    }

    @Override
    public int hashCode() {
        int result = 17;
        result = 31 * result + age;
        result = 31 * result + (name == null ? 0 : name.hashCode());
        return result;
    }
}
```

---

# Summary

| Concept                  | Details                                                       |
| ------------------------ | ------------------------------------------------------------- |
| `hashCode()`             | Returns an int hash value for the object                      |
| Contract with `equals()` | Equal objects must have equal hash codes                      |
| Override together        | If you override `equals()`, always override `hashCode()`      |
| Hash-based collections   | Use `hashCode()` to quickly locate objects in `HashMap`, etc. |

---
