# 1. **What are Generics? — Concept & Why**

**What:** generics let you parameterize types — classes, interfaces, methods — so the code works with different types while staying type-safe.

**Why:** before generics you used `Object` everywhere and casted. That caused `ClassCastException` at runtime. Generics move type errors to **compile time** and remove most casts.

**How it looks:**

```java
List<String> names = new ArrayList<>(); // compile-time: list of Strings only
String s = names.get(0); // no cast needed
```

**Pitfalls / Notes**

* Generics provide **compile-time** safety; they don’t create new runtime types (type erasure — explained in #9).
* Don’t use raw types (e.g., `List`) — they disable checks and produce warnings.

**Interview note:** “Generics = types as parameters; purpose: safety + reuse.”

---

# 2. **Generic Classes — definition, usage, common traps**

**What:** classes that declare a type parameter: `class Box<T> { ... }`.

**Why:** one implementation works for many types.

**Basic example:**

```java
class Box<T> {
    private T value;
    public void set(T v){ value = v; }
    public T get(){ return value; }
}
Box<String> bs = new Box<>();
bs.set("hi");
String x = bs.get(); // OK
```

**Common traps**

* `Box<int>` is illegal — generics work with reference types only. Use `Box<Integer>`.
* **Generic inheritance**: `Box<Integer>` is NOT a subtype of `Box<Number>`. Generics are **invariant**:

  ```java
  Box<Integer> bi = new Box<>();
  Box<Number> bn = bi; // compile error
  ```

**Why invariance?** if `Box<Integer>` were a `Box<Number>`, you could insert a `Double` into it and break type-safety.

**Practical tips**

* Make fields private.
* Use proper bounds if you need numeric-like behavior (`<T extends Number>`).

---

# 3. **Generic Methods — why and how**

**What:** methods with their own type parameters, `public static <T> void m(T x) { ... }`.

**Why:** you may need generic behavior in a class that itself is not generic, or you want method-level type inference.

**Example:**

```java
public static <T> T pickFirst(T a, T b) { return a; }

String s = YourClass.<String>pickFirst("a","b"); // explicit
String t = pickFirst("a","b"); // type inferred — common
```

**Important points**

* Type parameter **declaration `<T>` is before the return type**.
* Static methods cannot use the class’s type parameter (unless the method declares its own).

**Interview edge:** show both explicit `<Type>` invocation and inference.

---

# 4. **Bounded Type Parameters — upper and multiple bounds**

**What:** restrict the set of admissible types: `T extends SomeClass` or `T extends Interface1 & Interface2`.

**Upper bound example:**

```java
class NumBox<T extends Number> {
    T value;
    double doubleValue() { return value.doubleValue(); }
}
```

Only `Number` and subclasses allowed (`Integer`, `Double`, ...).

**Multiple bounds:**

```java
<T extends Number & Comparable<T>> // class first, then interfaces
```

**Remember ordering:** if you use a class bound, it must come first.

**Why useful:** you can call methods defined on the bound — `doubleValue()` exists on `Number`.

**Pitfalls**

* `T extends Number & Serializable` — ok.
* `T extends Comparable<T> & SomeInterface` — ok.
* Cannot write `T extends Integer` (final class) for variance reasons — still allowed but pointless.

**Practical tip:** Use bounded types when you need to call methods from the bound.

---

# 5. **Wildcards `?` — unbounded, `extends`, `super` (PECS)**

This is where many people trip up. Wildcards let you refer to generically-typed objects without fixing the exact parameter.

### a) Unbounded `<?>`

`List<?>` = list of *some* unknown type.

* You can **read** elements as `Object`.
* You **cannot add** anything (except `null`).

Example:

```java
void printList(List<?> list) {
  for(Object o: list) System.out.println(o);
}
```

### b) Upper-bounded `<? extends T>` — **Producer**

`List<? extends Number>` means a list of some subtype of `Number`. Useful to **read** numbers.

* You can **get** Number values.
* You **cannot add** (except `null`) because the exact subtype is unknown.

Example:

```java
List<Integer> ints = List.of(1,2,3);
List<? extends Number> nums = ints;
Number n = nums.get(0); // OK
nums.add(4); // compile error
```

### c) Lower-bounded `<? super T>` — **Consumer**

`List<? super Integer>` means a list of Integer or a supertype (Number, Object).

* You can **add** `Integer` safely.
* When you **get**, you only know it’s an `Object` (or a common supertype).

Example:

```java
List<Object> objs = new ArrayList<>();
List<? super Integer> superInts = objs;
superInts.add(10); // OK
Object o = superInts.get(0); // returns Object
Integer i = superInts.get(0); // compile error without cast
```

### PECS mnemonic

* **Producer Extends** (`? extends T`) — use when method **produces** `T`s for you (you read).
* **Consumer Super** (`? super T`) — use when method **consumes** `T`s from you (you write).

**Pitfalls / common mistakes**

* Trying to add to `? extends` — will fail.
* Expecting `List<Subclass>` to be assignable to `List<Superclass>` — not true without wildcard.

**Practical design note**

* Use `? extends` as return type (if you only read).
* Use `? super` as parameter type (if you only write).

---

# 6. **Multiple Type Parameters**

**What:** classes or methods can use several type parameters: `class Pair<K,V> { ... }`.

**Example:**

```java
class Pair<K,V> {
  private K key; private V value;
  Pair(K k, V v){ key=k; value=v; }
  K getKey(){ return key; }
  V getValue(){ return value; }
}
Pair<String,Integer> p = new Pair<>("Sahi",95);
```

**Why:** maps, pairs, tuples need separate types.

**Pitfalls**

* Keep it readable; too many type parameters (`A,B,C,D`) make APIs confusing.
* Use meaningful names: `K, V` or `T,U` for generic methods, `E` for elements.

**Interview point:** Understand nested generics like `Map<String, List<Integer>>`.

---

# 7. **Generic Interfaces**

Interfaces can declare type parameters — very common in the Java API.

**Examples:** `Comparable<T>`, `Comparator<T>`, `Function<T,R>`.

```java
interface Repository<T> {
  void save(T t);
  T load(int id);
}

class UserRepository implements Repository<User> { ... }
```

**Why:** allows polymorphic code that operates on abstract behavior rather than concrete types.

**Advanced:** a generic interface may allow different implementations with different type parameters, enabling flexibility in frameworks.

---

# 8. **Generic Constructors**

**What:** constructors can be generic even if the class isn’t. Syntax: `<T>` before constructor.

**Example:**

```java
class Container {
  public <T> Container(T t) {
    System.out.println(t.getClass());
  }
}

new Container("hello"); // T inferred as String
new Container(10);      // T inferred as Integer
```

**Why:** sometimes used for utility initialization or factory-like behavior during construction.

**Pitfall**: don’t confuse with class-level type parameters.

---

# 9. **Type Erasure — how generics work at runtime & consequences**

This is *the* deep topic for interviews.

**What:** Java generics are implemented by **type erasure**. The compiler enforces generics at compile time, but erases type parameters at runtime to maintain backward compatibility with pre-Java-5 bytecode.

**Mechanics**

* `List<String>` and `List<Integer>` both become `List` at runtime.
* Generic type parameters are replaced with their **first bound** (or `Object` if unbounded).
* The compiler inserts **casts** where necessary.
* The compiler may create **bridge methods** to preserve polymorphism across erasure.

**Consequences / concrete examples**

1. **No instanceof with parameterized types**

   ```java
   if (obj instanceof List<String>) {} // compile error
   if (obj instanceof List) {} // OK
   ```

2. **Cannot create generic arrays**

   ```java
   T[] arr = new T[10]; // compile error
   // Workaround with Object[] and cast:
   T[] arr = (T[]) new Object[10]; // unchecked warning
   ```

3. **Overload problems**
   You cannot overload solely by generic parameter:

   ```java
   void m(List<String> l) {}
   void m(List<Integer> l) {} // compile-time error — erased signatures clash
   ```

4. **Runtime type equality**

   ```java
   List<String> a = new ArrayList<>();
   List<Integer> b = new ArrayList<>();
   System.out.println(a.getClass() == b.getClass()); // true
   ```

5. **Bridge methods**
   When a subclass overrides a generic method with a specific signature, the compiler generates bridge methods to preserve polymorphism after erasure. (Advanced detail; know it exists.)

6. **Reflection & Class<T>**
   Because runtime erasure removes parameter info, sometimes you pass `Class<T>` to methods to preserve type tokens:

   ```java
   <T> T create(Class<T> cls) throws Exception {
     return cls.getDeclaredConstructor().newInstance();
   }
   ```

7. **Cannot have generic exception types**
   `class MyException<T> extends Exception` isn't useful; Java disallows throwing/catching generic exception types safely.

8. **Varargs + generics warning**
   `List<T>... lists` causes `unchecked` warnings because arrays are reified and generics are erased.

**Practical implications**

* If you need runtime type checks for generics, keep `Class<T>` tokens (e.g., `Map<Class<?>, ?>` or `TypeToken` pattern from Guava).
* Expect `@SuppressWarnings("unchecked")` in a few factory places where you must cast after safe operations.

---

## Additional Advanced Topics & Examples

### Covariance vs Contravariance: contrast with arrays

* Arrays are **covariant**: `Integer[]` is a subtype of `Number[]` (can cause runtime `ArrayStoreException`).
* Generics are **invariant**: `List<Integer>` is NOT a `List<Number>`. Use wildcards for flexibility.

### Wildcard capture — solve limitations

If you have `List<?>` and want to add items, use a helper:

```java
public static <T> void addAll(List<T> list, T... items) {
   for(T i: items) list.add(i);
}
// use capture
public static void addAllCapture(List<?> list, Object... items) {
   addAllHelper(list, items); // uses helper to capture wildcard type
}
private static <T> void addAllHelper(List<T> list, Object... items) {
   for(Object o: items) list.add((T)o); // unchecked
}
```

This is advanced but shows wildcard *capture* trick.

### PECS in code: practical API design

```java
// Producer: returns values => use extends
public double sum(List<? extends Number> nums) { ... }

// Consumer: accepts values => use super
public void addNumbers(List<? super Integer> dst) { dst.add(1); }
```

### Raw types — avoid them

Raw type example:

```java
List raw = new ArrayList<Integer>();
raw.add("string"); // compiles; runtime problem
Integer i = (Integer) ((List) raw).get(0); // ClassCastException possible
```

Use generics always.

---

## Best Practices / Cheat Sheet (useful quick memory)

* Use specific generic types: `List<String>`, not raw `List`.
* For API parameters:

  * If method only **reads** from a collection use `? extends T`.
  * If method only **writes** to a collection use `? super T`.
  * If both read and write, use a concrete type `List<T>`.
* Don’t try to create `new T[]`. Use `ArrayList<T>` or `@SuppressWarnings("unchecked")` with care.
* Use meaningful type parameter names: `E` (element), `K,V` (map), `T` generic type, `R` return.
* Keep generics shallow — avoid `Map<String, List<Map<Integer, Set<Foo>>>>` unless necessary.
* Favor generic methods for utility operations.
* Remember erasure: no `instanceof` with generic parameter, and no overloaded methods that differ only by generic parameters.

---

## Quick Interview Questions you should be able to answer

1. Why is `List<Integer>` not a subtype of `List<Number>`? (Answer: invariance; runtime safety.)
2. What is PECS? (Producer Extends, Consumer Super.)
3. Why can’t you create `new T[]`? (Because of type erasure + arrays are reified.)
4. What is type erasure and its consequences? (Explained above.)
5. What are default methods and can generics be used with them? (Yes — default methods can have generics too.)
6. Explain wildcard capture. (Helper method technique.)


