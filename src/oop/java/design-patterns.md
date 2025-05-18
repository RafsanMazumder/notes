# Design Patterns in Java

## Creational Patterns

**Singleton Pattern**

Ensures a class has only one instance and provides a global point of access to it.
```java
public class DatabaseConnection {
    private static DatabaseConnection instance;
    
    // Private constructor prevents instantiation
    private DatabaseConnection() {
        // Initialize connection
    }
    
    // Thread-safe implementation
    public static synchronized DatabaseConnection getInstance() {
        if (instance == null) {
            instance = new DatabaseConnection();
        }
        return instance;
    }
    
    public void query(String sql) {
        // Execute query
    }
}

// Usage
DatabaseConnection connection = DatabaseConnection.getInstance();
connection.query("SELECT * FROM users");
```

**Factory Method Pattern**

Defines an interface for creating an object, but lets subclasses decide which class to instantiate.
```java
// Product interface
public interface Vehicle {
    void drive();
}

// Concrete products
public class Car implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a car");
    }
}

public class Truck implements Vehicle {
    @Override
    public void drive() {
        System.out.println("Driving a truck");
    }
}

// Creator
public abstract class VehicleFactory {
    public abstract Vehicle createVehicle();
    
    public void deliverVehicle() {
        Vehicle vehicle = createVehicle();
        System.out.println("Delivering the vehicle...");
        vehicle.drive();
    }
}

// Concrete creators
public class CarFactory extends VehicleFactory {
    @Override
    public Vehicle createVehicle() {
        return new Car();
    }
}

public class TruckFactory extends VehicleFactory {
    @Override
    public Vehicle createVehicle() {
        return new Truck();
    }
}
```

**Builder Pattern**

Separates the construction of a complex object from its representation.
```java
public class Computer {
    private String cpu;
    private String ram;
    private String storage;
    private String gpu;
    private String motherboard;
    
    private Computer() {}
    
    public static class Builder {
        private Computer computer = new Computer();
        
        public Builder cpu(String cpu) {
            computer.cpu = cpu;
            return this;
        }
        
        public Builder ram(String ram) {
            computer.ram = ram;
            return this;
        }
        
        public Builder storage(String storage) {
            computer.storage = storage;
            return this;
        }
        
        public Builder gpu(String gpu) {
            computer.gpu = gpu;
            return this;
        }
        
        public Builder motherboard(String motherboard) {
            computer.motherboard = motherboard;
            return this;
        }
        
        public Computer build() {
            return computer;
        }
    }
}

// Usage
Computer computer = new Computer.Builder()
    .cpu("Intel i7")
    .ram("16GB")
    .storage("1TB SSD")
    .gpu("NVIDIA RTX 3080")
    .motherboard("ASUS ROG")
    .build();
```

## Structural Patterns

**Adapter Pattern**

Allows incompatible interfaces to work together.
```java
// OldPaymentGateway.java (Adaptee)
class OldPaymentGateway {
    public void makePayment(String accountNumber, double amount) {
        System.out.println("Old Gateway: Processing payment of $" + amount + " for account " + accountNumber);
    }
}

// NewPaymentProcessor.java (Target Interface)
interface NewPaymentProcessor {
    void processPayment(String creditCardNumber, String expiryDate, String cvv, double amount);
}

// PaymentAdapter.java (Adapter)
class PaymentAdapter implements NewPaymentProcessor {
    private OldPaymentGateway oldGateway;

    public PaymentAdapter(OldPaymentGateway oldGateway) {
        this.oldGateway = oldGateway;
    }

    @Override
    public void processPayment(String creditCardNumber, String expiryDate, String cvv, double amount) {
        // Adapt: Here we might simplify and just use creditCardNumber as account for the old system
        // In a real scenario, you'd map fields appropriately or throw an error if not possible.
        System.out.println("Adapter: Translating new payment request for old gateway...");
        oldGateway.makePayment(creditCardNumber, amount); // Delegates to the old system
    }
}

// Main.java (Client)
public class Main {
    public static void main(String[] args) {
        OldPaymentGateway oldGateway = new OldPaymentGateway();
        NewPaymentProcessor adaptedProcessor = new PaymentAdapter(oldGateway);

        // Client code uses the NewPaymentProcessor interface
        adaptedProcessor.processPayment("1234-5678-9012-3456", "12/25", "123", 100.00);
    }
}
```

