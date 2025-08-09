# `Object` Class Thread Synchronization Methods: `wait()`, `notify()`, and `notifyAll()`

---

## Overview

In Java, **every object** has an intrinsic **monitor** (or lock). This monitor is used to coordinate threads that want to access critical sections of code or share data safely.

The `wait()`, `notify()`, and `notifyAll()` methods are part of this monitor mechanism and enable **thread communication** — allowing threads to wait for certain conditions and notify others when conditions change.

These methods belong to the `java.lang.Object` class (not `Thread`!), meaning **all objects can be used for thread synchronization**.

---

## Why are these important?

* They allow **threads to pause execution (`wait`)** until some condition is met.
* One or more threads can be **notified (`notify` or `notifyAll`)** to resume execution after the condition changes.
* This coordination avoids busy-waiting and improves efficiency.

---

## Method Signatures

```java
public final void wait() throws InterruptedException
public final void wait(long timeout) throws InterruptedException
public final void wait(long timeout, int nanos) throws InterruptedException

public final void notify()
public final void notifyAll()
```

---

## 1. `wait()`

* **Purpose:** Causes the current thread to **release the object's monitor** and go to the waiting state until:

  * Another thread calls `notify()` or `notifyAll()` on the same object, **or**
  * The thread is interrupted, **or**
  * The specified timeout (if any) expires.

* **Important:**

  * The current thread must **own the object's monitor** (i.e., be inside a `synchronized` block/method on that object), otherwise it throws `IllegalMonitorStateException`.
  * `wait()` releases the monitor and suspends the thread.
  * When notified, the thread will not immediately run, but **must reacquire the monitor first**.

* **Variants:**

  * `wait()`: waits indefinitely until notified.
  * `wait(long timeout)`: waits up to `timeout` milliseconds.
  * `wait(long timeout, int nanos)`: waits up to `timeout` milliseconds + `nanos` nanoseconds.

---

## 2. `notify()`

* **Purpose:** Wakes up **one** single thread that is waiting on the object's monitor (if any).
* The awakened thread will compete to reacquire the object's monitor and resume.
* If no threads are waiting, `notify()` does nothing.
* Must be called from a block synchronized on the object, or it throws `IllegalMonitorStateException`.

---

## 3. `notifyAll()`

* **Purpose:** Wakes up **all** threads waiting on the object's monitor.
* All awakened threads will compete to reacquire the monitor.
* Also must be called inside synchronized code on the object.

---

## How They Work Together: Basic Usage Pattern

### Producer-Consumer Example (Simplified)

```java
class Data {
    private int value;
    private boolean available = false;

    public synchronized void put(int val) {
        while (available) {
            try {
                wait();  // Wait until consumer consumes
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        value = val;
        available = true;
        notify();  // Notify consumer
    }

    public synchronized int get() {
        while (!available) {
            try {
                wait();  // Wait until producer produces
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        available = false;
        notify();  // Notify producer
        return value;
    }
}
```

* The `put` method waits if data is already available.
* The `get` method waits if data is not available.
* `wait()` makes threads release the monitor and sleep.
* `notify()` wakes up a waiting thread to continue execution.

---

## Important Points / Best Practices

* **`wait()`, `notify()`, and `notifyAll()` must always be called inside synchronized blocks/methods on the object’s monitor.**

* Use `wait()` inside a **loop that checks the condition** (not just `if`) because of **spurious wakeups** (a thread may wake up without a notification).

  ```java
  synchronized(lock) {
      while (!condition) {
          lock.wait();
      }
      // Proceed
  }
  ```

* Use `notifyAll()` instead of `notify()` if multiple threads might be waiting and you want to avoid missed notifications or deadlocks.

* Threads awakened by `notify()` or `notifyAll()` **must reacquire the lock** before continuing.

---

## Common Mistakes

| Mistake                                                    | Result / Error                                      |
| ---------------------------------------------------------- | --------------------------------------------------- |
| Calling `wait()` / `notify()` outside a synchronized block | `IllegalMonitorStateException` at runtime           |
| Using `if` instead of `while` before `wait()`              | Can cause missed conditions due to spurious wakeups |
| Forgetting to re-check condition after wakeup              | Thread may continue incorrectly                     |
| Using `notify()` when multiple threads wait                | Possible missed wakeups or deadlocks                |

---

## Summary Table

| Method        | Behavior                                       | Must be called inside `synchronized`? | Releases lock? | Wakes up waiting threads? |
| ------------- | ---------------------------------------------- | ------------------------------------- | -------------- | ------------------------- |
| `wait()`      | Waits indefinitely (or for timeout) for notify | Yes                                   | Yes            | No                        |
| `notify()`    | Wakes up one waiting thread                    | Yes                                   | No             | Yes (one thread)          |
| `notifyAll()` | Wakes up all waiting threads                   | Yes                                   | No             | Yes (all waiting threads) |

