# Design Patterns in C++

## Creational Patterns

**Singleton Pattern**

Ensures a class has only one instance and provides a global point of access to it.
```cpp
#include <iostream>
#include <string>

class DatabaseConnection {
private:
    // Private constructor prevents instantiation
    DatabaseConnection() {
        // Initialize connection
    }
    
    // Delete copy constructor and assignment operator
    DatabaseConnection(const DatabaseConnection&) = delete;
    DatabaseConnection& operator=(const DatabaseConnection&) = delete;

public:
    // Thread-safe in C++11 and later due to magic statics
    static DatabaseConnection& getInstance() {
        static DatabaseConnection instance;
        return instance;
    }
    
    void query(const std::string& sql) {
        // Execute query
        std::cout << "Executing query: " << sql << std::endl;
    }
};

// Usage
int main() {
    DatabaseConnection& connection = DatabaseConnection::getInstance();
    connection.query("SELECT * FROM users");
    
    return 0;
}
```

**Factory Method Pattern**

Defines an interface for creating an object, but lets subclasses decide which class to instantiate.
```cpp
#include <iostream>
#include <memory>

// Product interface
class Vehicle {
public:
    virtual ~Vehicle() = default;
    virtual void drive() = 0;
};

// Concrete products
class Car : public Vehicle {
public:
    void drive() override {
        std::cout << "Driving a car" << std::endl;
    }
};

class Truck : public Vehicle {
public:
    void drive() override {
        std::cout << "Driving a truck" << std::endl;
    }
};

// Creator
class VehicleFactory {
public:
    virtual ~VehicleFactory() = default;
    virtual Vehicle& createVehicle() = 0;
    
    void deliverVehicle() {
        Vehicle& vehicle = createVehicle();
        std::cout << "Delivering the vehicle..." << std::endl;
        vehicle.drive();
    }
};

// Concrete creators using static storage
class CarFactory : public VehicleFactory {
private:
    Car car;
    
public:
    Vehicle& createVehicle() override {
        return car;
    }
};

class TruckFactory : public VehicleFactory {
private:
    Truck truck;
    
public:
    Vehicle& createVehicle() override {
        return truck;
    }
};

int main() {
    CarFactory carFactory;
    carFactory.deliverVehicle();
    
    TruckFactory truckFactory;
    truckFactory.deliverVehicle();
    
    return 0;
}
```

**Builder Pattern**

Separates the construction of a complex object from its representation.
```cpp
#include <iostream>
#include <string>

class Computer {
private:
    std::string cpu;
    std::string ram;
    std::string storage;
    std::string gpu;
    std::string motherboard;
    
public:
    // Make Builder a friend to access private fields
    friend class ComputerBuilder;
    
    void display() const {
        std::cout << "Computer Specs:\n"
                  << "CPU: " << cpu << "\n"
                  << "RAM: " << ram << "\n"
                  << "Storage: " << storage << "\n"
                  << "GPU: " << gpu << "\n"
                  << "Motherboard: " << motherboard << std::endl;
    }
};

class ComputerBuilder {
private:
    Computer computer;
    
public:
    ComputerBuilder& cpu(const std::string& cpu) {
        computer.cpu = cpu;
        return *this;
    }
    
    ComputerBuilder& ram(const std::string& ram) {
        computer.ram = ram;
        return *this;
    }
    
    ComputerBuilder& storage(const std::string& storage) {
        computer.storage = storage;
        return *this;
    }
    
    ComputerBuilder& gpu(const std::string& gpu) {
        computer.gpu = gpu;
        return *this;
    }
    
    ComputerBuilder& motherboard(const std::string& motherboard) {
        computer.motherboard = motherboard;
        return *this;
    }
    
    Computer build() {
        return computer;
    }
};

// Usage
int main() {
    Computer computer = ComputerBuilder()
        .cpu("Intel i7")
        .ram("16GB")
        .storage("1TB SSD")
        .gpu("NVIDIA RTX 3080")
        .motherboard("ASUS ROG")
        .build();
    
    computer.display();
    return 0;
}
```

