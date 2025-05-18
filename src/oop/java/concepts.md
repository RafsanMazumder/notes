# Concepts

**Four Pillars of OOP**

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

**Encapsulation**

Encapsulation is the bundling of data and methods that operate on that data within a single unit (class).
```java
public class BankAccount {
    // Private fields - encapsulated
    private String accountNumber;
    private double balance;
    
    // Public methods to access and modify the fields in a controlled way
    public String getAccountNumber() {
        return accountNumber;
    }
    
    public double getBalance() {
        return balance;
    }
    
    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public boolean withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
}
```

**Inheritance**

Inheritance establishes an "is-a" relationship between classes.

```java
// Base class
public class Shape {
    protected String color;
    
    public Shape(String color) {
        this.color = color;
    }
    
    public String getColor() {
        return color;
    }
    
    public double calculateArea() {
        return 0.0; // Default implementation
    }
}

// Derived class
public class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color); // Call to parent constructor
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

**Polymorphism**

Polymorphism allows objects of different types to be treated as objects of a common type.

* Method Overriding (Runtime Polymorphism / Dynamic Dispatch)
* Method Overloading (Compile-time Polymorphism)

```java
public class ShapeProcessor {
    public void printArea(Shape shape) {
        // Works with any Shape subclass due to polymorphism
        System.out.println("Area: " + shape.calculateArea());
    }
    
    public static void main(String[] args) {
        ShapeProcessor processor = new ShapeProcessor();
        
        // Different objects, same method call
        processor.printArea(new Circle("red", 5.0));
        processor.printArea(new Rectangle("blue", 4.0, 6.0));
    }
}
```

**Abstraction**

Abstraction means focusing on essential qualities rather than the specific details.

```java
// Abstract class
public abstract class DatabaseConnection {
    public abstract void connect();
    public abstract void disconnect();
    public abstract void executeQuery(String query);
    
    // Concrete method in abstract class
    public void printConnectionStatus() {
        System.out.println("Checking connection status...");
    }
}

// Concrete implementation
public class MySQLConnection extends DatabaseConnection {
    @Override
    public void connect() {
        System.out.println("Connecting to MySQL database...");
    }
    
    @Override
    public void disconnect() {
        System.out.println("Disconnecting from MySQL database...");
    }
    
    @Override
    public void executeQuery(String query) {
        System.out.println("Executing query in MySQL: " + query);
    }
}
```

**Interfaces vs Abstract Classes**

When to Use Interfaces:

- When unrelated classes need to implement the same behavior

When to Use Abstract Classes:

- When you want to provide a common base implementation
- When you need to define non-public members
- When you want to maintain state across related classes