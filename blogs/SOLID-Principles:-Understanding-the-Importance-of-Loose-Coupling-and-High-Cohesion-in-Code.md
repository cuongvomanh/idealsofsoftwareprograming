# SOLID Principles: Understanding the Importance of Loose Coupling and High Cohesion in Code

As a software developer, you always strive to write clean and maintainable code. The SOLID principles help you design software that is easy to understand, modify, and extend. SOLID is an acronym for five design principles that form the foundation of object-oriented programming. In this post, we'll take a closer look at these principles and how they relate to the concepts of loose coupling and high cohesion, with some code examples to help illustrate the points.

S - Single Responsibility Principle 

The Single Responsibility Principle (SRP) states that a class should have only one reason to change. In other words, a class should have a single responsibility or purpose. This principle encourages you to create smaller, more focused classes that are easier to understand and modify. By adhering to SRP, you can achieve high cohesion, where the methods and properties in a class have a strong and related purpose.

Here's an example of a class that violates the SRP:

```
class Customer {
  void save() {
    // save customer data to database
  }

  void sendEmail() {
    // send welcome email to customer
  }
}
```

This class has two responsibilities, saving customer data to the database and sending welcome emails, which violates the SRP. To fix this, we could create a separate class (or two) for handling each responsibility, like so:

```
class CustomerRepository {
  void save() {
    // save customer data to database
  }
}

class EmailService {
  void sendWelcomeEmail() {
    // send welcome email to customer
  }
}

class Customer {
  // constructor dependency injection
  Customer(CustomerRepository customerRepository, EmailService emailService) {
    this.customerRepository = customerRepository;
    this.emailService = emailService;
  }

  void save() {
    this.customerRepository.save();
  }

  void sendWelcomeEmail() {
    this.emailService.sendWelcomeEmail();
  }
}
```

Now our `Customer` class only has the responsibility of handling customer data, and we've split the email sending logic into a separate class (`EmailService`) that can be reused in other parts of the system. The `Customer` class also has high cohesion, where its methods and properties are strongly related to the task of handling customer data.

L - Liskov Substitution Principle 

The Liskov Substitution Principle (LSP) states that subtypes should be substitutable for their base types without altering the correctness of the program. This principle ensures that you can use derived classes in place of parent classes without causing any issues. By following LSP, you can achieve loose coupling, where the code is not dependent on specific implementations, but rather on contracts and interfaces.

Here's an example of a class hierarchy that violates the LSP:

```
class Animal {}
class Bird extends Animal {}
class Duck extends Bird {
  void fly() {
    // ducks can't fly
  }
}
```

In this example, the `Duck` class is a subtype of `Bird`, which is a subtype of `Animal`. However, the `Duck` class has a method `fly()` that contradicts the `Bird` class's method `fly()`, because ducks can't fly. This violates the LSP because you can't substitute a `Duck` object for a `Bird` object without causing a bug.

To fix this, we could move the `fly()` method to a separate interface `IFlyable`, which only `Bird` objects implement:

```
interface IFlyable {
  void fly();
}

class Animal {}
class Bird extends Animal implements IFlyable {
  void fly() {
    // implementation for birds
  }
}
class Duck extends Bird {
  // ducks can't fly, no need to implement IFlyable
}
```

Now, we've separated the `fly()` responsibility into a separate interface `IFlyable`, which ensures that any `Bird` object has the ability to fly, but we don't force `Duck` objects to override the `fly()` method. By adhering to LSP, we've achieved loose coupling, where the code depends on the `IFlyable` interface, not on specific implementations.

O - Open-Closed Principle 

The Open-Closed Principle (OCP) states that a module should be open for extension but closed for modification. This principle encourages you to write code that can be extended without changing the existing code. By following OCP, you can achieve loose coupling, where the dependencies between modules are reduced, and high cohesion, where each module has a single responsibility.

Here's an example of a class that violates the OCP:

```
class Customer {
  void addBonusPoints(int points) {
    // add bonus points to customer account
    // this method violates OCP because it may need to change when new bonus rules are added
  }
}
```

In this example, the `addBonusPoints()` method is violating the OCP because it may need to change whenever new bonus rules are added, which goes against the idea of a closed module that is easy to maintain and extend.

To fix this, we could use the Strategy pattern, which encapsulates an algorithm in a separate class that can be varied independently:

