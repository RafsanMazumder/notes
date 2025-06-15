1. Virtual Function in Java

A virtual function is a method that is resolved at runtime (dynamic dispatch) rather than compile-time. This allows Java
to support polymorphism, where a subclass can override a method from its superclass and the appropriate method is called
based on the actual object type at runtime.

In Java, all non-static, non-final, non-private instance methods are virtual functions by default.

These methods are not virtual in Java:

private methods: can't be overridden.

static methods: resolved at compile-time.

final methods: cannot be overridden, so no dynamic dispatch.

2. Java class locking

Every class in Java has a corresponding Class object in memory. You can synchronize on this Class object to create a lock that is shared among all instances of that class.

```java
public class MyClass {
    public static void doSomething() {
        synchronized (MyClass.class) {
            // critical section
            System.out.println("Lock acquired on MyClass.class");
        }
    }
}
```

| Lock Type     | Synchronized On   | Scope                                    |
|---------------|-------------------|------------------------------------------|
| Instance lock | `this`            | Per object instance                      |
| Class lock    | `ClassName.class` | Shared across all instances of the class |

To synchronize static methods:

```java
public class MyClass {
    public static synchronized void staticMethod() {
        // synchronized on MyClass.class
    }
}
```

This is functionally equivalent to:

```java
public class MyClass {
    public static void staticMethod() {
        synchronized (MyClass.class) {
            // critical section
        }
    }
}
```

3. Visitor Pattern

```java
void handle(Object object) {
    if (object instanceof A) {
        doSomethingA((A) object);
    } else if (object instanceof B) {
        doSomethingB((B) object);
    }
}
```

4. Java call by value or reference

5. What is the class variable?

Static variables are called class variable

6. What is the difference between the instanceof and getclass

7. 



