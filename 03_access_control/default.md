## **2️⃣ Default (Package-Private) Access Modifier**

In Java, when you **do not** explicitly specify an access modifier for a class member (variable, method, or constructor), Java automatically assigns it **default access**.

### **Key Rule**

* **Accessible only within the same package** — other classes in **the same package** can access it, regardless of whether they’re related by inheritance.
* **Not accessible from outside the package**, even if the class is a subclass.

---

### **How It Works**

* **Scope**: Package-level
* This is useful when you want your methods/variables to be shared across related classes in the same package, but hidden from unrelated code in other packages.

---

### **Example**

```java
// File: package1/Example.java
package package1;

public class Example {
    int num = 10;  // default access
    void display() {  // default access
        System.out.println("Number: " + num);
    }
}
```

```java
// File: package1/SamePackageTest.java
package package1;

public class SamePackageTest {
    public static void main(String[] args) {
        Example ex = new Example();
        System.out.println(ex.num); // ✅ Works (same package)
        ex.display(); // ✅ Works
    }
}
```

```java
// File: package2/DifferentPackageTest.java
package package2;
import package1.Example;

public class DifferentPackageTest {
    public static void main(String[] args) {
        Example ex = new Example();
        // System.out.println(ex.num); ❌ Compile Error
        // ex.display(); ❌ Compile Error
    }
}
```

---

### **When to Use Default Access**

* When you have a group of **closely related classes** in the same package.
* When you don’t want to expose internal workings to other packages.
* Example: Utility classes within a package can interact internally without public exposure.

---

### **Important Points**

1. **No keyword for default** — you simply omit the access modifier.
2. Can be applied to:

   * Instance variables
   * Methods
   * Constructors
   * Inner classes
3. **Cannot** be applied to **top-level interfaces** (they’re always `public`).
4. Default access **is package-level**; inheritance does not override package rules.