## Structural Patterns

**Adapter Pattern**

Allows incompatible interfaces to work together.
```cpp
#include <iostream>
#include <string>

// OldPaymentGateway (Adaptee)
class OldPaymentGateway {
public:
    void makePayment(const std::string& accountNumber, double amount) {
        std::cout << "Old Gateway: Processing payment of $" << amount 
                  << " for account " << accountNumber << std::endl;
    }
};

// NewPaymentProcessor (Target Interface)
class NewPaymentProcessor {
public:
    virtual ~NewPaymentProcessor() = default;
    virtual void processPayment(const std::string& creditCardNumber, 
                               const std::string& expiryDate, 
                               const std::string& cvv, 
                               double amount) = 0;
};

// PaymentAdapter (Adapter)
class PaymentAdapter : public NewPaymentProcessor {
private:
    OldPaymentGateway& oldGateway;

public:
    PaymentAdapter(OldPaymentGateway& gateway) : oldGateway(gateway) {}

    void processPayment(const std::string& creditCardNumber,
                        const std::string& expiryDate,
                        const std::string& cvv,
                        double amount) override {
        // Adapt: Here we might simplify and just use creditCardNumber as account for the old system
        std::cout << "Adapter: Translating new payment request for old gateway..." << std::endl;
        oldGateway.makePayment(creditCardNumber, amount); // Delegates to the old system
    }
};

// Client code
int main() {
    OldPaymentGateway oldGateway;
    PaymentAdapter adaptedProcessor(oldGateway);

    // Client code uses the NewPaymentProcessor interface
    adaptedProcessor.processPayment("1234-5678-9012-3456", "12/25", "123", 100.00);
    
    return 0;
}
```

**Decorator Pattern**

Attaches additional responsibilities to an object dynamically.
```cpp
#include <iostream>
#include <string>

// Component interface
class Coffee {
public:
    virtual ~Coffee() = default;
    virtual double getCost() const = 0;
    virtual std::string getDescription() const = 0;
};

// Concrete component
class SimpleCoffee : public Coffee {
public:
    double getCost() const override {
        return 2.0;
    }
    
    std::string getDescription() const override {
        return "Simple coffee";
    }
};

// Decorator
class CoffeeDecorator : public Coffee {
protected:
    const Coffee& decoratedCoffee;
    
public:
    CoffeeDecorator(const Coffee& coffee) : decoratedCoffee(coffee) {}
    
    double getCost() const override {
        return decoratedCoffee.getCost();
    }
    
    std::string getDescription() const override {
        return decoratedCoffee.getDescription();
    }
};

// Concrete decorators
class MilkDecorator : public CoffeeDecorator {
public:
    MilkDecorator(const Coffee& coffee) : CoffeeDecorator(coffee) {}
    
    double getCost() const override {
        return CoffeeDecorator::getCost() + 0.5;
    }
    
    std::string getDescription() const override {
        return CoffeeDecorator::getDescription() + ", with milk";
    }
};

class SugarDecorator : public CoffeeDecorator {
public:
    SugarDecorator(const Coffee& coffee) : CoffeeDecorator(coffee) {}
    
    double getCost() const override {
        return CoffeeDecorator::getCost() + 0.2;
    }
    
    std::string getDescription() const override {
        return CoffeeDecorator::getDescription() + ", with sugar";
    }
};

// Note: This is a limited version of the decorator pattern without dynamic lifetime.
// Real use case might require allocations which would involve pointers.
int main() {
    // Create base component
    SimpleCoffee simpleCoffee;
    
    // Stack allocated decorators
    MilkDecorator coffeeWithMilk(simpleCoffee);
    SugarDecorator sweetMilkCoffee(coffeeWithMilk);
    
    std::cout << "Description: " << sweetMilkCoffee.getDescription() << std::endl;
    std::cout << "Cost: $" << sweetMilkCoffee.getCost() << std::endl;
    
    return 0;
}
```

## Behavioral Patterns

