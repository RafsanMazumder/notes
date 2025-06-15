# Difference between Java & C++

| Feature                  | Java                                     | C++                                      |
|--------------------------|------------------------------------------|------------------------------------------|
| **Memory Management**    | Automatic Garbage Collection             | Manual `new`/`delete` + Destructors      |
| **Compilation**          | Source → Bytecode → JIT to machine code  | Source → Direct machine code             |
| **Pointers**             | References only, no pointer arithmetic   | Pointers with full arithmetic (`*`, `&`) |
| **Multiple Inheritance** | Single inheritance + multiple interfaces | Full multiple inheritance                |
| **Method Overriding**    | Virtual by default                       | Requires `virtual` keyword               |
| **Operator Overloading** | Not supported (except `+` for String)    | Fully supported for all operators        |
| **Templates/Generics**   | Type erasure at runtime                  | Compile-time template instantiation      |
| **Global Functions**     | Must be inside classes                   | Allowed outside classes                  |
| **Const Correctness**    | `final` keyword only                     | `const` keyword with deep semantics      |
| **Header Files**         | Not required (.java files only)          | Required (.h/.hpp declarations)          |

## Code Examples

### Memory Management

```java
// Java - Automatic cleanup
String str = new String("Hello");
// GC handles cleanup automatically

// C++ - Manual cleanup
std::string* str = new std::string("Hello");
delete str; // Must manually delete
```

### Pointers vs References

```java
// Java - Only references
int[] arr = {1, 2, 3};
// No pointer arithmetic possible

// C++ - Pointers with arithmetic
int arr[] = {1, 2, 3};
int* ptr = arr;
ptr++; // Move to next element
```

### Multiple Inheritance

```java
// Java - Single inheritance
class Child extends Parent implements Interface1, Interface2 {}

// C++ - Multiple inheritance
class Child : public Parent1, public Parent2 {};
```

# What happens when you run `java HelloWorld`?

## Step 1: Source Code Creation

```java
// HelloWorld.java
public class HelloWorld {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
    }
}
```

## Step 2: Compilation Process

| Component             | Function          | Details                                                      |
|-----------------------|-------------------|--------------------------------------------------------------|
| **Lexical Analysis**  | Tokenization      | Breaks source into tokens (keywords, identifiers, operators) |
| **Syntax Analysis**   | Parsing           | Creates Abstract Syntax Tree (AST)                           |
| **Semantic Analysis** | Type checking     | Verifies types, scopes, and declarations                     |
| **Code Generation**   | Bytecode creation | Generates platform-independent bytecode                      |

**Command:** `javac HelloWorld.java`

**Output:** `HelloWorld.class` (contains bytecode)

## Step 3: JVM Execution Process

**Command:** `java HelloWorld`

| Phase              | Component                | Action                   | Details                                          |
|--------------------|--------------------------|--------------------------|--------------------------------------------------|
| **Loading**        | Bootstrap Class Loader   | Load core classes        | `java.lang.*`, `java.util.*`                     |
|                    | Extension Class Loader   | Load extension classes   | `javax.*` packages                               |
|                    | Application Class Loader | Load application classes | Your `.class` files                              |
| **Linking**        | Verification             | Bytecode verification    | Security checks, format validation               |
|                    | Preparation              | Memory allocation        | Static variables initialization                  |
|                    | Resolution               | Symbol resolution        | Convert symbolic references to direct references |
| **Initialization** | Class initialization     | Static block execution   | Run static initializers                          |
| **Execution**      | Method execution         | Run main method          | Actual program execution                         |

## Key Takeaways for Interviews

1. **Java is both compiled AND interpreted** - compiled to bytecode, then interpreted/JIT compiled
2. **Bytecode is platform-independent** - same .class file runs on any JVM
3. **JIT compilation provides runtime optimization** - gets faster with more execution
4. **Class loading is hierarchical** - delegation model with parent-first loading
5. **JVM manages memory automatically** - garbage collection handles object cleanup
6. **Bytecode verification ensures security** - prevents malicious code execution
7. **Method area stores class-level data** - shared across all instances
8. **Each thread has its own stack** - method calls and local variables
9. **Heap stores all objects** - shared memory for instance data
10. **JIT compilation is adaptive** - optimizes based on runtime behavior

# JVM, JDK, JRE

## Core Definitions

| Component | Full Name                | Purpose                        | Contains                 |
|-----------|--------------------------|--------------------------------|--------------------------|
| **JVM**   | Java Virtual Machine     | **Executes** Java bytecode     | Runtime environment only |
| **JRE**   | Java Runtime Environment | **Runs** Java applications     | JVM + Standard Libraries |
| **JDK**   | Java Development Kit     | **Develops** Java applications | JRE + Development Tools  |

## Relationship Hierarchy

```
JDK (Development Kit)
├── JRE (Runtime Environment)
│   ├── JVM (Virtual Machine)
│   └── Standard Libraries (java.lang, java.util, etc.)
└── Development Tools (javac, javadoc, jar, etc.)
```

## Practical Examples

### JVM Example

```bash
# JVM executes bytecode
java HelloWorld    # JVM loads and executes HelloWorld.class
```

**What JVM does:**

- Loads `.class` files
- Verifies bytecode
- Executes instructions
- Manages memory (garbage collection)

### JRE Example

```java
// This code needs JRE to run
import java.util.ArrayList;  // Standard library
import java.io.File;         // I/O library

public class App {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();  // JRE provides ArrayList
        File file = new File("data.txt");           // JRE provides File class
    }
}
```

**What JRE provides:**

- JVM to execute the code
- Standard libraries (`java.util.*`, `java.io.*`, etc.)

### JDK Example

```bash
# Development workflow using JDK tools
javac HelloWorld.java    # Compiler (JDK tool)
jar cf app.jar *.class   # JAR tool (JDK tool)  
javadoc *.java          # Documentation (JDK tool)
java HelloWorld         # Execution (uses JRE within JDK)
```

**Bottom Line:** You develop with JDK, distribute with JRE, execute on JVM.

# Java Access Modifiers

| Modifier      | Keyword     | Same Class | Subclass | Different Package (Non-subclass) |
|---------------|-------------|------------|----------|----------------------------------|
| **Private**   | `private`   | ✅          | ❌        | ❌                                |
| **Protected** | `protected` | ✅          | ✅        | ❌                                |
| **Public**    | `public`    | ✅          | ✅        | ✅                                |

## Use Cases

| Use Case          | Modifier    | Example                            |
|-------------------|-------------|------------------------------------|
| **Encapsulation** | `private`   | Internal fields, helper methods    |
| **Inheritance**   | `protected` | Methods for subclasses to override |
| **Public API**    | `public`    | Methods/fields for external use    |

# Java Primitive & Object Data Types

## Primitive Types (8 Total)

| Type        | Size   | Range                 | Default  | Example                |
|-------------|--------|-----------------------|----------|------------------------|
| **byte**    | 8-bit  | -128 to 127           | 0        | `byte b = 10;`         |
| **short**   | 16-bit | -32,768 to 32,767     | 0        | `short s = 1000;`      |
| **int**     | 32-bit | -2.1B to 2.1B         | 0        | `int i = 42;`          |
| **long**    | 64-bit | -9.2E18 to 9.2E18     | 0L       | `long l = 123L;`       |
| **float**   | 32-bit | 6-7 decimal digits    | 0.0f     | `float f = 3.14f;`     |
| **double**  | 64-bit | 15 decimal digits     | 0.0d     | `double d = 3.14;`     |
| **char**    | 16-bit | 0 to 65,535 (Unicode) | '\u0000' | `char c = 'A';`        |
| **boolean** | 1-bit  | true/false            | false    | `boolean flag = true;` |

## Wrapper Classes (Object Types)

| Primitive   | Wrapper Class | Example                |
|-------------|---------------|------------------------|
| **byte**    | `Byte`        | `Byte b = 10;`         |
| **short**   | `Short`       | `Short s = 1000;`      |
| **int**     | `Integer`     | `Integer i = 42;`      |
| **long**    | `Long`        | `Long l = 123L;`       |
| **float**   | `Float`       | `Float f = 3.14f;`     |
| **double**  | `Double`      | `Double d = 3.14;`     |
| **char**    | `Character`   | `Character c = 'A';`   |
| **boolean** | `Boolean`     | `Boolean flag = true;` |

## Key Differences

| Aspect              | Primitive                    | Object                   |
|---------------------|------------------------------|--------------------------|
| **Memory Location** | Stack                        | Heap                     |
| **Null Assignment** | ❌ Cannot be null             | ✅ Can be null            |
| **Default Value**   | Has default (0, false, etc.) | `null`                   |
| **Performance**     | Faster                       | Slower (object overhead) |
| **Memory Usage**    | Less memory                  | More memory              |
| **Methods**         | No methods                   | Has methods              |
| **Collections**     | Cannot use directly          | Can use in collections   |
| **Comparison**      | `==` compares values         | `==` compares references |

## Autoboxing & Unboxing

| Operation          | Example                        | Description              |
|--------------------|--------------------------------|--------------------------|
| **Autoboxing**     | `Integer i = 42;`              | Primitive → Wrapper      |
| **Unboxing**       | `int x = Integer.valueOf(42);` | Wrapper → Primitive      |
| **In Collections** | `list.add(42);`                | Auto-converts to Integer |
| **In Arithmetic**  | `Integer a = 5; a++;`          | Unbox → increment → box  |

# switch statement syntax

```java
switch (expression) {
    case value1:
        // statements
        break;
    case value2:
        // statements
        break;
    default:
        // statements
        break;
}
```

# do-while vs while loop

| Loop Type    | Syntax                                 | Key Difference             |
|--------------|----------------------------------------|----------------------------|
| **while**    | Condition checked **before** execution | May not execute at all     |
| **do-while** | Condition checked **after** execution  | Executes **at least once** |

# Class & Object

## What is a Class?

- **Blueprint/Template** for creating objects
- Defines **state** (fields/attributes) and **behavior** (methods)
- Does **NOT** consume memory until objects are created

### Class Syntax

```java
[access_modifier] class ClassName {
    // Fields (state)
    [access_modifier] dataType fieldName;
    
    // Constructor
    [access_modifier] ClassName(parameters) {
        // initialization code
    }
    
    // Methods (behavior)
    [access_modifier] returnType methodName(parameters) {
        // method body
    }
}
```

## What is an Object?

- **Instance** of a class
- **Actual entity** that occupies memory
- Has **state** (field values) and **behavior** (can invoke methods)

### What Happens During Object Creation?

