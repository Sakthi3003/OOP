## **1. `private` Access Modifier** 🔒

The **most restrictive** access level in Java.

---

### **Definition**

* A `private` member (variable, method, constructor, or inner class) **can be accessed only within the same class** where it is declared.
* No other class — not even a subclass — can directly access it.

---

### **Scope**

* **Same Class** ✅
* **Same Package (different class)** ❌
* **Subclass (same or different package)** ❌
* **Other Packages** ❌

---

### **Example**

```java
class BankAccount {
    private double balance;

    private void showBalance() {
        System.out.println("Balance: " + balance);
    }

    public void deposit(double amount) {
        balance += amount;
        showBalance(); // ✅ Allowed (same class)
    }
}

class Test {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount();
        acc.deposit(1000); // ✅ Allowed (public method)
        // acc.balance = 500; // ❌ ERROR - private variable
        // acc.showBalance(); // ❌ ERROR - private method
    }
}
```

---

### **Key Points**

1. **Encapsulation**:
   `private` is heavily used to hide internal implementation and protect data.
   Commonly combined with **getters/setters** for controlled access.

2. **Constructors can be private**:
   Used in **Singleton patterns** to restrict object creation.

   ```java
   class Singleton {
       private static Singleton instance = new Singleton();
       private Singleton() {} // Prevents external instantiation
       public static Singleton getInstance() {
           return instance;
       }
   }
   ```

3. **Inner Classes**:
   You can have `private` inner classes that are accessible only inside the outer class.

4. **Private methods are NOT inherited**:
   Even in subclasses, you **cannot override** them.
   You can **declare a method with the same name** in the subclass, but it’s not overriding — it’s a new method.

---

### **Special Cases**

* **Reflection**:
  `private` can be accessed by reflection using:

  ```java
  field.setAccessible(true);
  ```

  But this breaks encapsulation and is generally discouraged unless in frameworks/libraries.

* **Anonymous inner classes** can still access `private` members of the enclosing class.

---

✅ **Bottom line**:
Use `private` when you want **full control** over how and where something is accessed, especially for **data hiding** and **security**.

---