---



# Producer-Consumer Problem in Java with `wait()` and `notify()`

---

### The Problem:

* A **Producer** produces data and puts it into a shared buffer.
* A **Consumer** takes data from the shared buffer and processes it.
* The Producer must wait if the buffer is full.
* The Consumer must wait if the buffer is empty.
* Both coordinate via shared object methods and Java synchronization (`wait`/`notify`).

---

### Code:

```java
class Drop {
    // Message sent from producer to consumer.
    private String message;
    // True if message is available to be consumed.
    private boolean empty = true;

    // Called by producer to put a message
    public synchronized void put(String message) {
        // Wait until the buffer is empty
        while (!empty) {
            try {
                wait();  // release lock and wait
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        // Put the message and mark buffer as full
        empty = false;
        this.message = message;
        System.out.println("Produced: " + message);
        notify();  // Notify the waiting consumer
    }

    // Called by consumer to get the message
    public synchronized String take() {
        // Wait until the buffer is full (message available)
        while (empty) {
            try {
                wait();  // release lock and wait
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
        // Mark buffer as empty and return message
        empty = true;
        System.out.println("Consumed: " + message);
        notify();  // Notify the waiting producer
        return message;
    }
}

// Producer thread class
class Producer implements Runnable {
    private Drop drop;

    public Producer(Drop drop) {
        this.drop = drop;
    }

    public void run() {
        String[] messages = {
            "Message 1", "Message 2", "Message 3", "Message 4", "DONE"
        };

        for (String msg : messages) {
            drop.put(msg);  // Produce message
            try {
                Thread.sleep(500);  // simulate delay
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

// Consumer thread class
class Consumer implements Runnable {
    private Drop drop;

    public Consumer(Drop drop) {
        this.drop = drop;
    }

    public void run() {
        for (String msg = drop.take(); !msg.equals("DONE"); msg = drop.take()) {
            // Process the message
            try {
                Thread.sleep(1000);  // simulate delay in consumption
            } catch (InterruptedException e) {
                Thread.currentThread().interrupt();
            }
        }
    }
}

// Main class to run the example
public class ProducerConsumerExample {
    public static void main(String[] args) {
        Drop drop = new Drop();
        Thread producerThread = new Thread(new Producer(drop));
        Thread consumerThread = new Thread(new Consumer(drop));

        producerThread.start();
        consumerThread.start();
    }
}
```

---

### Explanation:

1. **Shared Resource: `Drop`**

   * It acts as a simple **buffer** holding one message.
   * Uses a boolean `empty` to track whether the buffer is free (`true`) or full (`false`).
   * Methods `put()` and `take()` are synchronized — only one thread can access them at a time.

2. **`put(String message)` (Producer)**

   * If the buffer is **not empty** (message waiting to be consumed), the producer **waits**.
   * When buffer becomes empty, it places the message and sets `empty` to `false`.
   * Calls `notify()` to wake up a waiting consumer.

3. **`take()` (Consumer)**

   * If the buffer is **empty** (no message to consume), the consumer **waits**.
   * When a message is available, it takes it, marks buffer as empty (`empty = true`).
   * Calls `notify()` to wake up a waiting producer.

4. **`Producer` Thread**

   * Creates a list of messages to produce.
   * Calls `put()` on `Drop` to produce messages, one by one.
   * Sleeps briefly to simulate time to produce.

5. **`Consumer` Thread**

   * Continuously calls `take()` on `Drop` to consume messages.
   * Stops consuming when it receives the special `"DONE"` message.
   * Sleeps briefly to simulate processing time.

6. **Thread Coordination**

   * When producer puts a message, it notifies consumer to wake and consume.
   * When consumer takes a message, it notifies producer to wake and produce next.

---

### Output Sample:

```
Produced: Message 1
Consumed: Message 1
Produced: Message 2
Consumed: Message 2
Produced: Message 3
Consumed: Message 3
Produced: Message 4
Consumed: Message 4
Produced: DONE
Consumed: DONE
```

---

### Key Points

* `wait()` **releases the lock and suspends the thread**, allowing others to enter synchronized methods.
* `notify()` wakes **one** waiting thread; `notifyAll()` wakes **all** waiting threads.
* Always call `wait()` inside a **loop** checking the condition to handle spurious wakeups.
* `put()` and `take()` are synchronized to prevent race conditions on shared data.
* This design pattern can be extended to buffers holding multiple items using more complex data structures like queues.

---