**Observer Pattern**

Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

```cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

// Forward declaration
class NewsAgency;

// Observer interface
class Observer {
public:
    virtual ~Observer() = default;
    virtual void update(const std::string& message) = 0;
    virtual void registerWith(NewsAgency& agency) = 0;
    virtual void unregisterFrom(NewsAgency& agency) = 0;
};

// Subject
class NewsAgency {
private:
    std::vector<Observer*> observers;
    std::string news;
    
public:
    void addObserver(Observer* observer) {
        observers.push_back(observer);
    }
    
    void removeObserver(Observer* observer) {
        observers.erase(
            std::remove(observers.begin(), observers.end(), observer),
            observers.end()
        );
    }
    
    void setNews(const std::string& news) {
        this->news = news;
        notifyObservers();
    }
    
private:
    void notifyObservers() {
        for (Observer* observer : observers) {
            observer->update(news);
        }
    }
};

// Concrete observer
class NewsSubscriber : public Observer {
private:
    std::string name;
    
public:
    NewsSubscriber(const std::string& name) : name(name) {}
    
    void update(const std::string& message) override {
        std::cout << name << " received news: " << message << std::endl;
    }
    
    void registerWith(NewsAgency& agency) override {
        agency.addObserver(this);
    }
    
    void unregisterFrom(NewsAgency& agency) override {
        agency.removeObserver(this);
    }
};

int main() {
    NewsAgency agency;
    
    // Create subscribers
    NewsSubscriber subscriber1("John");
    NewsSubscriber subscriber2("Jane");
    
    // Register observers
    subscriber1.registerWith(agency);
    subscriber2.registerWith(agency);
    
    // Set news
    agency.setNews("Breaking News: C++ 23 standard released!");
    
    // Unregister an observer
    subscriber1.unregisterFrom(agency);
    
    // Set more news
    agency.setNews("Update: New features in C++ 23 explained.");
    
    return 0;
}
```

**Strategy Pattern**

Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
```cpp
#include <iostream>
#include <string>

// Strategy interface
class PaymentStrategy {
public:
    virtual ~PaymentStrategy() = default;
    virtual void pay(int amount) = 0;
};

// Concrete strategies
class CreditCardStrategy : public PaymentStrategy {
private:
    std::string name;
    std::string cardNumber;
    std::string cvv;
    std::string dateOfExpiry;
    
public:
    CreditCardStrategy(const std::string& name, 
                      const std::string& cardNumber, 
                      const std::string& cvv, 
                      const std::string& dateOfExpiry)
        : name(name), cardNumber(cardNumber), cvv(cvv), dateOfExpiry(dateOfExpiry) {}
    
    void pay(int amount) override {
        std::cout << amount << " paid with credit card" << std::endl;
    }
};

class PayPalStrategy : public PaymentStrategy {
private:
    std::string email;
    std::string password;
    
public:
    PayPalStrategy(const std::string& email, const std::string& password)
        : email(email), password(password) {}
    
    void pay(int amount) override {
        std::cout << amount << " paid using PayPal" << std::endl;
    }
};

// Context
class ShoppingCart {
private:
    PaymentStrategy* paymentStrategy = nullptr;
    
public:
    // Using reference to strategy, not taking ownership
    void setPaymentStrategy(PaymentStrategy& strategy) {
        paymentStrategy = &strategy;
    }
    
    void checkout(int amount) {
        if (paymentStrategy) {
            paymentStrategy->pay(amount);
        } else {
            std::cout << "No payment strategy set!" << std::endl;
        }
    }
};

int main() {
    ShoppingCart cart;
    
    // Create strategies on the stack
    CreditCardStrategy creditCard("John Doe", "1234567890123456", "123", "12/25");
    PayPalStrategy payPal("john@example.com", "password123");
    
    // Use credit card
    cart.setPaymentStrategy(creditCard);
    cart.checkout(100);
    
    // Switch to PayPal
    cart.setPaymentStrategy(payPal);
    cart.checkout(200);
    
    return 0;
}
```