| Step                         | Process                      | Memory Action                 |
|------------------------------|------------------------------|-------------------------------|
| **1. Memory Allocation**     | JVM allocates memory in heap | Heap space reserved           |
| **2. Field Initialization**  | Default values assigned      | Fields get default values     |
| **3. Constructor Execution** | Constructor code runs        | Custom initialization         |
| **4. Reference Assignment**  | Reference stored in variable | Stack variable points to heap |

## Dot Operator (.)

- **Access** object members (fields and methods)
- **Syntax**: `objectReference.memberName`

## Types of Object Creation

### 1. Using 'new' Keyword (Most Common)

```java
// Standard constructor call
Car car1 = new Car("Toyota", "Prius", 2023, 28000.0);

// Anonymous object (no reference stored)
new Car("Honda", "Accord", 2022, 26000.0).start();
```

### 2. Using Factory Methods

```java
public class Car {
    private String brand, model;
    private int year;
    
    // Private constructor
    private Car(String brand, String model, int year) {
        this.brand = brand;
        this.model = model;
        this.year = year;
    }
    
    // Factory method
    public static Car createEconomyCar(String brand, String model) {
        return new Car(brand, model, 2020);
    }
    
    public static Car createLuxuryCar(String brand, String model) {
        return new Car(brand, model, 2023);
    }
}

// Usage
Car economyCar = Car.createEconomyCar("Nissan", "Versa");
Car luxuryCar = Car.createLuxuryCar("Mercedes", "S-Class");
```

### 3. Using Clone Method

```java
public class Car implements Cloneable {
    private String brand, model;
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

// Usage
Car originalCar = new Car("Toyota", "Camry", 2023, 25000.0);
Car clonedCar = (Car) originalCar.clone();
```

### 4. Using Reflection

```java
import java.lang.reflect.Constructor;

public class ReflectionExample {
    public static void main(String[] args) throws Exception {
        // Get class
        Class<?> carClass = Car.class;
        
        // Get constructor
        Constructor<?> constructor = carClass.getConstructor(
            String.class, String.class, int.class, double.class);
        
        // Create object
        Car car = (Car) constructor.newInstance("BMW", "X3", 2023, 45000.0);
    }
}
```

## Memory Layout

### Stack vs Heap for Objects

```java
public void demonstrateMemory() {
    // Stack: stores reference variable 'car'
    // Heap: stores actual Car object
    Car car = new Car("Tesla", "Model S", 2023, 75000.0);
    
    // Stack: stores reference variable 'anotherCar'  
    // Heap: stores another Car object
    Car anotherCar = new Car("Audi", "A4", 2022, 40000.0);
    
    // Stack: stores reference variable 'sameCar'
    // Heap: NO new object created, points to existing object
    Car sameCar = car;  // Both 'car' and 'sameCar' point to same object
}
```

### Reference vs Object

```java
public class ReferenceExample {
    public static void main(String[] args) {
        Car car1 = new Car("Ford", "F150", 2023, 35000.0);
        Car car2 = new Car("Ford", "F150", 2023, 35000.0);
        Car car3 = car1;
        
        // Reference comparison (==)
        System.out.println(car1 == car2);  // false (different objects)
        System.out.println(car1 == car3);  // true (same object reference)
        
        // Object content comparison (equals)
        System.out.println(car1.equals(car2));  // depends on equals() implementation
    }
}
```

# Java Pass by Value or Reference

## Key Concept

**Java is ALWAYS pass-by-value.** This is a fundamental concept that often confuses developers, especially those coming
from languages that support pass-by-reference.

## What This Means

### For Primitive Types

When you pass primitive types (int, double, boolean, char, etc.), Java passes a **copy of the actual value**.

```java
public void modifyPrimitive(int x) {
    x = 100;  // Only modifies the local copy
}

int original = 5;
modifyPrimitive(original);
System.out.println(original);  // Still prints 5
```

### For Object References

When you pass objects, Java passes a **copy of the reference** (memory address), not the object itself or a reference to
the reference.

```java
public void modifyObject(StringBuilder sb) {
    sb.append(" World");  // Modifies the object that the reference points to
}

public void reassignObject(StringBuilder sb) {
    sb = new StringBuilder("New Object");  // Only changes the local copy of reference
}

StringBuilder original = new StringBuilder("Hello");
modifyObject(original);
System.out.println(original);  // Prints "Hello World"

reassignObject(original);
System.out.println(original);  // Still prints "Hello World"
```

## Common Interview Questions & Answers

**Q: "Can you change the contents of an object passed to a method?"**
A: Yes, because you receive a copy of the reference pointing to the same object in memory.

**Q: "Can you make the original reference variable point to a different object?"**
A: No, because you only have a copy of the reference, not the original reference variable itself.

**Q: "What about arrays?"**
A: Arrays are objects in Java, so the same rules apply - you get a copy of the reference to the array.

# this keyword

The **this** keyword is a reference variable that refers to the current object instance within an instance method or
constructor.

## Cannot Use in Static Context

```java
public class Example {
    static int count = 0;
    
    public static void staticMethod() {
        // this.count = 5;  // COMPILE ERROR - no 'this' in static context
        count = 5;  // Correct way
    }
}
```

# Java Garbage Collection (GC)

## What is Garbage Collection?

Garbage Collection is Java's **automatic memory management** process that identifies and removes objects from heap
memory that are no longer reachable or referenced by any part of the program.

## Why Garbage Collection?

- **Prevents memory leaks** by automatically freeing unused memory
- **Reduces programmer burden** - no manual memory management like C/C++
- **Improves application reliability** by preventing OutOfMemoryError in many cases

## How GC Works - Object Lifecycle

### Object Creation

```java
String str = new String("Hello");  // Object created in heap
```

### Object Becomes Unreachable

```java
str = null;  // Original "Hello" object now unreachable
str = new String("World");  // Previous object becomes eligible for GC
```

### GC Process

1. **Mark**: Identify which objects are still reachable
2. **Sweep**: Remove unreachable objects
3. **Compact**: Defragment memory (optional, depends on collector)

## Memory Areas and Generations

### Heap Structure (Generational Hypothesis)

```
Heap Memory
├── Young Generation
│   ├── Eden Space (new objects)
│   ├── Survivor Space 0 (S0)
│   └── Survivor Space 1 (S1)
└── Old Generation (Tenured Space)
```

### Why Generational?

- **Most objects die young** - short-lived objects are collected quickly
- **Fewer old objects need collection** - reduces GC overhead
- **Different algorithms** optimized for each generation

## GC Process Flow

### Minor GC (Young Generation)

1. New objects allocated in **Eden space**
2. When Eden fills up, **Minor GC** triggered
3. Live objects moved to **Survivor space**
4. Objects surviving multiple Minor GCs **promoted to Old Generation**

### Major/Full GC (Old Generation)

1. When Old Generation fills up, **Major GC** triggered
2. **More expensive** - examines entire heap
3. **Stop-the-world** event - application pauses

## Common GC Algorithms

### Serial GC

```bash
-XX:+UseSerialGC
```

- **Single-threaded** collector
- Good for **small applications** or single-core machines
- **Pauses application** during collection

### Parallel GC (Default in Java 8)

```bash
-XX:+UseParallelGC
```

- **Multi-threaded** collector
- Good for **throughput-focused** applications
- Still has **stop-the-world** pauses

## Key GC Tuning Parameters

### Heap Size

```bash
-Xms2g          # Initial heap size
-Xmx4g          # Maximum heap size
-XX:NewRatio=2  # Old/Young generation ratio
```

### GC Behavior

```bash
-XX:MaxGCPauseMillis=100    # Target pause time (G1)
-XX:GCTimeRatio=99          # Throughput goal
-XX:+PrintGC                # Enable GC logging
```

## When Objects Become Eligible for GC

### Reference Types

```java
// Strong reference - prevents GC
Object obj = new Object();

// Null reference - eligible for GC
obj = null;

// Method local variables - eligible after method ends
public void method() {
    String local = new String("temp");
}  // 'local' becomes eligible here
```

### Common Scenarios

1. **Object goes out of scope**
2. **Reference set to null**
3. **Circular references** with no external references
4. **Anonymous objects**: `new StringBuilder().append("test")`

## Memory Leaks in Java (Despite GC)

### Common Causes

```java
// 1. Static collections
public class LeakyClass {
    private static List<Object> cache = new ArrayList<>();  // Never cleared
}

// 2. Listener not removed
button.addActionListener(listener);  // Forgot to remove

// 3. Inner class holding outer reference
public class Outer {
    class Inner {  // Holds reference to Outer instance
    }
}
```

## Interview Questions & Answers

**Q: "Can you force garbage collection?"**
A: You can **suggest** it with `System.gc()` or `Runtime.gc()`, but JVM is not obligated to run GC immediately.

**Q: "What happens if an object's finalize() method throws an exception?"**
A: The exception is **ignored**, and the object may not be garbage collected properly.

**Q: "Difference between Minor and Major GC?"**
A: Minor GC cleans Young Generation (fast, frequent), Major GC cleans Old Generation (slow, expensive).

**Q: "How do you detect memory leaks?"**
A: Use profiling tools like **JVisualVM**, **Eclipse MAT**, **JProfiler**, or monitor heap usage patterns.

## Best Practices

- **Avoid premature optimization** - profile first
- **Set appropriate heap sizes** based on application needs
- **Monitor GC logs** in production
- **Choose GC algorithm** based on application requirements (throughput vs latency)
- **Avoid creating unnecessary objects** in tight loops

## Red Flags for Interviewers

- Don't say "Java has no memory leaks" - it does, just different types
- Don't recommend calling `System.gc()` in production code
- Understand that **finalize()** is deprecated (Java 9+) and unreliable

# GC-Free Programming

## What is GC-Free Programming?

Writing Java code that **creates fewer objects** to reduce garbage collection overhead. Important for **high-performance
applications** like trading systems or real-time games.

## Main Strategies

### 1. Object Pooling

Reuse objects instead of creating new ones:

```java
// Simple object pool
public class StringBuilderPool {
    private final Queue<StringBuilder> pool = new ArrayDeque<>();
    
    public StringBuilder get() {
        StringBuilder sb = pool.poll();
        return sb != null ? sb : new StringBuilder();
    }
    
    public void release(StringBuilder sb) {
        sb.setLength(0);  // Reset
        pool.offer(sb);
    }
}

// Usage
StringBuilderPool pool = new StringBuilderPool();
StringBuilder sb = pool.get();
try {
    sb.append("Hello").append(" World");
    return sb.toString();
} finally {
    pool.release(sb);
}
```

### 2. Avoid Boxing/Unboxing

Use primitive collections instead of wrapper objects:

```java
// BAD - creates Integer objects
List<Integer> numbers = new ArrayList<>();
numbers.add(42);  // Boxing: int → Integer

// BETTER - using Eclipse Collections
MutableIntList numbers = new IntArrayList();
numbers.add(42);  // No boxing, no objects created
```

### 3. Reuse StringBuilder

Don't create new StringBuilder instances:

```java
public class StringHelper {
    private final StringBuilder reusable = new StringBuilder();
    
    public String concat(String a, String b, String c) {
        reusable.setLength(0);  // Reset without creating new object
        reusable.append(a).append(b).append(c);
        return reusable.toString();
    }
}
```

### 4. Avoid Common Allocation Traps

```java
// BAD - hidden allocations
String result = str1 + str2;        // Creates StringBuilder
Integer count = map.get(key);       // Boxing
String[] parts = text.split(",");   // Creates array

// BETTER
StringBuilder sb = reusableBuilder;
sb.setLength(0).append(str1).append(str2);

int count = primitiveMap.get(key);  // No boxing

// Manual parsing instead of split()
```

### 5. Use Primitive Arrays

Instead of collections for simple data:

```java
// BAD
List<Integer> ids = new ArrayList<>();

// BETTER  
int[] ids = new int[100];  // Fixed size, no objects
int count = 0;

// Add
ids[count++] = newId;

// Iterate
for (int i = 0; i < count; i++) {
    process(ids[i]);
}
```

## Simple Patterns

### Ring Buffer for Fixed-Size Queues

```java
public class IntRingBuffer {
    private final int[] buffer;
    private int head = 0, tail = 0;
    
    public IntRingBuffer(int size) {
        buffer = new int[size];
    }
    
    public boolean offer(int value) {
        int nextTail = (tail + 1) % buffer.length;
        if (nextTail == head) return false;  // Full
        
        buffer[tail] = value;
        tail = nextTail;
        return true;
    }
    
    public int poll() {
        if (head == tail) return -1;  // Empty
        
        int value = buffer[head];
        head = (head + 1) % buffer.length;
        return value;
    }
}
```

### ThreadLocal for Thread-Safe Reuse

```java
public class ReusableObjects {
    private static final ThreadLocal<StringBuilder> BUILDER = 
        ThreadLocal.withInitial(() -> new StringBuilder(256));
    
    public static String buildString(String... parts) {
        StringBuilder sb = BUILDER.get();
        sb.setLength(0);
        
        for (String part : parts) {
            sb.append(part);
        }
        return sb.toString();
    }
}
```

# Data Types & Operators

## Primitive Arrays

### One Dimensional Arrays

```java
// Declaration and initialization
int[] numbers = new int[5];           // Creates array of size 5, all elements = 0
int[] values = {1, 2, 3, 4, 5};      // Array literal
int[] data = new int[]{10, 20, 30};  // Explicit initialization

// Access and modification
numbers[0] = 100;        // Set first element
int first = numbers[0];  // Get first element
int length = numbers.length;  // Array length (property, not method)

// Common operations
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}

// Arrays are objects
int[] arr1 = {1, 2, 3};
int[] arr2 = arr1;  // Both reference same array
arr2[0] = 99;       // Changes arr1[0] as well
```

### Two Dimensional Arrays

```java
// Declaration methods
int[][] matrix = new int[3][4];              // 3 rows, 4 columns
int[][] grid = {{1, 2}, {3, 4}, {5, 6}};    // Irregular initialization
int[][] jagged = new int[3][];               // Jagged array - different row sizes

// Initialize jagged array
jagged[0] = new int[2];  // First row: 2 elements
jagged[1] = new int[4];  // Second row: 4 elements
jagged[2] = new int[1];  // Third row: 1 element

// Access elements
matrix[1][2] = 10;           // Row 1, Column 2
int value = matrix[1][2];    // Get value

// Iterate through 2D array
for (int row = 0; row < matrix.length; row++) {
    for (int col = 0; col < matrix[row].length; col++) {
        System.out.print(matrix[row][col] + " ");
    }
    System.out.println();
}
```

## For-Each Pattern

```java
// Arrays
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);  // num is copy, can't modify original
}

// Collections
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
for (String name : names) {
    System.out.println(name);
}

// 2D Arrays
int[][] matrix = {{1, 2}, {3, 4}, {5, 6}};
for (int[] row : matrix) {
    for (int element : row) {
        System.out.print(element + " ");
    }
    System.out.println();
}
```

## String Immutability

### Understanding Immutability

```java
String str = "Hello";
str.concat(" World");  // Returns new string, doesn't modify original
System.out.println(str);  // Still prints "Hello"

// Correct way
str = str.concat(" World");  // Assign returned value
System.out.println(str);  // Now prints "Hello World"

// Common mistake
String result = "";
for (int i = 0; i < 1000; i++) {
    result += i + ",";  // Creates 1000 intermediate String objects!
}
```

### Why Strings are Immutable

```java
// Security - can't change string after validation
public void processUser(String username) {
    if (isValid(username)) {
        // username can't be changed by another thread
        database.save(username);
    }
}

// String pool efficiency
String s1 = "Hello";
String s2 = "Hello";  // Points to same object in string pool
System.out.println(s1 == s2);  // true

// Hash code caching
String key = "myKey";
int hash1 = key.hashCode();  // Calculated once
int hash2 = key.hashCode();  // Cached value returned
```

### Working with Immutable Strings

```java
// Use StringBuilder for multiple concatenations
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++) {
    sb.append(i).append(",");
}
String result = sb.toString();

// String methods return new strings
String original = "  Hello World  ";
String trimmed = original.trim();        // New string
String upper = original.toUpperCase();   // New string
String replaced = original.replace("Hello", "Hi");  // New string

// Original string unchanged
System.out.println(original);  // Still "  Hello World  "
```

## Basic var Usage

```java
// Instead of explicit types
List<String> names = new ArrayList<String>();
Map<Integer, List<String>> groups = new HashMap<Integer, List<String>>();

// Use var for cleaner code
var names = new ArrayList<String>();  // Type inferred as ArrayList<String>
var groups = new HashMap<Integer, List<String>>();  // Type inferred

// Primitive types
var count = 10;        // int
var price = 19.99;     // double
var active = true;     // boolean
var letter = 'A';      // char
```

## Bitwise Operators

### Basic Bitwise Operations

```java
int a = 12;   // Binary: 1100
int b = 10;   // Binary: 1010

// AND (&) - both bits must be 1
int and = a & b;    // 1100 & 1010 = 1000 = 8

// OR (|) - at least one bit must be 1  
int or = a | b;     // 1100 | 1010 = 1110 = 14

// XOR (^) - bits must be different
int xor = a ^ b;    // 1100 ^ 1010 = 0110 = 6

// NOT (~) - flips all bits
int not = ~a;       // ~1100 = ...11110011 = -13 (two's complement)

System.out.println("AND: " + and);  // 8
System.out.println("OR: " + or);    // 14  
System.out.println("XOR: " + xor);  // 6
System.out.println("NOT: " + not);  // -13
```

### Practical Bitwise Applications

```java
// Check if number is even/odd
boolean isEven = (num & 1) == 0;  // Last bit is 0 for even numbers
boolean isOdd = (num & 1) == 1;   // Last bit is 1 for odd numbers

// Set specific bit (make it 1)
int setBit(int num, int position) {
    return num | (1 << position);
}

// Clear specific bit (make it 0)
int clearBit(int num, int position) {
    return num & ~(1 << position);
}

// Toggle specific bit
int toggleBit(int num, int position) {
    return num ^ (1 << position);
}

// Check if specific bit is set
boolean isBitSet(int num, int position) {
    return (num & (1 << position)) != 0;
}

// Example usage
int flags = 0;
flags = setBit(flags, 2);    // Set bit 2: 0100
flags = setBit(flags, 0);    // Set bit 0: 0101
boolean bit1Set = isBitSet(flags, 1);  // false
```

## Shift Operators

### Left Shift (<<)

```java
int num = 5;        // Binary: 101
int left1 = num << 1;  // 101 << 1 = 1010 = 10
int left2 = num << 2;  // 101 << 2 = 10100 = 20

// Left shift by n positions = multiply by 2^n
System.out.println(5 << 1);  // 5 * 2^1 = 10
System.out.println(5 << 2);  // 5 * 2^2 = 20
System.out.println(5 << 3);  // 5 * 2^3 = 40

// Fast multiplication by powers of 2
int fastMultiply = num << 3;  // Faster than num * 8
```

### Right Shift (>>)

```java
int num = 20;       // Binary: 10100
int right1 = num >> 1;  // 10100 >> 1 = 1010 = 10
int right2 = num >> 2;  // 10100 >> 2 = 101 = 5

// Right shift by n positions = divide by 2^n (integer division)
System.out.println(20 >> 1);  // 20 / 2^1 = 10
System.out.println(20 >> 2);  // 20 / 2^2 = 5

// Handles negative numbers (sign extension)
int negative = -8;  // Binary: ...11111000
int rightNeg = negative >> 1;  // ...11111100 = -4
```

## Ternary Operator

```java
// condition ? valueIfTrue : valueIfFalse
int a = 10, b = 20;
int max = (a > b) ? a : b;  // max = 20

String result = (score >= 60) ? "Pass" : "Fail";

// Equivalent if-else
int max2;
if (a > b) {
    max2 = a;
} else {
    max2 = b;
}
```

# Method Overloading, Static, and Inner Classes

## Method Overloading & Polymorphism

### Method Overloading

**Same method name, different parameters** in the same class.

```java
public class Calculator {
    // Different number of parameters
    public int add(int a, int b) {
        return a + b;
    }
    
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

### Polymorphism (Method Overriding)

**Same method signature in parent and child classes.**

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
    
    public void eat() {
        System.out.println("Animal eats");
    }
}

class Dog extends Animal {
    @Override
    public void makeSound() {  // Overriding parent method
        System.out.println("Dog barks");
    }
    
    // Overloading within same class
    public void makeSound(String intensity) {
        System.out.println("Dog barks " + intensity);
    }
}

// Runtime polymorphism
Animal animal = new Dog();  // Reference type: Animal, Object type: Dog
animal.makeSound();         // Calls Dog's version - "Dog barks"
animal.eat();              // Calls Animal's version

// Compile-time method resolution
Dog dog = new Dog();
dog.makeSound();           // Calls overridden version
dog.makeSound("loudly");   // Calls overloaded version
```

## Constructor Overloading

### Multiple Constructors

