## 4️⃣ `public` Access Modifier 🌍

---

### **Definition**

* The `public` access modifier allows **full access** to the class member (variable, method, constructor, inner class, etc.) from **anywhere** in the program.
* There are **no restrictions** on visibility for `public` members.

---

### **Access Scope**

| Location                              | Access to `public` member? |
| ------------------------------------- | -------------------------- |
| Same class                            | ✅ Yes                      |
| Same package (different class)        | ✅ Yes                      |
| Subclass in same or different package | ✅ Yes                      |
| Non-subclass in different package     | ✅ Yes                      |

---

### **Key Characteristics**

* `public` members are the **most accessible** — can be used by any other code regardless of package or inheritance.
* Use `public` when you want to expose an API, utility methods, or constants.
* Top-level classes can also be declared `public`. The file name must match the public class name.

---

### **Example**

```java
// File: package1/PublicExample.java
package package1;

public class PublicExample {
    public int number = 42;

    public void showNumber() {
        System.out.println("Number: " + number);
    }
}

// File: package2/TestPublic.java
package package2;
import package1.PublicExample;

public class TestPublic {
    public static void main(String[] args) {
        PublicExample obj = new PublicExample();
        System.out.println(obj.number);  // ✅ Accessible
        obj.showNumber();                 // ✅ Accessible
    }
}
```

---

### **When to Use `public`?**

* When you want members or classes to be accessible **from anywhere** in your program or API consumers.
* When designing **interfaces** and **APIs**, the exposed methods will almost always be `public`.
* When your class is a **main entry point** (like the one containing the `public static void main(String[] args)` method).

---

### **Important Points**

* **`public` classes:**

  * The class itself can be declared `public`.
  * If a class is `public`, the filename must exactly match the class name.
* **Methods and variables:**

  * Declared `public` can be accessed everywhere.
* **Security considerations:**

  * Exposing everything as `public` can break encapsulation and expose internals unnecessarily. Use with caution.

---

### **Summary**

| Modifier | Same Class | Same Package | Subclass (diff pkg) | Other packages |
| -------- | ---------- | ------------ | ------------------- | -------------- |
| public   | Yes        | Yes          | Yes                 | Yes            |

---

**To summarize access modifiers hierarchy:**

```plaintext
private < default < protected < public
```

---
