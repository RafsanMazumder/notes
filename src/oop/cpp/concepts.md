# Concepts

**Four Pillars of OOP**

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

**Encapsulation**

Encapsulation is the bundling of data and methods that operate on that data within a single unit (class).
```cpp
class BankAccount {
private:
    // Private fields - encapsulated
    std::string accountNumber;
    double balance;
    
public:
    // Public methods to access and modify the fields in a controlled way
    std::string getAccountNumber() const {
        return accountNumber;
    }
    
    double getBalance() const {
        return balance;
    }
    
    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }
    
    bool withdraw(double amount) {
        if (amount > 0 && balance >= amount) {
            balance -= amount;
            return true;
        }
        return false;
    }
};
```

**Inheritance**

Inheritance establishes an "is-a" relationship between classes.

```cpp
// Base class
class Shape {
protected:
    std::string color;
    
public:
    Shape(const std::string& color) : color(color) {
    }
    
    // Virtual destructor for proper cleanup in derived classes
    virtual ~Shape() = default;
    
    std::string getColor() const {
        return color;
    }
    
    virtual double calculateArea() const {
        return 0.0; // Default implementation
    }
};

// Derived class
class Circle : public Shape {
private:
    double radius;
    
public:
    Circle(const std::string& color, double radius) 
        : Shape(color), radius(radius) {
    }
    
    double calculateArea() const override {
        return M_PI * radius * radius;
    }
};
```

**Polymorphism**

Polymorphism allows objects of different types to be treated as objects of a common type.

* Method Overriding (Runtime Polymorphism / Dynamic Dispatch)
* Method Overloading (Compile-time Polymorphism)

```cpp
class Rectangle : public Shape {
private:
    double width;
    double height;
    
public:
    Rectangle(const std::string& color, double width, double height)
        : Shape(color), width(width), height(height) {
    }
    
    double calculateArea() const override {
        return width * height;
    }
};

class ShapeProcessor {
public:
    void printArea(const Shape& shape) {
        // Works with any Shape subclass due to polymorphism
        std::cout << "Area: " << shape.calculateArea() << std::endl;
    }
};

int main() {
    ShapeProcessor processor;
    
    // Different objects, same method call
    Circle circle("red", 5.0);
    Rectangle rectangle("blue", 4.0, 6.0);
    
    processor.printArea(circle);
    processor.printArea(rectangle);
    
    return 0;
}
```

**Abstraction**

Abstraction means focusing on essential qualities rather than the specific details.

```cpp
// Abstract class
class DatabaseConnection {
public:
    // Virtual destructor for proper cleanup in derived classes
    virtual ~DatabaseConnection() = default;
    
    // Pure virtual functions (abstract methods)
    virtual void connect() = 0;
    virtual void disconnect() = 0;
    virtual void executeQuery(const std::string& query) = 0;
    
    // Concrete method in abstract class
    void printConnectionStatus() {
        std::cout << "Checking connection status..." << std::endl;
    }
};

// Concrete implementation
class MySQLConnection : public DatabaseConnection {
public:
    void connect() override {
        std::cout << "Connecting to MySQL database..." << std::endl;
    }
    
    void disconnect() override {
        std::cout << "Disconnecting from MySQL database..." << std::endl;
    }
    
    void executeQuery(const std::string& query) override {
        std::cout << "Executing query in MySQL: " << query << std::endl;
    }
};
```

**Interfaces vs Abstract Classes**

When to Use Interfaces:

- When unrelated classes need to implement the same behavior

When to Use Abstract Classes:

- When you want to provide a common base implementation
- When you need to define non-public members
- When you want to maintain state across related classes~~