```java
public class Person {
    private String name;
    private int age;
    private String email;
    
    // Default constructor
    public Person() {
        this("Unknown", 0, "no-email");  // Constructor chaining
    }
    
    // Constructor with name only
    public Person(String name) {
        this(name, 0, "no-email");
    }
    
    // Constructor with name and age
    public Person(String name, int age) {
        this(name, age, "no-email");
    }
    
    // Full constructor
    public Person(String name, int age, String email) {
        this.name = name;
        this.age = age;
        this.email = email;
    }
}

// Usage
Person p1 = new Person();                          // Uses default
Person p2 = new Person("John");                    // Name only
Person p3 = new Person("Jane", 25);               // Name and age
Person p4 = new Person("Bob", 30, "bob@email.com"); // All parameters
```

### Constructor Chaining Rules

```java
public class Example {
    private int value;
    
    public Example() {
        this(10);  // Must be first statement
        // System.out.println("Hello");  // This would cause error
    }
    
    public Example(int value) {
        this.value = value;
        System.out.println("Constructor called");  // OK after this()
    }
}

// Inheritance constructor chaining
class Parent {
    public Parent(String message) {
        System.out.println("Parent: " + message);
    }
}

class Child extends Parent {
    public Child() {
        super("Default message");  // Must call parent constructor first
        System.out.println("Child constructor");
    }
    
    public Child(String message) {
        super(message);
        System.out.println("Child with message");
    }
}
```

## Understanding Static

### Static Variables (Class Variables)

```java
public class Counter {
    private static int count = 0;  // Shared among all instances
    private int instanceId;        // Unique per instance
    
    public Counter() {
        count++;                   // Increment class variable
        this.instanceId = count;   // Set instance variable
    }
    
    public static int getCount() {
        return count;
    }
    
    public int getInstanceId() {
        return instanceId;
    }
}

// Usage
Counter c1 = new Counter();  // count = 1
Counter c2 = new Counter();  // count = 2
Counter c3 = new Counter();  // count = 3

System.out.println(Counter.getCount());  // 3 - accessed via class name
System.out.println(c1.getInstanceId()); // 1
System.out.println(c2.getInstanceId()); // 2
```

### Static Methods

```java
public class MathUtils {
    // Static method - belongs to class, not instance
    public static int add(int a, int b) {
        return a + b;
    }
    
    // Static method can only access static members directly
    private static String className = "MathUtils";
    
    public static void printClassName() {
        System.out.println(className);        // OK - static variable
        // System.out.println(instanceVar);   // Error - can't access instance variable
        // instanceMethod();                  // Error - can't call instance method
    }
    
    // Instance method can access both static and instance members
    private String instanceVar = "instance";
    
    public void instanceMethod() {
        System.out.println(className);    // OK - can access static
        System.out.println(instanceVar); // OK - can access instance
        printClassName();                // OK - can call static method
    }
}

// Usage - no object creation needed
int result = MathUtils.add(5, 3);  // Called via class name
MathUtils.printClassName();
```

### Static Blocks

```java
public class Configuration {
    private static Properties config;
    private static String dbUrl;
    
    // Static block - runs when class is first loaded
    static {
        System.out.println("Loading configuration...");
        config = new Properties();
        try {
            config.load(new FileInputStream("config.properties"));
            dbUrl = config.getProperty("db.url");
        } catch (IOException e) {
            dbUrl = "default-url";
        }
        System.out.println("Configuration loaded");
    }
    
    // Multiple static blocks execute in order
    static {
        System.out.println("Second static block");
        validateConfiguration();
    }
    
    private static void validateConfiguration() {
        if (dbUrl == null) {
            throw new RuntimeException("DB URL not configured");
        }
    }
    
    public static String getDbUrl() {
        return dbUrl;
    }
}

// First access to class triggers static blocks
String url = Configuration.getDbUrl();  // Prints loading messages
```

### Static Inner Classes

```java
public class OuterClass {
    private String outerField = "outer";
    private static String staticOuterField = "static outer";
    
    // Static nested class
    public static class StaticNestedClass {
        public void display() {
            // Can access static members of outer class
            System.out.println(staticOuterField);  // OK
            
            // Cannot access instance members directly
            // System.out.println(outerField);     // Error
            
            // Need outer class instance to access instance members
            OuterClass outer = new OuterClass();
            System.out.println(outer.outerField);  // OK
        }
    }
    
    // Non-static inner class for comparison
    public class InnerClass {
        public void display() {
            System.out.println(outerField);        // OK - direct access
            System.out.println(staticOuterField);  // OK - can access static too
        }
    }
}

// Usage
// Static nested class - no outer instance needed
OuterClass.StaticNestedClass nested = new OuterClass.StaticNestedClass();
nested.display();

// Non-static inner class - needs outer instance
OuterClass outer = new OuterClass();
OuterClass.InnerClass inner = outer.new InnerClass();
inner.display();
```

### Static Memory Model

```java
public class MemoryExample {
    private static int staticVar = 100;     // Method Area (Metaspace)
    private int instanceVar = 200;          // Heap
    
    public static void staticMethod() {     // Method Area
        int localVar = 300;                 // Stack
    }
    
    public void instanceMethod() {          // Method Area (method code)
        int localVar = 400;                 // Stack
    }
}

/*
Memory Layout:
├── Method Area (Metaspace)
│   ├── Class metadata
│   ├── Static variables (staticVar = 100)
│   ├── Static methods (staticMethod)
│   └── Instance method code (instanceMethod)
├── Heap
│   └── Instance variables (instanceVar = 200)
└── Stack (per thread)
    └── Local variables (localVar)
*/
```

### Static Import

```java
// Static import for utility methods
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;
import static java.util.Collections.sort;

public class StaticImportExample {
    public void calculate() {
        double radius = 5.0;
        double area = PI * radius * radius;  // No need for Math.PI
        double side = sqrt(25);              // No need for Math.sqrt
        
        List<String> list = Arrays.asList("c", "a", "b");
        sort(list);                          // No need for Collections.sort
    }
}
```

## Nested and Inner Classes

### Types of Nested Classes

```java
public class OuterClass {
    private String outerField = "Outer";
    private static String staticField = "Static";
    
    // 1. Static Nested Class
    public static class StaticNested {
        public void method() {
            System.out.println(staticField);        // Can access static members
            // System.out.println(outerField);      // Cannot access instance members
        }
    }
    
    // 2. Non-static Inner Class (Member Inner Class)
    public class MemberInner {
        public void method() {
            System.out.println(outerField);         // Can access all outer members
            System.out.println(staticField);        // Can access static members
            System.out.println(OuterClass.this.outerField); // Explicit outer reference
        }
    }
    
    public void outerMethod() {
        // 3. Local Inner Class
        class LocalInner {
            public void method() {
                System.out.println(outerField);     // Can access outer members
                // Can access local variables if they are effectively final
            }
        }
        
        LocalInner local = new LocalInner();
        local.method();
        
        // 4. Anonymous Inner Class
        Runnable runnable = new Runnable() {
            @Override
            public void run() {
                System.out.println(outerField);     // Can access outer members
            }
        };
        
        // Modern anonymous class (lambda)
        Runnable lambda = () -> System.out.println(outerField);
    }
}
```

### Creating Nested Class Instances

```java
// Static nested class - no outer instance needed
OuterClass.StaticNested staticNested = new OuterClass.StaticNested();

// Member inner class - needs outer instance
OuterClass outer = new OuterClass();
OuterClass.MemberInner memberInner = outer.new MemberInner();

// Alternative syntax for member inner class
OuterClass.MemberInner memberInner2 = new OuterClass().new MemberInner();
```

### Local Inner Class with Variables

```java
public class LocalExample {
    public void method() {
        final String finalVar = "final";
        String effectivelyFinal = "effectively final";
        String notFinal = "not final";
        notFinal = "changed";  // Now not effectively final
        
        class LocalInner {
            public void display() {
                System.out.println(finalVar);           // OK
                System.out.println(effectivelyFinal);   // OK
                // System.out.println(notFinal);        // Error - not effectively final
            }
        }
        
        LocalInner inner = new LocalInner();
        inner.display();
    }
}
```

### Practical Use Cases

#### Builder Pattern with Static Nested Class

```java
public class Pizza {
    private final String dough;
    private final String sauce;
    private final String cheese;
    
    private Pizza(Builder builder) {
        this.dough = builder.dough;
        this.sauce = builder.sauce;
        this.cheese = builder.cheese;
    }
    
    public static class Builder {
        private String dough = "thin";
        private String sauce = "tomato";
        private String cheese = "mozzarella";
        
        public Builder dough(String dough) {
            this.dough = dough;
            return this;
        }
        
        public Builder sauce(String sauce) {
            this.sauce = sauce;
            return this;
        }
        
        public Builder cheese(String cheese) {
            this.cheese = cheese;
            return this;
        }
        
        public Pizza build() {
            return new Pizza(this);
        }
    }
}

// Usage
Pizza pizza = new Pizza.Builder()
    .dough("thick")
    .sauce("pesto")
    .cheese("parmesan")
    .build();
```

#### Event Handling with Anonymous Classes

```java
public class ButtonExample {
    public void setupButton() {
        Button button = new Button();
        
        // Anonymous class implementation
        button.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick() {
                System.out.println("Button clicked!");
                // Can access outer class members
            }
        });
        
        // Lambda equivalent (modern Java)
        button.setOnClickListener(() -> System.out.println("Button clicked!"));
    }
}
```

## Key Interview Points

### Method Overloading vs Overriding

- **Overloading**: Same class, same method name, different parameters
- **Overriding**: Inheritance, same method signature, runtime polymorphism

### Static Memory and Lifecycle

- **Static variables**: Created when class first loaded, shared among all instances
- **Static methods**: Belong to class, cannot access instance members directly
- **Static blocks**: Execute once when class is loaded, in order of appearance

### Inner Class Access Rules

- **Static nested**: Can only access static members of outer class
- **Member inner**: Can access all members of outer class
- **Local inner**: Can access effectively final local variables
- **Anonymous**: Same as local inner, often used for event handling

# Java Inheritance

## Inheritance Basics

### What is Inheritance?

**Definition**: A mechanism where one class acquires properties and methods from another class.

- **Parent Class**: Superclass/Base class (gives properties)
- **Child Class**: Subclass/Derived class (receives properties)
- **Keyword**: `extends`

```java
// Parent class
class Animal {
    String name;
    
    void eat() {
        System.out.println("Animal eats");
    }
}

// Child class inherits from Animal
class Dog extends Animal {
    void bark() {
        System.out.println("Dog barks");
    }
}

Dog dog = new Dog();
dog.name = "Buddy";  // Inherited from Animal
dog.eat();          // Inherited method
dog.bark();         // Own method
```

**Key Points:**

- Child gets all non-private members of parent
- Java supports only single inheritance (one parent)
- Use `extends` keyword to inherit

