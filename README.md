classDiagram
    %% Interfaces / Estrategia
    class PaymentStrategy {
      <<interface>>
      +pay(amount: double) double
      +getName() String
    }

    class CardStrategy {
      +pay(amount: double) double
      +getName() String
    }

    class YapeStrategy {
      +pay(amount: double) double
      +getName() String
    }

    class CashStrategy {
      +pay(amount: double) double
      +getName() String
    }

    PaymentStrategy <|.. CardStrategy
    PaymentStrategy <|.. YapeStrategy
    PaymentStrategy <|.. CashStrategy

    %% Payment (modelo)
    class Payment {
      -baseAmount: double
      -totalAmount: double
      -methodName: String
      +getBaseAmount() double
      +getTotalAmount() double
      +getMethodName() String
      +toString() String
    }

    %% Observer
    class Observer {
      <<interface>>
      +update(payment: Payment) void
      +getName() String
    }

    class UserAppObserver {
      +update(payment: Payment) void
      +getName() String
    }

    class AccountingObserver {
      +update(payment: Payment) void
      +getName() String
    }

    Observer <|.. UserAppObserver
    Observer <|.. AccountingObserver

    %% PaymentProcessor (Subject)
    class PaymentProcessor {
      -observers: List~Observer~
      -strategy: PaymentStrategy
      +PaymentProcessor(strategy: PaymentStrategy)
      +setStrategy(s: PaymentStrategy) void
      +addObserver(o: Observer) void
      +removeObserver(o: Observer) void
      +notifyObservers(p: Payment) void
      +processPayment(amount: double) void
    }

    %% Relaciones
    PaymentProcessor --> PaymentStrategy : uses
    PaymentProcessor "1" o-- "*" Observer : manages
    PaymentProcessor --> Payment : creates
