## **1. Concept of Encapsulation**

Encapsulation = **"Wrapping data (variables) and methods (logic) that operate on the data into a single unit"** and controlling how the data is accessed from outside.

Think of it as **data hiding + controlled access**.

* **Data hiding**: We keep fields `private` so that no outside class can directly modify them.
* **Controlled access**: We provide `public` getters/setters (or other methods) to control how outside code interacts with our data.

It’s like keeping valuables in a locker (private) and only giving a **controlled key** to trusted people.

---

## **2. Why is Encapsulation important?**

* **Security**: Prevents unauthorized access or invalid data.
* **Flexibility**: You can change the internal implementation without affecting other classes.
* **Maintainability**: Code is easier to manage and debug.
* **Validation**: You can check or modify data before storing it.

---

## **3. Syntax Structure**

```java
class BankAccount {
    // 1. Private variables (Data hiding)
    private String accountHolder;
    private double balance;

    // 2. Constructor to initialize
    public BankAccount(String accountHolder, double balance) {
        this.accountHolder = accountHolder;
        setBalance(balance); // using setter for validation
    }

    // 3. Getter (read-only access)
    public String getAccountHolder() {
        return accountHolder;
    }

    // 4. Setter with validation
    public void setBalance(double balance) {
        if (balance >= 0) {
            this.balance = balance;
        } else {
            System.out.println("Invalid balance! Cannot be negative.");
        }
    }

    public double getBalance() {
        return balance;
    }

    // Business logic method
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            System.out.println(amount + " deposited. New balance: " + balance);
        } else {
            System.out.println("Invalid deposit amount.");
        }
    }

    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println(amount + " withdrawn. New balance: " + balance);
        } else {
            System.out.println("Invalid withdraw amount.");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        BankAccount acc = new BankAccount("Sahi", 5000);

        // Direct access to variables is not possible:
        // acc.balance = -1000; // ❌ Compilation error

        // Controlled access:
        acc.deposit(2000);
        acc.withdraw(1000);

        // Trying invalid operation:
        acc.setBalance(-500); // ❌ Won't set due to validation
    }
}
```

**Output (sample):**

```
2000 deposited. New balance: 7000.0
1000 withdrawn. New balance: 6000.0
Invalid balance! Cannot be negative.
```

---

## **4. Tricky Interview Questions on Encapsulation**

### **Q1. Is Encapsulation the same as Data Hiding?**

**Trick:** Many think they are the same.
**Answer:**

* **Data hiding** is about restricting direct access to variables by making them `private`.
* **Encapsulation** is a broader concept that includes data hiding *and* bundling data with methods that operate on it.
  So, **data hiding is a subset of encapsulation**.

---

### **Q2. Can we achieve Encapsulation without getters/setters?**

**Trick:** People think getters/setters are mandatory.
**Answer:**
No, they are not mandatory. Encapsulation can be achieved simply by having private fields and providing any method (not necessarily a getter/setter) to control access. For example, you might only allow deposits/withdrawals without giving a getter for balance.

---

### **Q3. Can Encapsulation break if we return a reference to a mutable object?**

**Example:**

```java
class Student {
    private List<String> subjects = new ArrayList<>();

    public List<String> getSubjects() {
        return subjects; // ❌ Direct reference breaks encapsulation
    }
}
```

If you return the actual `List`, outside code can modify it directly, bypassing control.
**Solution:** Return an unmodifiable list or a copy.

---

### **Q4. How is Encapsulation different from Abstraction?**

**Trick:** They sound similar.
**Answer:**

* **Encapsulation:** Hides *how* the data is stored and controlled (implementation detail).
* **Abstraction:** Hides *unnecessary details* from the user and shows only relevant features.
  Encapsulation is about **access control**, Abstraction is about **designing an interface**.

---

### **Q5. Can fields be `public` and still be encapsulated?**