**Decorator Pattern**

Attaches additional responsibilities to an object dynamically.
```java
// Component interface
public interface Coffee {
    double getCost();
    String getDescription();
}

// Concrete component
public class SimpleCoffee implements Coffee {
    @Override
    public double getCost() {
        return 2.0;
    }
    
    @Override
    public String getDescription() {
        return "Simple coffee";
    }
}

// Decorator
public abstract class CoffeeDecorator implements Coffee {
    protected Coffee decoratedCoffee;
    
    public CoffeeDecorator(Coffee coffee) {
        this.decoratedCoffee = coffee;
    }
    
    @Override
    public double getCost() {
        return decoratedCoffee.getCost();
    }
    
    @Override
    public String getDescription() {
        return decoratedCoffee.getDescription();
    }
}

// Concrete decorators
public class MilkDecorator extends CoffeeDecorator {
    public MilkDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public double getCost() {
        return super.getCost() + 0.5;
    }
    
    @Override
    public String getDescription() {
        return super.getDescription() + ", with milk";
    }
}

public class SugarDecorator extends CoffeeDecorator {
    public SugarDecorator(Coffee coffee) {
        super(coffee);
    }
    
    @Override
    public double getCost() {
        return super.getCost() + 0.2;
    }
    
    @Override
    public String getDescription() {
        return super.getDescription() + ", with sugar";
    }
}

// Order a coffee with milk and sugar
Coffee sweetMilkCoffee = new SugarDecorator(new MilkDecorator(new SimpleCoffee()));
```

## Behavioral Patterns

**Observer Pattern**

Defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified.

```java
// Observer interface
public interface Observer {
    void update(String message);
}

// Subject
public class NewsAgency {
    private List<Observer> observers = new ArrayList<>();
    private String news;
    
    public void addObserver(Observer observer) {
        observers.add(observer);
    }
    
    public void removeObserver(Observer observer) {
        observers.remove(observer);
    }
    
    public void setNews(String news) {
        this.news = news;
        notifyObservers();
    }
    
    private void notifyObservers() {
        for (Observer observer : observers) {
            observer.update(news);
        }
    }
}
```

**Strategy Pattern**

Defines a family of algorithms, encapsulates each one, and makes them interchangeable.
```java
// Strategy interface
public interface PaymentStrategy {
    void pay(int amount);
}

// Concrete strategies
public class CreditCardStrategy implements PaymentStrategy {
    private String name;
    private String cardNumber;
    private String cvv;
    private String dateOfExpiry;
    
    public CreditCardStrategy(String name, String cardNumber, String cvv, String dateOfExpiry) {
        this.name = name;
        this.cardNumber = cardNumber;
        this.cvv = cvv;
        this.dateOfExpiry = dateOfExpiry;
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid with credit card");
    }
}

public class PayPalStrategy implements PaymentStrategy {
    private String email;
    private String password;
    
    public PayPalStrategy(String email, String password) {
        this.email = email;
        this.password = password;
    }
    
    @Override
    public void pay(int amount) {
        System.out.println(amount + " paid using PayPal");
    }
}

// Context
public class ShoppingCart {
    private PaymentStrategy paymentStrategy;
    
    public void setPaymentStrategy(PaymentStrategy paymentStrategy) {
        this.paymentStrategy = paymentStrategy;
    }
    
    public void checkout(int amount) {
        paymentStrategy.pay(amount);
    }
}
```
