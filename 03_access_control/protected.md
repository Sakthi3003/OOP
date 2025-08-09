
## 3️⃣ `protected` Access Modifier 🛡️

---

### **Definition**

* The `protected` access modifier allows access to members (variables, methods, constructors) **within the same package** **and** also **to subclasses** even if they are in a **different package**.
* So, `protected` sits between **default (package-private)** and **public** in terms of visibility.

---

### **Access Scope**

| Location                          | Access to `protected` member? |
| --------------------------------- | ----------------------------- |
| Same class                        | ✅ Yes                         |
| Same package (different class)    | ✅ Yes                         |
| Subclass in **same package**      | ✅ Yes                         |
| Subclass in **different package** | ✅ Yes                         |
| Non-subclass in different package | ❌ No                          |

---

### **Key Characteristics**

* Allows subclasses outside the package to access superclass members marked `protected`.
* Non-subclass code outside the package **cannot access** protected members.
* Useful for designing class hierarchies where subclasses need to use or override certain members, but you don’t want those members to be publicly exposed.

---

### **Example**

```java
// File: package1/Base.java
package package1;

public class Base {
    protected void display() {
        System.out.println("Protected display method in Base");
    }
}

// File: package1/SamePackageClass.java
package package1;

public class SamePackageClass {
    public void test() {
        Base b = new Base();
        b.display();  // ✅ Allowed - same package access
    }
}

// File: package2/SubClass.java
package package2;
import package1.Base;

public class SubClass extends Base {
    public void test() {
        display();  // ✅ Allowed - subclass can access protected member even in different package

        Base b = new Base();
        // b.display();  // ❌ Not allowed - protected not accessible through superclass reference outside package
    }
}

// File: package2/NonSubClass.java
package package2;
import package1.Base;

public class NonSubClass {
    public void test() {
        Base b = new Base();
        // b.display();  // ❌ Not allowed - not a subclass, different package
    }
}
```

---

### **Why does this matter?**

* When a subclass inherits from a superclass, it needs to access and possibly override `protected` members.
* This ensures **encapsulation** within the package but still provides flexibility for class extensions outside the package.
* It prevents external classes (not subclasses) from accessing sensitive or internal members that should only be used within the inheritance hierarchy.

---

### **Important Details**

* The `protected` keyword **also applies to constructors and inner classes**.
* When accessing a protected member from a subclass in a different package, you must access it through the subclass or its instances, **not through superclass instances**.
* This rule prevents unrelated classes in other packages from accessing protected members on objects of the superclass.

---

### **Summary**

| Modifier  | Same Class | Same Package | Subclass (diff pkg) | Other packages |
| --------- | ---------- | ------------ | ------------------- | -------------- |
| protected | Yes        | Yes          | Yes                 | No             |

---