**Answer:**
If fields are public, you lose control over validation, so technically it breaks encapsulation. However, if you still route data changes through methods internally, you might keep logic intact — but it’s not recommended.

---

## **1. Returning Mutable Objects Directly**

**Looks safe** — private field, getter provided.
**Reality** — outsider can still change your data without permission.

```java
import java.util.*;

class Student {
    private List<String> subjects = new ArrayList<>();

    public Student() {
        subjects.add("Math");
        subjects.add("Science");
    }

    public List<String> getSubjects() {
        return subjects; // ❌ Returning actual list reference
    }
}

public class Main {
    public static void main(String[] args) {
        Student s = new Student();
        System.out.println("Before: " + s.getSubjects());

        // Modifying directly from outside
        s.getSubjects().add("History"); // Bypasses any validation

        System.out.println("After: " + s.getSubjects());
    }
}
```

**Output:**

```
Before: [Math, Science]
After: [Math, Science, History]
```

**Why it’s broken:** External code modified `subjects` without going through a controlled method.

**Fix:**

```java
public List<String> getSubjects() {
    return Collections.unmodifiableList(subjects); // Read-only
}
```

or

```java
return new ArrayList<>(subjects); // Return a copy
```

---

## **2. Using Setters Without Validation**

You *think* you’re controlling access, but you’re allowing invalid states.

```java
class BankAccount {
    private double balance;

    public void setBalance(double balance) { 
        this.balance = balance; // ❌ No validation
    }
}
```

This allows:

```java
acc.setBalance(-9999); // Balance can be negative
```

**Fix:**

```java
public void setBalance(double balance) {
    if (balance >= 0) {
        this.balance = balance;
    } else {
        throw new IllegalArgumentException("Balance cannot be negative");
    }
}
```

---

## **3. Exposing Internal References in Constructors**

Even if your fields are private, passing them by reference breaks encapsulation.

```java
class University {
    private List<String> students;

    public University(List<String> students) {
        this.students = students; // ❌ External reference stored
    }
}
```

If the caller modifies their original list, your internal list changes too.

**Fix:** Create a copy in the constructor:

```java
this.students = new ArrayList<>(students);
```

---

## **4. Making Fields `final` But Still Mutable**

Some developers think `final` = safe. Not always.

```java
class Library {
    private final List<String> books = new ArrayList<>();

    public List<String> getBooks() {
        return books; // ❌ Still mutable
    }
}
```

You can still do:

```java
library.getBooks().add("New Book");
```

**Fix:** Return unmodifiable list or a copy.

---

## **5. Partially Encapsulated Class**

Encapsulation must be **consistent**. Mixing `public` and `private` for related data is bad.

```java
class Employee {
    public String name; // ❌ Not hidden
    private double salary; // ✅ Hidden
}
```

Here, `salary` is protected but `name` is open — violates encapsulation principles.

---

## **6. Breaking Encapsulation Through Inheritance**

If you mark fields `protected` in a superclass, subclasses can directly change them.

```java
class Person {
    protected String name; // ❌ Accessible to all subclasses
}
```

Better: keep them private and use protected getters/setters if needed.

---

## **7. Immutable Objects That Aren’t Actually Immutable**

In interviews, they may ask: *"Can you make a truly immutable class?"*
Many fail because they forget **deep copies**.

```java
final class ImmutableStudent {
    private final String name;
    private final List<String> subjects;

    public ImmutableStudent(String name, List<String> subjects) {
        this.name = name;
        this.subjects = subjects; // ❌ Direct reference stored
    }

    public List<String> getSubjects() {
        return subjects; // ❌ External modification possible
    }
}
```

Fix by making defensive copies in **both constructor and getters**.

---

💡 **Pro Interview Tip:**
If they give you a code snippet and ask “Is this fully encapsulated?” — **always check for:**

1. Mutable object references being returned.
2. Constructor storing references instead of copies.
3. Setters without validation.
4. Protected/public fields that should be private.
5. Mutable objects inside “immutable” classes.

---

