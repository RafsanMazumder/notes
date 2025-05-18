# Solid Principles in Java

**SOLID** is an acronym that stands for five design principles:

1. **Single Responsibility Principle (SRP)**: A class should have only one reason to change, meaning that a class should
   only have one job or responsibility.
2. **Open/Closed Principle (OCP)**: Software entities (classes, modules, functions, etc.) should be open for extension
   but closed for modification.
3. **Liskov Substitution Principle (LSP)**: Subtypes must be substitutable for their base types.
4. **Interface Segregation Principle (ISP)**: Clients should not be forced to depend on interfaces they do not use.
5. **Dependency Inversion Principle (DIP)**: High-level modules should not depend on low-level modules. Both should
   depend on abstractions.

## Single Responsibility Principle (SRP)

**Principle**: A class should have only one reason to change, meaning it should have only one job or responsibility.

**Bad Example**:

```java
public class Employee {
    private String name;
    private String id;

    // Employee data methods
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }

    // Database Operations
    public void saveToDatabase() {
        // Database code here
    }

    // Report generation
    public void generateReport() {
        // Report generation code here
    }
}
```

**Good Example**:

```java
// Employee class only manages employee data
public class Employee {
    private String name;
    private String id;

    // Employee data methods
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
}

// Separate class for database operations
public class EmployeeRepository {
    public void save(Employee employee) {
        // Database code here
    }
}

// Separate class for reporting
public class EmployeeReportGenerator {
    public void generateReport(Employee employee) {
        // Report generation code
    }
}
```

## Open/Closed Principle (OCP)

**Principle**: Software entities (classes, modules, functions, etc.) should be open for extension but closed for
modification.

**Bad Example**:

```java
public class PaymentProcessor {
    public void processPayment(String paymentType, double amount) {
        if (paymentType.equals("credit_card")) {
            // Process credit card payment
        } else if (paymentType.equals("paypal")) {
            // Process PayPal payment
        } else if (paymentType.equals("bitcoin")) {
            // Process Bitcoin payment
        }
        // Need to modify this class when adding new payment types
    }
}
```

**Good Example**:

```java
// Interface
public interface PaymentMethod {
    void processPayment(double amount);
}

// Implementations
public class CreditCardPayment implements PaymentMethod {
    @Override
    public void processPayment(double amount) {
        // Process credit card payment
    }
}

public class PayPalPayment implements PaymentMethod {
    @Override
    public void processPayment(double amount) {
        // Process PayPal payment
    }
}

public class BitcoinPayment implements PaymentMethod {
    @Override
    public void processPayment(double amount) {
        // Process Bitcoin payment
    }
}

// Client code - closed for modification
public class PaymentProcessor {
    public void processPayment(PaymentMethod paymentMethod, double amount) {
        paymentMethod.processPayment(amount);
    }
}
```

## Liskov Substitution Principle (LSP)

**Principle**: Subtypes must be substitutable for their base types without altering the correctness of the program.

**Bad Example**:

```java
// BAD: Ostrich cannot fly, violates the "Bird can fly" expectation
class Bird {
    public void fly() {
        System.out.println("Bird is flying");
    }
}

class Sparrow extends Bird {
    // Sparrow can fly, consistent with Bird
}

class Ostrich extends Bird {
    @Override
    public void fly() {
        // Ostriches cannot fly. This breaks the expectation.
        throw new UnsupportedOperationException("Ostrich cannot fly!");
        // Or: System.out.println("Ostrich cannot fly"); // Still problematic
    }
}

// Client code
// public void makeBirdFly(Bird bird) {
//     bird.fly(); // This will crash if 'bird' is an Ostrich
// }
```

**Good Example**:

```java
// GOOD: Separate flying behavior
interface Flyable {
    void fly();
}

abstract class Bird { // General bird properties
    abstract void makeSound();
    public void eat() { System.out.println("Bird is eating."); }
}

class Sparrow extends Bird implements Flyable { // Sparrow IS-A Bird and IS Flyable
    @Override public void makeSound() { System.out.println("Chirp"); }
    @Override public void fly() { System.out.println("Sparrow is flying high."); }
}

class Ostrich extends Bird { // Ostrich IS-A Bird, but NOT Flyable
    @Override public void makeSound() { System.out.println("Boom"); }
    // No fly() method, or a run() method specific to Ostrich
    public void run() { System.out.println("Ostrich is running fast."); }
}

class Penguin extends Bird { // Penguin is a bird but doesn't fly
    @Override public void makeSound() { System.out.println("Squawk"); }
    public void swim() { System.out.println("Penguin is swimming."); }
}
```

## Interface Segregation Principle (ISP)

**Principle**: Clients should not be forced to depend on interfaces they do not use.

**Bad Example**:

```java
// Fat interface
public interface Worker {
    void work();
    void eat();
    void sleep();
}

// Problem: Robot can't eat or sleep
public class Robot implements Worker {
    @Override
    public void work() {
        // Working
    }
    
    @Override
    public void eat() {
        // Robot can't eat, but forced to implement
        throw new UnsupportedOperationException();
    }
    
    @Override
    public void sleep() {
        // Robot can't sleep, but forced to implement
        throw new UnsupportedOperationException();
    }
}
```

**Good Example**:

```java
// Segregated interfaces
public interface Workable {
    void work();
}

public interface Eatable {
    void eat();
}

public interface Sleepable {
    void sleep();
}

// Human implements all interfaces
public class Human implements Workable, Eatable, Sleepable {
    @Override
    public void work() {
        // Human working
    }
    
    @Override
    public void eat() {
        // Human eating
    }
    
    @Override
    public void sleep() {
        // Human sleeping
    }
}

// Robot only implements what it needs
public class Robot implements Workable {
    @Override
    public void work() {
        // Robot working
    }
}
```

## Dependency Inversion Principle (DIP)

**Principle**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

**Bad Example**:

```java
// Low-level module
public class MySQLDatabase {
    public void insert(String data) {
        // Insert data to MySQL
    }
}

// High-level module depends on low-level module
public class UserService {
    private MySQLDatabase database;
    
    public UserService() {
        this.database = new MySQLDatabase(); // Hard dependency
    }
    
    public void addUser(String userData) {
        database.insert(userData);
    }
}
```

**Good Example**:

```java
// Abstraction
public interface Database {
    void insert(String data);
}

// Low-level module implements abstraction
public class MySQLDatabase implements Database {
    @Override
    public void insert(String data) {
        // Insert data to MySQL
    }
}

// Alternative implementation
public class MongoDatabase implements Database {
    @Override
    public void insert(String data) {
        // Insert data to MongoDB
    }
}

// High-level module depends on abstraction
public class UserService {
    private Database database;
    
    // Dependency injection
    public UserService(Database database) {
        this.database = database;
    }
    
    public void addUser(String userData) {
        database.insert(userData);
    }
}
```