## Constructors & Inheritance

### Constructor Chaining Rules

**Rule 1**: Parent constructor is called before child constructor
**Rule 2**: If parent has no default constructor, child must explicitly call `super()`
**Rule 3**: `super()` must be first statement in child constructor

```java
class Parent {
    String name;
    
    Parent(String name) {  // No default constructor
        this.name = name;
        System.out.println("Parent constructor: " + name);
    }
}

class Child extends Parent {
    int age;
    
    Child(String name, int age) {
        super(name);  // MUST call parent constructor first
        this.age = age;
        System.out.println("Child constructor: " + age);
    }
}

Child c = new Child("John", 25);
// Output: Parent constructor: John
//         Child constructor: 25
```

**Important Notes:**

- If parent has default constructor, `super()` is automatically called
- `super()` calls parent constructor, `this()` calls another constructor in same class
- Constructor chaining ensures proper object initialization

### When Are Constructors Executed?

**Execution Order:**

1. Static blocks (class loading time)
2. Instance blocks and constructors (object creation time)
3. Parent → Child order

```java
class A {
    static { System.out.println("1. A static"); }
    { System.out.println("3. A instance block"); }
    A() { System.out.println("4. A constructor"); }
}

class B extends A {
    static { System.out.println("2. B static"); }
    { System.out.println("5. B instance block"); }
    B() { System.out.println("6. B constructor"); }
}

B obj = new B();  // Executes in order 1→2→3→4→5→6
```

**Memory Point**: Static blocks execute only once when class is first loaded, not on every object creation.

## Superclass References and Subclass Objects

### Reference vs Object Type

**Concept**: Reference type determines what methods you can CALL, Object type determines which method is EXECUTED.

```java
class Animal {
    void eat() { System.out.println("Animal eats"); }
}

class Dog extends Animal {
    void eat() { System.out.println("Dog eats"); }  // Override
    void bark() { System.out.println("Woof!"); }
}

// Different reference-object combinations
Animal a1 = new Animal();  // Animal reference, Animal object
Dog d1 = new Dog();        // Dog reference, Dog object
Animal a2 = new Dog();     // Animal reference, Dog object ← POLYMORPHISM

a1.eat();    // "Animal eats"
d1.eat();    // "Dog eats" 
a2.eat();    // "Dog eats" ← Runtime decides which eat() to call

d1.bark();   // OK - Dog reference can call Dog methods
// a2.bark();  // ERROR - Animal reference can't call Dog methods
```

**Key Interview Points:**

- **Upcasting**: Child → Parent reference (automatic)
- **Downcasting**: Parent → Child reference (requires explicit cast)
- **instanceof**: Check object type before casting

```java
if (a2 instanceof Dog) {
    Dog realDog = (Dog) a2;  // Safe downcasting
    realDog.bark();          // Now can call Dog methods
}
```

## Polymorphism (Dynamic Method Dispatch)

### How Runtime Polymorphism Works

**Definition**: Same method call behaves differently based on actual object type at runtime.

```java
class Shape {
    void draw() { System.out.println("Drawing shape"); }
    double area() { return 0; }
}

class Circle extends Shape {
    double radius;
    Circle(double r) { radius = r; }
    
    void draw() { System.out.println("Drawing circle"); }
    double area() { return Math.PI * radius * radius; }
}

class Square extends Shape {
    double side;
    Square(double s) { side = s; }
    
    void draw() { System.out.println("Drawing square"); }
    double area() { return side * side; }
}

// Polymorphism in action
Shape[] shapes = {new Circle(5), new Square(4), new Circle(3)};

for (Shape shape : shapes) {
    shape.draw();  // Calls appropriate draw() method
    System.out.println("Area: " + shape.area());
}
```

**Runtime Decision**: JVM looks at actual object type and calls the overridden method.

### Method Overriding Rules

**Must Follow:**

- Same method signature (name, parameters, return type)
- Same or wider access modifier
- Cannot override `final`, `static`, or `private` methods

```java
class Parent {
    protected String method() { return "Parent"; }
    final void finalMethod() { }      // Cannot override
    static void staticMethod() { }    // Cannot override (hidden instead)
    private void privateMethod() { }  // Cannot override (not inherited)
}

class Child extends Parent {
    public String method() { return "Child"; }  // ✓ Wider access (protected→public)
    // private String method() { }              // ✗ Narrower access
    // void finalMethod() { }                   // ✗ Cannot override final
}
```

## Abstract Classes

### Why Abstract Classes?

**Purpose**: Provide common base with some implemented methods and some that must be implemented by subclasses.

**When to Use:**

