# Strategy Pattern

## Category

**Behavioral Design Pattern**

---

# Definition

The **Strategy Pattern** is a behavioral design pattern that allows you to define a family of algorithms, encapsulate each one into a separate class, and make them interchangeable at runtime without modifying the client code.

> **In simple words:** Same goal, different ways to achieve it.

---

# Motivation

During software development, we often encounter situations where the **behavior (algorithm)** changes while the overall workflow remains the same.

Instead of writing long `if-else` or `switch` statements for every possible behavior, we can separate each behavior into its own class and choose the appropriate one when needed.

---

# Real-Life Analogy (Google Maps)

Imagine using **Google Maps**.

Your destination is fixed:

- **From:** Bangalore
- **To:** Electronic City

However, you can choose different travel modes:

- 🚗 Car
- 🚌 Bus
- 🚶 Walk
- 🚴 Bicycle

The **goal** is always the same—reach the destination.

Only the **route calculation algorithm** changes depending on the selected mode.

Google Maps doesn't rewrite its navigation logic every time; it simply switches the routing strategy.

This is exactly what the **Strategy Pattern** does.

---

# Problem Without Strategy Pattern

Suppose an application supports multiple payment methods.

```python
class PaymentProcessor:

    def pay(self, amount, payment_type):

        if payment_type == "credit_card":
            ...

        elif payment_type == "upi":
            ...

        elif payment_type == "wallet":
            ...

        elif payment_type == "netbanking":
            ...
```

### Problems

- Large `if-else` chains
- Difficult to maintain
- Violates **Open/Closed Principle**
- Violates **Single Responsibility Principle**
- Difficult to test
- High coupling

---

# Solution

Instead of putting every payment algorithm inside one class:

- Create a separate class for each payment method.
- Define a common interface.
- Let the client choose which payment strategy to use.
- The payment processor simply delegates the work.

---

# Structure

```
                 Client
                    |
                    |
                    ▼
        +----------------------+
        | PaymentProcessor     |   (Context)
        +----------------------+
                    |
                    ▼
        +----------------------+
        | PaymentStrategy      |   (Strategy Interface)
        +----------------------+
              ▲          ▲
              |          |
     CreditCard     UPI      Wallet
```

---

# Components

## 1. Strategy

Defines the common interface that every strategy must implement.

```python
class PaymentStrategy(ABC):

    @abstractmethod
    def pay(self, amount):
        pass
```

---

## 2. Concrete Strategies

Different implementations of the same behavior.

```python
class CreditCardPayment(PaymentStrategy):
    ...

class UPIPayment(PaymentStrategy):
    ...

class WalletPayment(PaymentStrategy):
    ...
```

---

## 3. Context

Uses a strategy object without knowing its implementation details.

```python
class PaymentProcessor:

    def __init__(self, strategy):
        self.strategy = strategy

    def process_payment(self, amount):
        self.strategy.pay(amount)
```

---

## 4. Client

Chooses the desired strategy.

```python
payment_processor = PaymentProcessor(CreditCardPayment())
payment_processor.process_payment(10)
```

Output

```
Paid ₹10 using Credit Card
```

Changing the payment method requires only changing the strategy object.

```python
payment_processor = PaymentProcessor(UPIPayment())
```

No changes are required inside `PaymentProcessor`.

---

# Advantages

- Eliminates long `if-else` statements
- Follows the **Open/Closed Principle**
- Follows **Single Responsibility Principle**
- Easy to extend with new strategies
- Easy to test each strategy independently
- Promotes composition over inheritance

---

# Disadvantages

- Increases the number of classes
- Client must know which strategy to choose
- Slightly more complex for very small applications

---

# When to Use

Use the Strategy Pattern when:

- Multiple algorithms solve the same problem.
- Algorithms need to be selected at runtime.
- You want to avoid large conditional statements.
- Different implementations should be easily replaceable.

---

# When Not to Use

Avoid using it when:

- There is only one algorithm.
- The behavior is unlikely to change.
- Introducing multiple classes adds unnecessary complexity.

---

# SOLID Principles Used

- ✅ Single Responsibility Principle (SRP)
- ✅ Open/Closed Principle (OCP)
- ✅ Dependency Inversion Principle (DIP)

---

# Real-World Examples

- Google Maps (Car, Bike, Walk routes)
- Payment Gateways (UPI, Card, Wallet)
- Sorting Algorithms
- Compression Algorithms (ZIP, RAR, 7Z)
- Image Filters

---

# Key Takeaways

- **Same goal, different algorithms.**
- Encapsulate each algorithm into its own class.
- The context delegates work to the selected strategy.
- New behavior is added by creating a new strategy instead of modifying existing code.
- Prefer **Composition over Inheritance**.

---

# Complete Example

```python
from abc import ABC, abstractmethod


class PaymentStrategy(ABC):

    @abstractmethod
    def pay(self, amount):
        pass


class PaymentProcessor:

    def __init__(self, strategy: PaymentStrategy):
        self.strategy = strategy

    def process_payment(self, amount):
        self.strategy.pay(amount)


class CreditCardPayment(PaymentStrategy):

    def pay(self, amount):
        print(f"Paid ₹{amount} using Credit Card")


class UPIPayment(PaymentStrategy):

    def pay(self, amount):
        print(f"Paid ₹{amount} using UPI")


class WalletPayment(PaymentStrategy):

    def pay(self, amount):
        print(f"Paid ₹{amount} using Wallet")


payment_processor = PaymentProcessor(CreditCardPayment())
payment_processor.process_payment(10)
```

---

# Interview Questions

1. What problem does the Strategy Pattern solve?
2. How is Strategy different from State Pattern?
3. Why is Strategy considered an example of composition over inheritance?
4. Which SOLID principles does Strategy Pattern follow?
5. When would you choose Strategy over a large `if-else` block?
