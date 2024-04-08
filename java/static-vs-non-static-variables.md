# Static vs Non Static variables

## What is the difference between static and non-static variables?

In programming, variables can be classified as static or non-static based on their lifetimes and accessibility:

*   #### Non-Static Variables

    Non-static variables, or instance variables, are unique to each instance of a class. When a new object is created, these variables get their own memory space. They are separate for each object, meaning changes in one object's non-static variables don't affect others. These variables are accessible only through objects and disappear when the object is deleted or the program ends.


*   #### Static Variables

    Static variables are the common shared variables within a class, stored in a common memory area. They are initialized just once, at the start of the program, and keep their value until the program finishes. Unlike non-static variables, static variables can be accessed directly using the class name, without creating an object. Any change in a static variable by one instance reflects across all instances of the class.

The choice between using static and non-static variables depends on the specific needs of your application, including whether the variable's value should be shared across all instances or not.

## Use cases of static and non-static variables

#### Use Cases of Static and Non-Static Variables

**Static Variables**

1. **Shared Configuration**: When you need a common configuration value to be accessible across all instances of a class, static variables serve this purpose well. For example, setting a logging level that affects all instances.
2. **Counting Instances**: To keep track of how many instances of a class have been created or are alive, a static variable can be incremented in the constructor and decremented in a destructor (or finalizer).
3. **Singleton Pattern**: In implementing the Singleton design pattern, a static variable holds the single instance of the class that is accessible globally.

**Non-Static Variables**

1. **Object-specific Properties**: When each object needs to maintain its own state, non-static (instance) variables are used. For instance, the balance of individual bank account objects.
2. **Dynamic Configuration**: If objects of the same class need different configurations or behaviors, using non-static variables allows for this flexibility.
3. **Encapsulation**: To encapsulate data specific to each object that must not be shared or accessible directly from outside the object, thereby adhering to the principles of object-oriented programming.

Both static and non-static variables have their place in programming, and their use should be carefully considered to align with the design and functionality of your application.



## How to update data in static variables? Give example with code

Updating data in static variables can be achieved directly by accessing the variable through the class name and assigning a new value to it. This is due to the fact that static variables are class-level variables and not tied to any specific instance of the class.

Here's an example in Java demonstrating how to update a static variable:

```java
public class BankAccount {
    // Static variable to hold the interest rate for all accounts
    public static double interestRate = 0.5;

    // Instance variable for the account balance
    private double balance;

    // Constructor to initialize the balance
    public BankAccount(double balance) {
        this.balance = balance;
    }

    // Method to update the static interest rate for all accounts
    public static void updateInterestRate(double newRate) {
        interestRate = newRate;
    }

    // Method to calculate the updated balance after applying the interest rate
    public void applyInterest() {
        balance += (balance * interestRate) / 100;
    }
}
```

To update the interest rate for all `BankAccount` objects, you would call the `updateInterestRate` method with the new rate as follows:

```java
BankAccount.updateInterestRate(0.75);
```

This updates the `interestRate` static variable to `0.75` for all instances of the `BankAccount` class.
