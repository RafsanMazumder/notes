# Solid Principles in C++

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

```cpp
class Employee {
private:
    std::string name;
    std::string id;

public:
    // Employee data methods
    std::string getName() const {
        return name;
    }
    
    void setName(const std::string& name) {
        this->name = name;
    }

    // Database Operations
    void saveToDatabase() {
        // Database code here
    }

    // Report generation
    void generateReport() {
        // Report generation code here
    }
};
```

**Good Example**:

```cpp
// Employee class only manages employee data
class Employee {
private:
    std::string name;
    std::string id;

public:
    // Employee data methods
    std::string getName() const {
        return name;
    }
    
    void setName(const std::string& name) {
        this->name = name;
    }
};

// Separate class for database operations
class EmployeeRepository {
public:
    void save(const Employee& employee) {
        // Database code here
    }
};

// Separate class for reporting
class EmployeeReportGenerator {
public:
    void generateReport(const Employee& employee) {
        // Report generation code
    }
};
```

## Open/Closed Principle (OCP)

**Principle**: Software entities (classes, modules, functions, etc.) should be open for extension but closed for
modification.

**Bad Example**:

```cpp
class PaymentProcessor {
public:
    void processPayment(const std::string& paymentType, double amount) {
        if (paymentType == "credit_card") {
            // Process credit card payment
        } else if (paymentType == "paypal") {
            // Process PayPal payment
        } else if (paymentType == "bitcoin") {
            // Process Bitcoin payment
        }
        // Need to modify this class when adding new payment types
    }
};
```

**Good Example**:

```cpp
// Interface (abstract class in C++)
class PaymentMethod {
public:
    virtual ~PaymentMethod() = default;
    virtual void processPayment(double amount) = 0;
};

// Implementations
class CreditCardPayment : public PaymentMethod {
public:
    void processPayment(double amount) override {
        // Process credit card payment
    }
};

class PayPalPayment : public PaymentMethod {
public:
    void processPayment(double amount) override {
        // Process PayPal payment
    }
};

class BitcoinPayment : public PaymentMethod {
public:
    void processPayment(double amount) override {
        // Process Bitcoin payment
    }
};

// Client code - closed for modification
class PaymentProcessor {
public:
    void processPayment(PaymentMethod& paymentMethod, double amount) {
        paymentMethod.processPayment(amount);
    }
};
```

## Liskov Substitution Principle (LSP)

**Principle**: Subtypes must be substitutable for their base types without altering the correctness of the program.

**Bad Example**:

```cpp
// BAD: Ostrich cannot fly, violates the "Bird can fly" expectation
class Bird {
public:
    virtual ~Bird() = default;
    
    virtual void fly() {
        std::cout << "Bird is flying" << std::endl;
    }
};

class Sparrow : public Bird {
    // Sparrow can fly, consistent with Bird
};

class Ostrich : public Bird {
public:
    void fly() override {
        // Ostriches cannot fly. This breaks the expectation.
        throw std::runtime_error("Ostrich cannot fly!");
        // Or: std::cout << "Ostrich cannot fly" << std::endl; // Still problematic
    }
};

// Client code
// void makeBirdFly(Bird& bird) {
//     bird.fly(); // This will crash if 'bird' is an Ostrich
// }
```

**Good Example**:

```cpp
// GOOD: Separate flying behavior
class Flyable {
public:
    virtual ~Flyable() = default;
    virtual void fly() = 0;
};

// Abstract base class for birds
class Bird {
public:
    virtual ~Bird() = default;
    virtual void makeSound() = 0;
    
    void eat() { 
        std::cout << "Bird is eating." << std::endl; 
    }
};

class Sparrow : public Bird, public Flyable { // Sparrow IS-A Bird and IS Flyable
public:
    void makeSound() override { 
        std::cout << "Chirp" << std::endl; 
    }
    
    void fly() override { 
        std::cout << "Sparrow is flying high." << std::endl; 
    }
};

class Ostrich : public Bird { // Ostrich IS-A Bird, but NOT Flyable
public:
    void makeSound() override { 
        std::cout << "Boom" << std::endl; 
    }
    
    // No fly() method, or a run() method specific to Ostrich
    void run() { 
        std::cout << "Ostrich is running fast." << std::endl; 
    }
};

class Penguin : public Bird { // Penguin is a bird but doesn't fly
public:
    void makeSound() override { 
        std::cout << "Squawk" << std::endl; 
    }
    
    void swim() { 
        std::cout << "Penguin is swimming." << std::endl; 
    }
};
```

## Interface Segregation Principle (ISP)

**Principle**: Clients should not be forced to depend on interfaces they do not use.

**Bad Example**:

```cpp
// Fat interface
class Worker {
public:
    virtual ~Worker() = default;
    virtual void work() = 0;
    virtual void eat() = 0;
    virtual void sleep() = 0;
};

// Problem: Robot can't eat or sleep
class Robot : public Worker {
public:
    void work() override {
        // Working
    }
    
    void eat() override {
        // Robot can't eat, but forced to implement
        throw std::runtime_error("Operation not supported");
    }
    
    void sleep() override {
        // Robot can't sleep, but forced to implement
        throw std::runtime_error("Operation not supported");
    }
};
```

**Good Example**:

```cpp
// Segregated interfaces
class Workable {
public:
    virtual ~Workable() = default;
    virtual void work() = 0;
};

class Eatable {
public:
    virtual ~Eatable() = default;
    virtual void eat() = 0;
};

class Sleepable {
public:
    virtual ~Sleepable() = default;
    virtual void sleep() = 0;
};

// Human implements all interfaces
class Human : public Workable, public Eatable, public Sleepable {
public:
    void work() override {
        // Human working
    }
    
    void eat() override {
        // Human eating
    }
    
    void sleep() override {
        // Human sleeping
    }
};

// Robot only implements what it needs
class Robot : public Workable {
public:
    void work() override {
        // Robot working
    }
};
```

## Dependency Inversion Principle (DIP)

**Principle**: High-level modules should not depend on low-level modules. Both should depend on abstractions.

**Bad Example**:

```cpp
// Low-level module
class MySQLDatabase {
public:
    void insert(const std::string& data) {
        // Insert data to MySQL
    }
};

// High-level module depends on low-level module
class UserService {
private:
    MySQLDatabase database;
    
public:
    UserService() {
        // Hard dependency
    }
    
    void addUser(const std::string& userData) {
        database.insert(userData);
    }
};
```

**Good Example**:

```cpp
// Abstraction
class Database {
public:
    virtual ~Database() = default;
    virtual void insert(const std::string& data) = 0;
};

// Low-level module implements abstraction
class MySQLDatabase : public Database {
public:
    void insert(const std::string& data) override {
        // Insert data to MySQL
    }
};

// Alternative implementation
class MongoDatabase : public Database {
public:
    void insert(const std::string& data) override {
        // Insert data to MongoDB
    }
};

// High-level module depends on abstraction
class UserService {
private:
    Database& database;
    
public:
    // Dependency injection
    UserService(Database& database) : database(database) {
    }
    
    void addUser(const std::string& userData) {
        database.insert(userData);
    }
};
```