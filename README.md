%%{init: {
    "theme": "base",
    "themeVariables": {
        "primaryColor": "#4C8BF5",
        "primaryTextColor": "#FFFFFF",
        "primaryBorderColor": "#1B4DC3",

        "secondaryColor": "#F59E0B",
        "secondaryTextColor": "#FFFFFF",
        "secondaryBorderColor": "#B45309",

        "tertiaryColor": "#10B981",
        "tertiaryTextColor": "#FFFFFF",
        "tertiaryBorderColor": "#047857",

        "lineColor": "#333333",
        "textColor": "#111111"
    }
}}%%
classDiagram

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

    class Payment {
        -baseAmount: double
        -totalAmount: double
        -methodName: String
        +getBaseAmount() double
        +getTotalAmount() double
        +getMethodName() String
    }

    class PaymentProcessor {
        -observers: List~Observer~
        -strategy: PaymentStrategy
        +setStrategy(s: PaymentStrategy) void
        +addObserver(o: Observer) void
        +removeObserver(o: Observer) void
        +notifyObservers(p: Payment) void
        +processPayment(amount: double) Payment
    }

    PaymentProcessor --> PaymentStrategy
    PaymentProcessor o-- Observer
    PaymentProcessor --> Payment