- When classes share common code but need different implementations for some methods
- Want to enforce certain methods in all subclasses
- Need constructors (interfaces can't have constructors)

```java
abstract class Vehicle {
    String brand;
    
    Vehicle(String brand) {  // Abstract classes can have constructors
        this.brand = brand;
    }
    
    // Concrete method - all vehicles can start the same way
    void start() {
        System.out.println(brand + " starting engine...");
    }
    
    // Abstract method - each vehicle accelerates differently
    abstract void accelerate();
    abstract void brake();
}

class Car extends Vehicle {
    Car(String brand) { super(brand); }
    
    void accelerate() { System.out.println("Car accelerating smoothly"); }
    void brake() { System.out.println("Car braking with disc brakes"); }
}

class Motorcycle extends Vehicle {
    Motorcycle(String brand) { super(brand); }
    
    void accelerate() { System.out.println("Motorcycle accelerating quickly"); }
    void brake() { System.out.println("Motorcycle braking carefully"); }
}

// Vehicle v = new Vehicle("Generic");  // ✗ Cannot instantiate abstract class
Vehicle car = new Car("Toyota");        // ✓ Can use abstract reference
car.start();       // Calls concrete method
car.accelerate();  // Calls overridden method
```

### Abstract Class vs Interface

**Abstract Class:**

- Can have both abstract and concrete methods
- Can have constructors and instance variables
- Single inheritance (`extends`)
- Use when classes share common code

**Interface:**

- All methods abstract (before Java 8)
- No constructors or instance variables
- Multiple inheritance (`implements`)
- Use for contracts/capabilities

```java
abstract class Animal {
    String name;                    // ✓ Instance variable
    Animal(String name) { }         // ✓ Constructor
    void sleep() { }               // ✓ Concrete method
    abstract void makeSound();     // ✓ Abstract method
}

interface Flyable {
    // String name;                // ✗ Cannot have instance variables
    // Flyable() { }              // ✗ Cannot have constructor
    void fly();                   // ✓ Abstract method
    
    // Java 8+
    default void land() { }       // ✓ Default method
    static void checkWeather() { } // ✓ Static method
}

class Bird extends Animal implements Flyable {
    Bird(String name) { super(name); }
    void makeSound() { System.out.println("Chirp"); }
    public void fly() { System.out.println("Flying high"); }
}
```

### Template Method Pattern

**Use Case**: Define algorithm structure in abstract class, let subclasses implement specific steps.

```java
abstract class DataProcessor {
    // Template method - defines the process
    public final void process() {  // final = cannot be overridden
        readData();
        validateData();
        processData();    // Abstract - subclasses implement
        saveData();
    }
    
    private void readData() { System.out.println("Reading data..."); }
    private void saveData() { System.out.println("Saving data..."); }
    
    // Hook method - subclasses can optionally override
    protected void validateData() { System.out.println("Basic validation"); }
    
    // Abstract method - subclasses must implement
    protected abstract void processData();
}

class CSVProcessor extends DataProcessor {
    protected void processData() {
        System.out.println("Processing CSV format");
    }
}

class XMLProcessor extends DataProcessor {
    protected void processData() {
        System.out.println("Processing XML format");
    }
    
    protected void validateData() {  // Override hook method
        System.out.println("XML schema validation");
    }
}
```

## Memory & Performance Notes

**Method Call Resolution:**

- **Compile time**: Check if method exists in reference type
- **Runtime**: Find actual method in object type (virtual method lookup)
- **Performance**: Virtual method calls have slight overhead vs direct calls

**Inheritance Memory Layout:**

- Child object contains parent's fields
- Method table includes both parent and child methods
- Overridden methods replace parent's method in table

# Final Keyword & Object Class

## Final Keyword

### What is Final?

**Definition**: `final` makes something unchangeable - variables, methods, or classes cannot be modified after
declaration.

### Final Variables

```java
// Final variable - cannot be reassigned
final int MAX_SIZE = 100;
// MAX_SIZE = 200;  // ERROR: Cannot assign to final variable

// Final object reference - reference cannot change, but object content can
final List<String> list = new ArrayList<>();
list.add("item");     // OK - modifying object content
// list = new ArrayList<>();  // ERROR - cannot reassign reference

// Final method parameter
public void process(final String name) {
    // name = "changed";  // ERROR: Cannot modify final parameter
    System.out.println(name);
}
```

### Final Methods

```java
class Parent {
    final void display() {  // Cannot be overridden
        System.out.println("Parent display");
    }
}

class Child extends Parent {
    // void display() { }  // ERROR: Cannot override final method
}
```

### Final Classes

```java
final class Utility {  // Cannot be extended
    static void helper() { }
}

// class MyUtility extends Utility { }  // ERROR: Cannot extend final class

// Examples: String, Integer, all wrapper classes are final
```

**Key Points:**

- **Variables**: Value cannot change (for primitives) or reference cannot change (for objects)
- **Methods**: Cannot be overridden by subclasses
- **Classes**: Cannot be extended (no inheritance)
- **Performance**: JVM can optimize final variables/methods

---

## Object Class & Its Methods

The `Object` class is the root class of all Java classes. Every class in Java directly or indirectly inherits from
`Object`.

### Object Class Methods

| Method                          | Return Type | Description                                    | Key Interview Points                                                                                                                                                                          |
|---------------------------------|-------------|------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `toString()`                    | `String`    | Returns string representation of object        | • Default: `className@hashCode`<br>• Should override for meaningful output<br>• Used by print statements automatically                                                                        |
| `equals(Object obj)`            | `boolean`   | Compares objects for equality                  | • Default: uses `==` (reference comparison)<br>• Override for logical equality<br>• Must satisfy: reflexive, symmetric, transitive, consistent<br>• If overridden, must override `hashCode()` |
| `hashCode()`                    | `int`       | Returns hash code value for object             | • Used by HashMap, HashSet, etc.<br>• Equal objects must have same hash code<br>• Should override when overriding `equals()`<br>• Contract: consistent, equal objects same hash               |
| `clone()`                       | `Object`    | Creates copy of object                         | • Protected method<br>• Class must implement `Cloneable` interface<br>• Throws `CloneNotSupportedException`<br>• Shallow copy by default                                                      |
| `getClass()`                    | `Class<?>`  | Returns runtime class of object                | • Final method (cannot override)<br>• Used for reflection<br>• Returns `Class` object representing the class                                                                                  |
| `finalize()`                    | `void`      | Called by garbage collector before destruction | • **Deprecated** since Java 9<br>• No guarantee when/if called<br>• Use try-with-resources instead<br>• Protected method                                                                      |
| `wait()`                        | `void`      | Current thread waits until notified            | • Must be called within synchronized block<br>• Releases lock on object<br>• Throws `InterruptedException`<br>• Has overloaded versions with timeout                                          |
| `wait(long timeout)`            | `void`      | Waits for specified time or until notified     | • Timeout in milliseconds<br>• Same synchronization requirements as `wait()`                                                                                                                  |
| `wait(long timeout, int nanos)` | `void`      | Waits with nanosecond precision                | • More precise timeout control<br>• Rarely used in practice                                                                                                                                   |
| `notify()`                      | `void`      | Wakes up single waiting thread                 | • Must be called within synchronized block<br>• Arbitrary thread selection<br>• No guarantee which thread wakes up                                                                            |
| `notifyAll()`                   | `void`      | Wakes up all waiting threads                   | • Must be called within synchronized block<br>• All threads compete for lock<br>• Generally preferred over `notify()`                                                                         |

# Packages & Class Members

## 📦 Java Packages

### What are Packages?

- **Definition**: Packages are namespaces that organize related classes and interfaces
- **Purpose**: Provide access protection, naming collision avoidance, and easier searching/locating of classes
- **Syntax**: `package com.company.project;`

### Package Hierarchy

- Java follows a hierarchical package structure
- **Root package**: `java` (contains all standard Java API classes)
- **Subpackages**: Organized by functionality

### Key Java API Packages

| Subpackage  | Description                                                     |
|-------------|-----------------------------------------------------------------|
| `java.lang` | Contains general-purpose classes (String, Object, System, etc.) |
| `java.io`   | Contains I/O classes (File, InputStream, OutputStream, etc.)    |
| `java.net`  | Contains networking classes (Socket, URL, etc.)                 |
| `java.util` | Contains utility classes and Collections Framework              |
| `java.awt`  | Contains Abstract Window Toolkit classes                        |

### Import Statements

```java
import java.util.List;           // Import specific class
import java.util.*;              // Import all classes from package
import static java.lang.Math.PI; // Static import
```

### Access Modifiers Explained

#### 1. **Private**

- Most restrictive
- Only accessible within the same class
- Not inherited by subclasses

```java
private int salary; // Only accessible within this class
```

#### 2. **Default (Package-Private)**

- No explicit modifier keyword
- Accessible within the same package only
- Not accessible from different packages

```java
int age; // Package-private access
```

#### 3. **Protected**

- Accessible within same package
- Accessible by subclasses in different packages
- More restrictive than public, less than default

```java
protected String name; // Accessible to subclasses
```

#### 4. **Public**

- Least restrictive
- Accessible from anywhere
- Can be accessed by any class in any package

```java
public void display(); // Accessible everywhere
```

# Java Interface

## 🔍 What is an Interface?

**Definition**: A contract that defines what a class can do, without specifying how it does it.

```java
interface Drawable {
    void draw(); // abstract method (implicitly public abstract)
    int SIZE = 100; // constant (implicitly public static final)
}
```

## 📋 Key Characteristics

- **100% abstraction** (before Java 8)
- **Multiple inheritance** support
- All methods are **public abstract** by default
- All variables are **public static final** by default
- Cannot be instantiated directly
- Implemented using `implements` keyword

## 🔧 Interface Evolution

### Before Java 8

```java
interface Calculator {
    int add(int a, int b);    // abstract method
    int PI = 3.14;           // constant
}
```

### Java 8+ Features

```java
interface Calculator {
    // Abstract method
    int add(int a, int b);
    
    // Default method
    default int multiply(int a, int b) {
        return a * b;
    }
    
    // Static method
    static void info() {
        System.out.println("Calculator interface");
    }
}
```

### Java 9+ Private Methods

```java
interface Calculator {
    default int addAndMultiply(int a, int b) {
        return helper(add(a, b), 2);
    }
    
    // Private method (Java 9+)
    private int helper(int x, int y) {
        return x * y;
    }
    
    int add(int a, int b);
}
```

## 🎯 Implementation

### Single Interface

```java
class Circle implements Drawable {
    public void draw() {
        System.out.println("Drawing circle");
    }
}
```

### Multiple Interfaces

```java
class SmartPhone implements Callable, Browsable {
    public void call() { /* implementation */ }
    public void browse() { /* implementation */ }
}
```

### Interface Inheritance

```java
interface Vehicle {
    void start();
}

interface Car extends Vehicle {
    void drive();
}

class BMW implements Car {
    public void start() { /* implementation */ }
    public void drive() { /* implementation */ }
}
```

# Java Exception Handling

## 🏗️ Exception Hierarchy

```
java.lang.Object
    └── java.lang.Throwable
        ├── java.lang.Error (Unchecked)
        │   ├── OutOfMemoryError
        │   ├── StackOverflowError
        │   └── VirtualMachineError
        └── java.lang.Exception
            ├── Checked Exceptions
            │   ├── IOException
            │   ├── SQLException
            │   ├── ClassNotFoundException
            │   └── InterruptedException
            └── java.lang.RuntimeException (Unchecked)
                ├── NullPointerException
                ├── ArrayIndexOutOfBoundsException
                ├── IllegalArgumentException
                └── NumberFormatException
```

### Exception Types

| Type          | Description             | Handling Required      |
|---------------|-------------------------|------------------------|
| **Checked**   | Compile-time exceptions | Must handle or declare |
| **Unchecked** | Runtime exceptions      | Optional handling      |
| **Error**     | System-level problems   | Usually not handled    |

## 🎯 Try and Catch

### Basic Syntax

```java
try {
    // Risky code
    int result = 10 / 0;
} catch (ArithmeticException e) {
    // Handle exception
    System.out.println("Cannot divide by zero: " + e.getMessage());
}
```

### Key Points

- **try block**: Contains code that might throw exception
- **catch block**: Handles specific exception types
- **Exception parameter**: Reference to the thrown exception object

## ⚠️ Effects of Uncaught Exception

### What Happens?

1. **Program terminates** abruptly
2. **Stack trace** is printed to console
3. **finally blocks** still execute before termination
4. **Resources** may not be properly cleaned up

### Example

```java
public class UncaughtExample {
    public static void main(String[] args) {
        System.out.println("Before exception");
        int x = 10 / 0; // ArithmeticException
        System.out.println("After exception"); // Never executed
    }
}
// Output: Exception in thread "main" java.lang.ArithmeticException: / by zero
```

## 🔒 Finally Block

### Always Executes (Almost)

```java
public class FinallyExample {
    public static void main(String[] args) {
        try {
            System.out.println("Try block");
            int x = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Catch block");
            return; // Finally still executes
        } finally {
            System.out.println("Finally block - Always executes");
            // Cleanup code here
        }
    }
}
```

### Finally Block Rules

- **Always executes** except when JVM exits (`System.exit()`)
- Executes even if **return statement** in try/catch
- **Exception in finally** masks exceptions from try/catch
- Used for **cleanup operations** (closing files, connections)

### Try-with-Resources (Java 7+)

```java
// Automatic resource management
try (FileReader file = new FileReader("data.txt");
     BufferedReader buffer = new BufferedReader(file)) {
    
    return buffer.readLine();
    
} catch (IOException e) {
    System.out.println("File error: " + e.getMessage());
}
// Resources automatically closed
```

## 🎯 Interview Questions & Answers

### Q1: What's the difference between throw and throws?

**A**: `throw` is used to explicitly throw an exception in code, while `throws` is used in method signature to declare
what exceptions the method might throw.

### Q2: Can finally block prevent an exception from propagating?

**A**: Yes, if finally block throws an exception or contains a return statement, it can mask the original exception.

### Q3: What happens if both try and finally blocks throw exceptions?

**A**: The exception from finally block suppresses the exception from try block. The try block exception becomes a "
suppressed exception."

### Q4: Can we have try without catch?

**A**: Yes, with finally block: `try { } finally { }` or with try-with-resources.

### Q5: What's the difference between Error and Exception?

**A**: Errors are serious system-level problems (OutOfMemoryError), while Exceptions are application-level problems that
can be handled.

### Q6: When should you create checked vs unchecked custom exceptions?

**A**:

- **Checked**: When caller can reasonably recover from the exception
- **Unchecked**: For programming errors or when recovery is unlikely

# Java I/O Streams

## 🔧 Low-Level I/O Fundamentals

### What Actually Happens When You Read a File?

**Understanding "Opening" a File:**
- **File on disk**: Just bytes stored on storage device
- **Opening a file**: OS creates internal data structures to track your access to that file
- **File descriptor**: OS assigns a unique number (like 3, 4, 5...) to identify this open file
- **File table entry**: OS maintains metadata about the open file (current position, permissions, etc.)

**System Level Process:**
1. **File**: A sequence of bytes stored on disk with metadata (permissions, size, timestamps)
2. **open() system call**: Your program asks OS "please give me access to this file"
3. **File Descriptor**: OS assigns a number (like file descriptor #7) to identify the open file
4. **File Table**: OS creates internal record tracking this open file
5. **System Call**: Your program uses file descriptor to ask OS to read/write
6. **Kernel**: OS kernel manages actual hardware interaction
7. **Buffer**: OS uses buffers to optimize disk access

**What "File Descriptor" Really Means:**
```
Your Program          Operating System
-----------          ----------------
FileInputStream  →   File Descriptor #7  →  Internal File Table Entry
                     (just a number)        - file path: /home/user/data.txt
                                           - current position: 1024 bytes
                                           - permissions: read-only
                                           - buffer: 4KB cache
```

```java
// When you write this Java code:
FileInputStream fis = new FileInputStream("data.txt");
int data = fis.read();

// This happens under the hood:
// 1. JVM calls OS open() system call with file path
// 2. OS checks permissions, locates file on disk
// 3. OS creates internal file table entry
// 4. OS returns file descriptor number (e.g., 7)
// 5. JVM stores this descriptor in FileInputStream object
// 6. When you call read(), JVM uses descriptor to call OS read()
// 7. OS reads from disk into kernel buffer
// 8. Data copied from kernel buffer to JVM memory
// 9. Your program gets the byte
```

**Why "Too Many Open Files" Happens:**
```
Process File Descriptor Table (Limited Size)
┌─────┬─────────────────┬──────────────────┐
│ FD# │ File            │ Status           │
├─────┼─────────────────┼──────────────────┤
│ 0   │ stdin           │ Standard         │
│ 1   │ stdout          │ Standard         │  
│ 2   │ stderr          │ Standard         │
│ 3   │ /data/file1.txt │ Your program     │
│ 4   │ /data/file2.txt │ Your program     │
│ 5   │ /data/file3.txt │ Your program     │
│ ... │ ...             │ ...              │
│1023 │ /data/file1021.txt│ Your program   │
│1024 │ LIMIT REACHED!  │ ❌ ERROR         │
└─────┴─────────────────┴──────────────────┘

// When you try to open more files:
FileInputStream fis = new FileInputStream("another-file.txt");
// Throws: IOException: Too many open files
```

**Each "Open File" Consumes:**
- **File descriptor number** (limited per process, typically 1024)
- **Memory for file table entry** (few KB per file)
- **OS kernel resources** (buffers, locks, metadata)
- **System-wide file table entries** (shared limit across all processes)

### System Calls vs Java Streams
| Level            | What It Does                                | Example                                  |
|------------------|---------------------------------------------|------------------------------------------|
| **Hardware**     | Physical disk operations                    | Disk head movement, sector reading       |
| **OS Kernel**    | Manages hardware, provides system calls     | `open()`, `read()`, `write()`, `close()` |
| **JVM**          | Translates Java calls to system calls       | Native methods in FileInputStream        |
| **Java Streams** | Object-oriented wrapper around system calls | `new FileInputStream()`                  |

### Why Buffering Matters
```java
// INEFFICIENT - Each read() = one system call
FileInputStream fis = new FileInputStream("file.txt");
int data;
while ((data = fis.read()) != -1) { // 1000 bytes = 1000 system calls!
    process(data);
}

// EFFICIENT - Fewer system calls
BufferedInputStream bis = new BufferedInputStream(fis, 8192);
while ((data = bis.read()) != -1) { // 1000 bytes = ~1 system call
    process(data);
}
```

## 🎯 Top 10 Java I/O Interview Questions

### Q1: "What's the difference between byte streams and character streams?"
**Answer:**
```java
// Byte streams - for binary data (images, executables, compressed files)
FileInputStream fis = new FileInputStream("image.jpg");
FileOutputStream fos = new FileOutputStream("backup.jpg");

// Character streams - for text data with encoding support
FileReader fr = new FileReader("document.txt");
FileWriter fw = new FileWriter("output.txt");
```
**Key Point**: Byte streams work with raw 8-bit data, character streams handle 16-bit Unicode with automatic encoding/decoding.

### Q2: "How do you prevent resource leaks in Java?"
**Answer:**
```java
// OLD WAY - Manual cleanup (error-prone)
FileInputStream fis = null;
try {
    fis = new FileInputStream("data.txt");
    // process file
} finally {
    if (fis != null) fis.close(); // Must remember to close
}

// MODERN WAY - Try-with-resources (automatic cleanup)
try (FileInputStream fis = new FileInputStream("data.txt")) {
    // process file
} // Automatically closed, even if exception occurs
```
**Key Point**: Try-with-resources calls `close()` automatically on anything implementing `AutoCloseable`.

### Q3: "Why use BufferedInputStream instead of FileInputStream directly?"
**Answer:**
```java
// Without buffering - Many system calls
FileInputStream fis = new FileInputStream("large-file.dat");
int data;
while ((data = fis.read()) != -1) { // Each read() = system call
    process(data);
}

// With buffering - Fewer system calls, better performance
BufferedInputStream bis = new BufferedInputStream(fis, 8192); // 8KB buffer
while ((data = bis.read()) != -1) { // Reads in chunks
    process(data);
}
```
**Key Point**: Buffering reduces the number of expensive system calls by reading/writing data in larger chunks.

### Q4: "How do you read and write primitive data types to files?"
**Answer:**
```java
// Writing different data types
try (DataOutputStream dos = new DataOutputStream(
        new FileOutputStream("data.bin"))) {
    dos.writeInt(42);
    dos.writeDouble(3.14159);
    dos.writeUTF("Hello World");
    dos.writeBoolean(true);
}

// Reading back in same order
try (DataInputStream dis = new DataInputStream(
        new FileInputStream("data.bin"))) {
    int number = dis.readInt();
    double pi = dis.readDouble();
    String text = dis.readUTF();
    boolean flag = dis.readBoolean();
}
```
**Key Point**: `DataInputStream`/`DataOutputStream` handle platform-independent binary format for primitive types.

### Q5: "What's the difference between FileReader and InputStreamReader?"
**Answer:**
```java
// FileReader - Uses default system encoding
FileReader fr = new FileReader("file.txt"); // Might be ISO-8859-1 or UTF-8

// InputStreamReader - Explicit encoding control
InputStreamReader isr = new InputStreamReader(
    new FileInputStream("file.txt"), StandardCharsets.UTF_8);
```
**Key Point**: `InputStreamReader` gives you control over character encoding, preventing corruption of international text.

### Q6: "How do you safely convert strings to numbers?"
**Answer:**
```java
public static Integer safeParseInt(String str) {
    try {
        return Integer.parseInt(str);
    } catch (NumberFormatException e) {
        System.err.println("Invalid number: " + str);
        return null; // or return default value
    }
}

// Usage
String userInput = "abc123";
Integer result = safeParseInt(userInput);
if (result != null) {
    // Use the number
} else {
    // Handle invalid input
}
```
**Key Point**: Always handle `NumberFormatException` when parsing user input or file data.

### Q7: "What's RandomAccessFile and when would you use it?"
**Answer:**
```java
try (RandomAccessFile raf = new RandomAccessFile("data.txt", "rw")) {
    // Write at beginning
    raf.writeUTF("Header");
    
    // Jump to position 100
    raf.seek(100);
    raf.writeUTF("Middle content");
    
    // Jump back to beginning to read
    raf.seek(0);
    String header = raf.readUTF();
    
    // Get file length
    long size = raf.length();
}
```
**Key Point**: Use for database files, log files, or any scenario where you need to read/write at specific positions.

### Q8: "How do you read a file line by line efficiently?"
**Answer:**
```java
// Efficient line-by-line reading
try (BufferedReader br = new BufferedReader(new FileReader("large-file.txt"))) {
    String line;
    while ((line = br.readLine()) != null) {
        processLine(line); // Process one line at a time
    }
}

// DON'T do this with large files
List<String> allLines = Files.readAllLines(Paths.get("large-file.txt")); // OutOfMemoryError!
```
**Key Point**: `BufferedReader.readLine()` is memory-efficient for large files, unlike loading everything into memory.

### Q9: "What are the predefined streams in Java?"
**Answer:**
```java
// Standard input (keyboard)
Scanner scanner = new Scanner(System.in);
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

// Standard output (console)
System.out.println("Normal output");

// Standard error (console, but separate stream)
System.err.println("Error output");

// Redirecting streams
System.setOut(new PrintStream("output.txt"));
System.setErr(new PrintStream("errors.txt"));
```
**Key Point**: `System.in`, `System.out`, `System.err` are automatically available and can be redirected.

### Q10: "What happens if you don't close a stream? Why is 'too many open files' a problem?"
**Answer:**
```java
// Resource leak example
public void processFiles() {
    for (int i = 0; i < 2000; i++) {
        try {
            FileInputStream fis = new FileInputStream("file" + i + ".txt");
            // Process file
            // BUG: Never call fis.close()!
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    // After ~1024 iterations: "Too many open files" error
}
```

**What "Too Many Open Files" Actually Means:**
- **OS Limit**: Each process can only have ~1024 open file descriptors
- **File descriptor**: OS assigns unique number to each open file (0=stdin, 1=stdout, 2=stderr, 3+=your files)
- **Resource exhaustion**: When you hit the limit, OS refuses to open more files
- **System impact**: Affects entire system, not just your program

**How to diagnose:**
```bash
# Check current open files for your Java process
lsof -p <java-process-id> | wc -l

# Check system limits
ulimit -n    # Shows max open files per process (usually 1024)
```

## 🚨 Common Mistakes & How to Avoid Them

### 1. Using Wrong Stream Type
```java
// WRONG - Using character stream for binary data
FileReader fr = new FileReader("image.jpg"); // Corrupts binary data!

// CORRECT - Use byte stream for binary data
FileInputStream fis = new FileInputStream("image.jpg");
```

### 2. Forgetting to Flush
```java
// Data might stay in buffer
FileWriter fw = new FileWriter("output.txt");
fw.write("Important data");
// If program crashes here, data is lost!

// Always flush or use try-with-resources
fw.flush(); // Or fw.close() which calls flush()
```

### 3. Inefficient File Reading
```java
// SLOW - Reading byte by byte
FileInputStream fis = new FileInputStream("large-file.dat");
int data;
while ((data = fis.read()) != -1) { // Many system calls
    process(data);
}

// FAST - Reading in chunks
byte[] buffer = new byte[8192];
int bytesRead;
while ((bytesRead = fis.read(buffer)) != -1) {
    for (int i = 0; i < bytesRead; i++) {
        process(buffer[i]);
    }
}
```

# Enums, Autoboxing, Static Import & More

## 1. Enumeration Fundamentals

### What are Enumerations?
- **Definition**: A special Java type that defines a collection of constants
- **Syntax**: `enum EnumName { CONSTANT1, CONSTANT2, CONSTANT3 }`
- **Key Points**:
  - Enums are implicitly `public`, `static`, and `final`
  - Each enum constant is an instance of the enum type
  - Enums cannot be instantiated using `new`
  - Enums can be used in switch statements

### Basic Example:
```java
enum Day {
    MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY
}

public class EnumExample {
    public static void main(String[] args) {
        Day today = Day.MONDAY;
        System.out.println("Today is: " + today);
    }
}
```

## 2. Class-Based Features of Enumerations

### Enums as Classes
- Enums are special classes that extend `java.lang.Enum`
- Can have fields, constructors, and methods
- Constructor is called once for each enum constant

### Example with Fields and Methods:
```java
enum Planet {
    MERCURY(3.303e+23, 2.4397e6),
    VENUS(4.869e+24, 6.0518e6),
    EARTH(5.976e+24, 6.37814e6);
    
    private final double mass;
    private final double radius;
    
    Planet(double mass, double radius) {
        this.mass = mass;
        this.radius = radius;
    }
    
    public double getMass() { return mass; }
    public double getRadius() { return radius; }
    
    public double surfaceGravity() {
        return 6.67300E-11 * mass / (radius * radius);
    }
}
```

### Interview Questions:
- **Q**: Can enums implement interfaces?
- **A**: Yes, enums can implement interfaces but cannot extend classes (they already extend Enum)

## 3. values() and valueOf() Methods

### values() Method
- **Purpose**: Returns an array containing all enum constants
- **Automatically generated** by the compiler
- **Usage**: Iterating through all enum constants

```java
for (Day day : Day.values()) {
    System.out.println(day);
}
```

### valueOf() Method
- **Purpose**: Returns the enum constant with the specified name
- **Throws**: `IllegalArgumentException` if no constant with specified name exists
- **Case-sensitive**

```java
Day day = Day.valueOf("MONDAY"); // Returns Day.MONDAY
Day invalid = Day.valueOf("monday"); // Throws IllegalArgumentException
```

### Interview Questions:
- **Q**: What happens if you pass null to valueOf()?
- **A**: Throws `NullPointerException`

## 4. Java's Type Wrappers

### Wrapper Classes Overview
- **Purpose**: Object representation of primitive types
- **Complete List**:
  - `Boolean` (boolean)
  - `Byte` (byte)
  - `Character` (char)
  - `Short` (short)
  - `Integer` (int)
  - `Long` (long)
  - `Float` (float)
  - `Double` (double)

### Key Features:
```java
// Creating wrapper objects
Integer intObj = new Integer(42);        // Deprecated in Java 9
Integer intObj2 = Integer.valueOf(42);   // Preferred way

// Utility methods
int parsed = Integer.parseInt("123");
String str = Integer.toString(456);
Integer max = Integer.max(10, 20);
```

### Interview Questions:
- **Q**: What's the difference between `Integer.valueOf()` and `new Integer()`?
- **A**: `valueOf()` uses caching (-128 to 127) and is more memory efficient; constructor always creates new object

## 7. Autoboxing and Auto-unboxing Basics

### Autoboxing
- **Definition**: Automatic conversion from primitive to wrapper object
- **When it happens**: Assignment, method parameters, return values

```java
Integer intObj = 42;        // Autoboxing: int to Integer
List<Integer> list = new ArrayList<>();
list.add(10);              // Autoboxing: int to Integer
```

### Auto-unboxing
- **Definition**: Automatic conversion from wrapper object to primitive
- **When it happens**: Assignment, method parameters, arithmetic operations

```java
Integer intObj = 42;
int primitive = intObj;     // Auto-unboxing: Integer to int
int result = intObj + 5;    // Auto-unboxing for arithmetic
```

## 10. Static Import

### Basic Syntax:
```java
import static java.lang.Math.*;
import static java.lang.System.out;

public class StaticImportExample {
    public static void main(String[] args) {
        out.println(sqrt(16));    // Instead of System.out.println(Math.sqrt(16))
        out.println(PI);          // Instead of Math.PI
    }
}
```

### Specific vs. Wildcard Import:
```java
// Specific static import
import static java.lang.Math.sqrt;
import static java.lang.Math.PI;

// Wildcard static import
import static java.lang.Math.*;
```

### Best Practices:
- Use sparingly to avoid confusion
- Prefer specific imports over wildcards
- Don't overuse; can make code less readable

### Interview Questions:
- **Q**: What's the difference between regular import and static import?
- **A**: Regular import imports types; static import imports static members (methods and fields)

## 11. Annotations Overview

### Basic Concepts:
- **Definition**: Metadata that provides information about code
- **Usage**: Compilation, runtime processing, documentation
- **Syntax**: `@AnnotationName` or `@AnnotationName(parameters)`

### Built-in Annotations:
```java
@Override
public String toString() {
    return "Example";
}

@Deprecated
public void oldMethod() {
    // Legacy code
}

@SuppressWarnings("unchecked")
public void rawTypeMethod() {
    List list = new ArrayList(); // Raw type usage
}
```

### Custom Annotations:
```java
@interface MyAnnotation {
    String value() default "default";
    int count() default 1;
}

@MyAnnotation(value = "test", count = 5)
public class AnnotatedClass {
    // Class implementation
}
```

### Retention Policies:
- `@Retention(RetentionPolicy.SOURCE)`: Compile-time only
- `@Retention(RetentionPolicy.CLASS)`: In class file, not at runtime
- `@Retention(RetentionPolicy.RUNTIME)`: Available at runtime

### Interview Questions:
- **Q**: What's the difference between `@Override` and `@Overload`?
- **A**: `@Override` exists and indicates method overriding; `@Overload` doesn't exist in Java

## 12. instanceof Operator

### Basic Usage:
```java
Object obj = "Hello";
if (obj instanceof String) {
    String str = (String) obj;
    System.out.println(str.toUpperCase());
}
```

### With Inheritance:
```java
class Animal { }
class Dog extends Animal { }

Animal animal = new Dog();
System.out.println(animal instanceof Dog);    // true
System.out.println(animal instanceof Animal); // true
System.out.println(animal instanceof Object); // true
```

### Null Handling:
```java
String str = null;
System.out.println(str instanceof String); // false (null is not instance of anything)
```

### Best Practices:
- Use before casting to avoid `ClassCastException`
- Consider using polymorphism instead of multiple instanceof checks
- Be careful with null values

### Interview Questions:
- **Q**: What does `instanceof` return when the object is null?
- **A**: Always returns false, regardless of the type being checked

## Common Interview Scenarios

### Enum in Switch Statement:
```java
enum Day { MONDAY, TUESDAY, WEDNESDAY }

public String getWorkload(Day day) {
    switch (day) {
        case MONDAY:
            return "Heavy";
        case TUESDAY:
            return "Medium";
        case WEDNESDAY:
            return "Light";
        default:
            return "Unknown";
    }
}
```

### Autoboxing Performance Issue:
```java
// Poor performance due to autoboxing
Long sum = 0L;
for (long i = 0; i < 1000000; i++) {
    sum += i; // Creates new Long object in each iteration
}

// Better performance
long sum = 0L;
for (long i = 0; i < 1000000; i++) {
    sum += i; // Pure primitive arithmetic
}
```

### Static Import Conflicts:
```java
import static java.lang.Math.max;
import static java.lang.Integer.max; // Compilation error: duplicate static import

// Solution: Use specific imports or qualify the method
Math.max(a, b);
Integer.max(x, y);
```

# Java Generics

## 1. Benefits of Generics
- **Type Safety**: Compile-time error instead of runtime `ClassCastException`
- **No Casting**: Eliminates explicit casting when retrieving from collections
- **Generic Algorithms**: Write methods that work with any type

```java
// Without generics - runtime error
List list = new ArrayList();
list.add("Hello");
String str = (String) list.get(0); // Cast required

// With generics - compile-time safety
List<String> list = new ArrayList<String>();
list.add("Hello");
String str = list.get(0); // No cast needed
```

**Q**: What are main benefits of generics?
**A**: Type safety at compile-time, elimination of casts, enabling generic algorithms

## 2. Generic Class
```java
public class Box<T> {
    private T content;
    
    public void set(T content) { this.content = content; }
    public T get() { return content; }
}

// Multiple type parameters
public class Pair<T, U> {
    private T first;
    private U second;
    // constructors and methods...
}
```

## 3. Bounded Type Parameters
```java
// Upper bound - T must extend Number
public class NumberBox<T extends Number> {
    private T number;
    public double getDoubleValue() {
        return number.doubleValue(); // Can call Number methods
    }
}

// Multiple bounds
public class BoundedBox<T extends Number & Comparable<T>> {
    // T must extend Number AND implement Comparable
}
```

**Q**: What does `<T extends Number>` mean?
**A**: T must be Number or its subclass (upper bound)

## 4. Wildcards
```java
// Unbounded wildcard
public void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}

// Upper bounded wildcard (? extends)
public double sum(List<? extends Number> numbers) {
    double sum = 0.0;
    for (Number num : numbers) {
        sum += num.doubleValue();
    }
    return sum;
}

// Lower bounded wildcard (? super)
public void addNumbers(List<? super Integer> list) {
    list.add(42); // Can add Integer
}
```

**PECS Rule**: **P**roducer **E**xtends, **C**onsumer **S**uper
- Use `? extends` when reading from collection (producer)
- Use `? super` when writing to collection (consumer)

## 5. Generic Methods
```java
public class Utility {
    // Generic method in non-generic class
    public static <T> void swap(T[] array, int i, int j) {
        T temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
    
    // With bounds
    public static <T extends Comparable<T>> T max(T a, T b) {
        return a.compareTo(b) > 0 ? a : b;
    }
}
```

## 6. Diamond Operator (Java 7+)
```java
// Before Java 7
List<String> list = new ArrayList<String>();

// Java 7+ with diamond operator
List<String> list = new ArrayList<>();
Map<String, List<Integer>> map = new HashMap<>();
```

## 7. Type Erasure
- Generic type information is removed at compile time
- All generic types become raw types or their bounds

```java
// Source code
List<String> stringList = new ArrayList<String>();
List<Integer> intList = new ArrayList<Integer>();

// After erasure (runtime)
List stringList = new ArrayList();
List intList = new ArrayList();

// This won't work - same type at runtime
if (stringList instanceof List<String>) { // Compilation error
    // Cannot check parameterized type
}
```

**Q**: What is type erasure?
**A**: Process where generic type information is removed during compilation for backward compatibility

## 8. Raw Types
```java
// Raw type (avoid in new code)
List rawList = new ArrayList();
rawList.add("Hello");
rawList.add(42); // No compile-time check

// Parameterized type (preferred)
List<String> stringList = new ArrayList<String>();
```

**Q**: Difference between `List` and `List<Object>`?
**A**: `List` is raw type (no type checking), `List<Object>` is parameterized type (type-safe)

## 9. Key Restrictions
- **No primitives**: `List<int>` ❌ → `List<Integer>` ✅
- **No arrays**: `new T[10]` ❌
- **No static fields**: `static T field` ❌
- **No instanceof**: `obj instanceof List<String>` ❌

## 10. Common Interview Questions

**Q**: Can you overload methods with different generic parameters?
**A**: No, `process(List<String>)` and `process(List<Integer>)` have same erasure signature

**Q**: Why can't you create generic arrays?
**A**: Arrays need runtime type info, but generics use type erasure

**Q**: When to use `<? extends T>` vs `<? super T>`?
**A**: Use `extends` for reading (producer), `super` for writing (consumer) - PECS rule

**Q**: What's the difference between `List<?>` and `List<Object>`?
**A**: `List<?>` can reference any parameterized list, `List<Object>` only accepts Object references

**Q**: Can static methods be generic?
**A**: Yes, but they can't use class type parameters, only their own: `public static <T> void method(T t)`

