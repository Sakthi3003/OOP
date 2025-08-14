

## 🏗️ Constructors

* A **constructor** is a special method that runs **automatically** when an object is created.
* It is used to **set values** inside the object.

```java
class Student {
    String name;
    int roll;
    int marks;

    // Default Constructor
    Student() {
        name = "Unknown";
        roll = 0;
        marks = 0;
    }

    // Parameterized Constructor
    Student(String n, int r, int m) {
        name = n;
        roll = r;
        marks = m;
    }
}
```

```java
Student s1 = new Student(); // calls default
Student s2 = new Student("Sahi", 101, 98); // calls parameterized
```

---

## 🔁 Constructor Chaining

> One constructor calls another using `this()`

```java
class Student {
    String name;
    int roll;
    int marks;

    Student() {
        this("NoName", 0, 0);  // 👈 calls below constructor
    }

    Student(String name, int roll, int marks) {
        this.name = name;
        this.roll = roll;
        this.marks = marks;
    }
}
```