```
interface IBonusPointsStrategy {
  int calculateBonusPoints();
}

class SimpleBonusPointsStrategy implements IBonusPointsStrategy {
  int calculateBonusPoints() {...} // implementation for simple bonus rules
}

class AdvancedBonusPointsStrategy implements IBonusPointsStrategy {
  int calculateBonusPoints() {...} // implementation for advanced bonus rules
}

class Customer {
  Customer(IBonusPointsStrategy bonusPointsStrategy) { // constructor dependency injection
    this.bonusPointsStrategy = bonusPointsStrategy;
  }

  void addBonusPoints() {
    int bonusPoints = this.bonusPointsStrategy.calculateBonusPoints();
    // add bonus points to customer account using bonusPoints value
  }
}
```

Now, we've improved the design of our `Customer` class by using the Strategy pattern. We've encapsulated the bonus points calculation in separate classes that implement the same interface (`IBonusPointsStrategy`). Our `Customer` class now has high cohesion, where it's only responsible for adding the bonus points, and loose coupling, because it's not dependent on specific bonus rules implementations.

I - Interface Segregation Principle 

The Interface Segregation Principle (ISP) states that clients should not be forced to depend on methods they do not use. This principle encourages you to create smaller and more focused interfaces that are easier to implement. By following ISP, you can achieve loose coupling, where the code is not dependent on unnecessary methods, and high cohesion, where the interface has a single responsibility.

Here's an example of an interface that violates the ISP:

```
interface ICustomer {
  void save();
  void sendEmail();
  void generateReport();
}
```

In this example, the `ICustomer` interface has three methods, but not all clients may need to use all three methods, leading to unnecessary dependencies. This violates the ISP because clients are forced to depend on methods they don't use.

To fix this, we could split the `ICustomer` interface into smaller, more focused interfaces:

```
interface ICustomerSaver {
  void save();
}

interface ICustomerEmailer {
  void sendEmail();
}

interface ICustomerReporter {
  void generateReport();
}
```

Now, we have three separate interfaces, each with a single responsibility. Clients can now depend only on the interfaces they need to use, which achieves loose coupling. Additionally, each interface has high cohesion because its methods are strongly related to a specific task.

D - Dependency Inversion Principle 

The Dependency Inversion Principle (DIP) states that high-level modules should not depend on low-level modules but both should depend on abstractions. This principle encourages you to write code that is not dependent on specific implementations but rather on abstract interfaces. By following DIP, you can achieve loose coupling, where the code is not dependent on specific implementations, and high cohesion, where the code is structured around abstract interfaces.

Here's an example of code that violates the DIP:

```
class CustomerService {
  CustomerRepository customerRepository = new CustomerRepository();

  List<Customer> getAllCustomers() {
    return this.customerRepository.getAllCustomers();
  }

  void saveCustomer(Customer customer) {
    this.customerRepository.saveCustomer(customer);
  }
}

class CustomerRepository {
  List<Customer> getAllCustomers() {...}
  void saveCustomer(Customer customer) {...}
}
```

In this example, the `CustomerService` class is directly instantiating the `CustomerRepository` class, creating a tight coupling between the two classes. This violates the DIP because we're depending on a specific implementation, not on an abstract interface.

To fix this, we could use constructor dependency injection to inject the `CustomerRepository` object into the `CustomerService` class:

```
interface ICustomerRepository {
  List<Customer> getAllCustomers();
  void saveCustomer(Customer customer);
}

class CustomerService {
  ICustomerRepository customerRepository;

  CustomerService(ICustomerRepository customerRepository) { // constructor dependency injection
    this.customerRepository = customerRepository;
  }

  List<Customer> getAllCustomers() {
    return this.customerRepository.getAllCustomers();
  }

  void saveCustomer(Customer customer) {
    this.customerRepository.saveCustomer(customer);
  }
}

class CustomerRepository implements ICustomerRepository {
  List<Customer> getAllCustomers() {...}
  void saveCustomer(Customer customer) {...}
}
```

Now, we have replaced the tight coupling between `CustomerService` and `CustomerRepository` with an abstract interface `ICustomerRepository`. By following DIP, we've achieved loose coupling, where the code depends on the `ICustomerRepository` abstraction, not on the `CustomerRepository` implementation. In addition, both `CustomerService` and `CustomerRepository` interfaces have high cohesion because they only have responsibility for handling customer data.
