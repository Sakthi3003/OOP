## **What Are Packages in Java?**

In Java, **packages are containers for classes and interfaces**.
Think of them like folders on your computer — they help you organize your code and keep class names from clashing.

---

### **Why Do We Use Packages?**

* **Organization**: Group related classes together.
* **Avoid name conflicts**: You can have your own `List` class in `mypackage.List` without clashing with `java.util.List`.
* **Visibility control**: Packages help control which classes and members can be accessed from outside.

---

### **How Packages Work**

Packages are both:

1. **A naming mechanism** — The package name becomes part of the class name (e.g., `mypackage.MyClass`).
2. **A visibility control mechanism** — Only `public` members are accessible outside the package (unless subclasses are involved).

---

### **Creating a Package**

```java
package MyPackage; // must be the first statement in the file

public class MyClass {
    public void greet() {
        System.out.println("Hello from MyPackage!");
    }
}
```

---

### **Where Are Packages Stored?**

* Java uses **folders (directories)** in your file system to store packages.
* The folder name **must match the package name exactly** (case-sensitive).
* For example, if you declare:

```java
package java.awt.image;
```

It must be stored in:

* Windows: `java\awt\image`
* Linux/Mac: `java/awt/image`

---

### **Package Naming Tips**

* Use **lowercase** names by convention.
* For big projects, use a **reverse domain name**:
  Example: `com.example.project.module`

---

### **How Java Finds Your Packages**

The Java runtime looks for packages in three ways:

1. **Current working directory** (default) – If your package folder is here, Java will find it automatically.
2. **`CLASSPATH` environment variable** – You can set this to tell Java where to look.
3. **`-classpath` (or `-cp`) option** – You can specify paths directly when running or compiling:

```bash
javac -cp "path/to/classes" MyClass.java
java -cp "path/to/classes" MyClass
```

---

### **User defined vd Predefined packages**

In Java, packages are basically containers (namespaces) that group related classes, interfaces, and sub-packages together to organize code and avoid naming conflicts.

---

## **1. Predefined Packages**

These are **built-in packages** that come with the Java Development Kit (JDK).
They contain ready-to-use classes and interfaces.

### Examples:

| **Package** | **Purpose**                                                                          |
| ----------- | ------------------------------------------------------------------------------------ |
| `java.lang` | Core language classes (String, Object, Math, Thread, etc.) – imported automatically. |
| `java.util` | Utility classes (Collections, Date, Scanner, etc.).                                  |
| `java.io`   | Input/output classes (File, BufferedReader, etc.).                                   |
| `java.sql`  | JDBC API for database connectivity.                                                  |
| `java.net`  | Networking classes (URL, Socket, etc.).                                              |
| `java.time` | Date and time API.                                                                   |

**Example:**

```java
import java.util.ArrayList; // Using a predefined package

public class Test {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("Java");
        list.add("Python");
        System.out.println(list);
    }
}
```

---

## **2. User-Defined Packages**

These are **custom packages** created by the programmer to organize their own classes.

### Steps to create a package:

#### 1️⃣ Create a package

```java
// File: MyPackage/MyClass.java
package MyPackage; // Declare package at the top

public class MyClass {
    public void display() {
        System.out.println("Hello from MyPackage!");
    }
}
```

#### 2️⃣ Compile with package option

```sh
javac -d . MyPackage/MyClass.java
```

(`-d .` means store class files in the proper package folder structure.)

#### 3️⃣ Use the package

```java
// File: TestPackage.java
import MyPackage.MyClass; // Import user-defined package

public class TestPackage {
    public static void main(String[] args) {
        MyClass obj = new MyClass();
        obj.display();
    }
}
```

---

✅ **Key Difference:**

* **Predefined packages**: Come with Java, ready to use.
* **User-defined packages**: You create them for your own project structure.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------

### **Importing Packages**

When you import a package, **only public members** of its classes are accessible to non-subclasses.

```java
import mypackage.MyClass;

public class Test {
    public static void main(String[] args) {
        MyClass obj = new MyClass(); // works if MyClass is public
    }
}
```

### **Normal Import vs Static Import 

**Static import** in Java is a feature that allows you to access static members (variables or methods) of a class **directly**, without qualifying them with the class name.

---

### **Normal Import vs Static Import**

#### **Normal Import**

You import the whole class and then call its static members with the class name.

```java
import java.lang.Math;

public class Example {
    public static void main(String[] args) {
        System.out.println(Math.sqrt(16)); // Need to use Math.
    }
}
```

---

#### **Static Import**

You import only the static members you want (or all of them), so you can use them **directly** without the class name.

```java
import static java.lang.Math.sqrt; // importing only sqrt method
import static java.lang.Math.PI;   // importing constant PI

public class Example {
    public static void main(String[] args) {
        System.out.println(sqrt(16)); // No need for Math.
        System.out.println(PI);       // No need for Math.
    }
}
```

Or import **all** static members:

```java
import static java.lang.Math.*;

public class Example {
    public static void main(String[] args) {
        System.out.println(sqrt(25));
        System.out.println(PI);
    }
}
```

---

### **When to Use Static Import**

✅ Use it when:

* You frequently use static members (constants, utility methods).
* It improves readability (e.g., `PI` instead of `Math.PI`).

⚠️ Avoid overuse because:

* It can make code less readable if too many static members are imported from different classes (name clashes).
* It hides the origin of methods/constants.


