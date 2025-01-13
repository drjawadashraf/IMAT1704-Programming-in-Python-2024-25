<table>
<tr>
<td></td>
<td>&nbsp;</td>  
<td>
<span style="font-size:32px;">Building and Refactoring an ATM Simulation in Python</span>
<br>
<span> Prepared by: Dr Jawad Ashraf </span>
<br>
<span> Year: 2024-25 </span>
</td>
</tr>
</table>

***

## Introduction

In this lecture, we'll develop an **ATM Simulation** program using Python. We'll start from scratch by outlining the problem and writing pseudocode. Then, we'll incrementally build our program, improving and refactoring it along the way. We'll explain **why** certain refactoring choices are made to enhance code quality, readability, and maintainability. By the end, we'll have a well-structured program that uses classes and modules, embracing Object-Oriented Programming (OOP) principles.

---

## Problem Description

We need to create a simple ATM system with the following functionalities:

- **Check Balance**: Allow users to view their current account balance.
- **Deposit**: Enable users to deposit money into their account.
- **Withdraw**: Allow users to withdraw funds from their account.
- **User Interaction**: Handle user inputs and provide a menu for selecting options.
- **Error Handling**: Manage invalid inputs and insufficient funds gracefully.

**Additional Requirements:**

- Use classes to encapsulate ATM logic (introduce basic OOP concepts).
- Separate user interactions through a controller class.

---

## Step 1: Writing Pseudocode

Before coding, let's outline the steps our program needs to perform.

### 1. Define the ATM Class

- **Attributes**:
  - `balance`: Stores the user's current balance.

- **Methods**:
  - `check_balance()`: Returns the current balance.
  - `deposit(amount)`: Adds the specified amount to the balance.
    - Validate that the amount is positive.
  - `withdraw(amount)`: Subtracts the specified amount from the balance.
    - Validate that the amount is positive and less than or equal to the balance.

### 2. Define the ATMController Class

- **Attributes**:
  - `atm`: An instance of the `ATM` class.

- **Methods**:
  - `display_menu()`: Displays the menu options to the user.
  - `get_number(prompt)`: Retrieves a numerical input from the user.
    - Validates that the input is a number.
  - `check_balance()`: Calls the ATM's `check_balance()` method and displays the balance.
  - `deposit()`: Prompts the user for an amount and calls the ATM's `deposit()` method.
    - Handles exceptions if input is invalid.
  - `withdraw()`: Prompts the user for an amount and calls the ATM's `withdraw()` method.
    - Handles exceptions if input is invalid or funds are insufficient.
  - `run()`: The main loop that runs the ATM simulation.
    - Displays the menu and handles user choices.

### 3. Main Function

- **`main()`**: Creates an instance of `ATMController` and starts the simulation by calling `run()`.

---

## Step 2: Initial Implementation

Let's start coding the basic version of the ATM simulation without classes.

```python
# Initial implementation without classes
balance = 0

while True:
    print('\nWelcome to the ATM!')
    print('1. Check Balance')
    print('2. Deposit')
    print('3. Withdraw')
    print('4. Exit')

    choice = input('Please choose an option: ')

    if choice == '1':
        print(f'Your current balance is: ${balance}')
    elif choice == '2':
        amount = float(input('Enter the amount to deposit: '))
        if amount <= 0:
            print('Deposit amount must be positive.')
        else:
            balance += amount
            print(f'Successfully deposited ${amount}.')
    elif choice == '3':
        amount = float(input('Enter the amount to withdraw: '))
        if amount <= 0:
            print('Withdrawal amount must be positive.')
        elif amount > balance:
            print('Insufficient funds.')
        else:
            balance -= amount
            print(f'Successfully withdrew ${amount}.')
    elif choice == '4':
        print('Thank you for using the ATM.')
        break
    else:
        print('Invalid choice. Please try again.')
```

---

## Step 3: Improving Logic and Error Handling

Our initial code works, but it lacks proper error handling and code organization.

**Issues:**

- **Error Handling**: No validation for non-numeric inputs when entering amounts.
- **Code Organization**: All logic is in the main loop, making it cluttered.
- **Scalability**: Hard to maintain or extend the code.

**Improvements:**

- Add try-except blocks to handle invalid numerical inputs.
- Modularize the code by moving repeated code into functions.

**Updated Code:**

```python
def get_number(prompt):
    while True:
        try:
            number = float(input(prompt))
            return number
        except ValueError:
            print('Please enter a valid number.')

# Use get_number() in deposit and withdraw sections
```

---

## Step 4: Introducing Classes for OOP

To embrace Object-Oriented Programming principles, we'll encapsulate the ATM's logic within classes.

### 4.1 Defining the `ATM` Class

**Why Use a Class?**

- **Encapsulation**: Groups related data and methods.
- **Reusability**: The `ATM` class can be reused or extended in other programs.
- **Maintainability**: Easier to manage and update the code.

**Implementation:**

```python
class ATM:
    def __init__(self):
        self.balance = 0

    def check_balance(self):
        return self.balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError('Deposit amount must be positive.')
        self.balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError('Withdrawal amount must be positive.')
        if amount > self.balance:
            raise ValueError('Insufficient funds.')
        self.balance -= amount
```

### 4.2 Defining the `ATMController` Class

**Purpose:**

- **User Interaction**: Handles all inputs and outputs.
- **Separation of Concerns**: Keeps the `ATM` class focused on business logic.

**Implementation:**

```python
class ATMController:
    def __init__(self):
        self.atm = ATM()

    def get_number(self, prompt):
        while True:
            try:
                number = float(input(prompt))
                return number
            except ValueError:
                print('Please enter a valid number.')

    def display_menu(self):
        print('\nWelcome to the ATM!')
        print('1. Check Balance')
        print('2. Deposit')
        print('3. Withdraw')
        print('4. Exit')

    def check_balance(self):
        balance = self.atm.check_balance()
        print(f'Your current balance is: ${balance}')

    def deposit(self):
        while True:
            try:
                amount = self.get_number('Enter the amount to deposit: ')
                self.atm.deposit(amount)
                print(f'Successfully deposited ${amount}.')
                break
            except ValueError as error:
                print(error)

    def withdraw(self):
        while True:
            try:
                amount = self.get_number('Enter the amount to withdraw: ')
                self.atm.withdraw(amount)
                print(f'Successfully withdrew ${amount}.')
                break
            except ValueError as error:
                print(error)

    def run(self):
        while True:
            self.display_menu()
            choice = input('Please choose an option: ')
            if choice == '1':
                self.check_balance()
            elif choice == '2':
                self.deposit()
            elif choice == '3':
                self.withdraw()
            elif choice == '4':
                print('Thank you for using the ATM.')
                break
            else:
                print('Invalid choice. Please try again.')
```

---

## Step 5: Final Refactored Code

Putting it all together, our refactored code looks like this:

```python
class ATM:
    def __init__(self):
        self.balance = 0

    def check_balance(self):
        return self.balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError('Deposit amount must be positive.')
        self.balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError('Withdrawal amount must be positive.')
        if amount > self.balance:
            raise ValueError('Insufficient funds.')
        self.balance -= amount

class ATMController:
    def __init__(self):
        self.atm = ATM()

    def get_number(self, prompt):
        while True:
            try:
                number = float(input(prompt))
                return number
            except ValueError:
                print('Please enter a valid number.')

    def display_menu(self):
        print('\nWelcome to the ATM!')
        print('1. Check Balance')
        print('2. Deposit')
        print('3. Withdraw')
        print('4. Exit')

    def check_balance(self):
        balance = self.atm.check_balance()
        print(f'Your current balance is: ${balance}')

    def deposit(self):
        while True:
            try:
                amount = self.get_number('Enter the amount to deposit: ')
                self.atm.deposit(amount)
                print(f'Successfully deposited ${amount}.')
                break
            except ValueError as error:
                print(error)

    def withdraw(self):
        while True:
            try:
                amount = self.get_number('Enter the amount to withdraw: ')
                self.atm.withdraw(amount)
                print(f'Successfully withdrew ${amount}.')
                break
            except ValueError as error:
                print(error)

    def run(self):
        while True:
            self.display_menu()
            choice = input('Please choose an option: ')
            if choice == '1':
                self.check_balance()
            elif choice == '2':
                self.deposit()
            elif choice == '3':
                self.withdraw()
            elif choice == '4':
                print('Thank you for using the ATM.')
                break
            else:
                print('Invalid choice. Please try again.')

def main():
    atm_controller = ATMController()
    atm_controller.run()

if __name__ == '__main__':
    main()
```

---

## Step 6: Explaining the Refactoring Choices

Let's discuss how we transformed the initial code into this final version and **why** these refactoring choices were made.

### 6.1 Introducing Classes (`ATM` and `ATMController`)

**Why Use Classes?**

- **Encapsulation**: Groups related data and methods, making the code organized.
- **Reusability**: Classes can be instantiated multiple times or used in different programs.
- **Maintainability**: Easier to update and manage code when it's well-structured.

**`ATM` Class Responsibilities:**

- **Business Logic**: Handles all operations related to the account balance.
- **Data Encapsulation**: Stores the balance as an attribute and manipulates it through methods.

**`ATMController` Class Responsibilities:**

- **User Interface**: Manages all inputs and outputs.
- **Error Handling**: Catches exceptions from the `ATM` class and provides user-friendly messages.
- **Flow Control**: Contains the main loop that keeps the program running.

### 6.2 Error Handling with Exceptions

**Why Use Exceptions?**

- **Separation of Concerns**: The `ATM` class raises exceptions without worrying about how they're handled.
- **Robustness**: Allows the program to handle unexpected situations gracefully.
- **User Feedback**: Provides clear error messages to the user.

**Implementation:**

- In the `ATM` class, methods `deposit()` and `withdraw()` raise `ValueError` if conditions are not met.
- The `ATMController` class catches these exceptions and prints the error messages.

### 6.3 Modularizing Input Handling (`get_number()`)

**Why Create `get_number()` Function?**

- **Code Reusability**: Avoids duplicating code for getting numerical input.
- **Consistency**: Ensures all numerical inputs are handled the same way.
- **User Experience**: Continuously prompts the user until a valid number is entered.

### 6.4 Structuring the Main Loop (`run()` Method)

**Why Use a `run()` Method?**

- **Encapsulation**: Keeps the main loop within the `ATMController` class.
- **Organization**: Separates the flow control from other functionalities.
- **Scalability**: Makes it easier to add new features or modify the flow.

### 6.5 Using `main()` Function and `if __name__ == '__main__':`

**Purpose:**

- **Entry Point**: Defines the starting point of the program.
- **Modularity**: Allows the code to be imported as a module without running the main program.
- **Good Practice**: Standard in Python scripts to allow code reuse.

---

## Conclusion

By starting with a basic implementation and gradually refactoring, we've improved our code's structure and maintainability. Using classes allows us to embrace Object-Oriented Programming principles, making our code more organized and scalable.

**Key Takeaways:**

- **Use Classes**: Encapsulate related data and methods for better organization.
- **Separate Concerns**: Keep business logic and user interaction separate.
- **Implement Error Handling**: Use exceptions to handle unexpected situations.
- **Modularize Code**: Create reusable functions and methods to avoid duplication.
- **Follow Best Practices**: Use `main()` function and `if __name__ == '__main__':` for code reusability.

---

## Optional Enhancements

1. **Implement a PIN System**

   - **Description**: Users must enter their PIN before accessing the ATM functions.
   - **Implementation**:
     - Add a `pin` attribute to the `ATM` class.
     - Create a method `verify_pin()` in the `ATMController` to check the user's input PIN.
     - Prompt the user to enter their PIN at the start of the program.

2. **Add Transaction History**

   - **Description**: Keep track of all transactions (deposits and withdrawals) and allow users to view their transaction history.
   - **Implementation**:
     - Add a `transactions` list attribute to the `ATM` class.
     - Append transaction details to the list whenever a deposit or withdrawal occurs.
     - Add a new menu option to display the transaction history.

3. **Handle Multiple User Accounts**

   - **Description**: Enable the system to handle multiple user accounts, allowing users to log in and manage their individual balances.
   - **Implementation**:
     - Create a `User` class with attributes like `username`, `pin`, and an instance of `ATM`.
     - Maintain a list or dictionary of users.
     - Modify the `ATMController` to handle user login and manage the session.

---

## Using Python Modules

To make our code more modular and reusable, we'll separate the classes into different Python modules.

### 1. Create Module Files

- **atm.py**: Contains the `ATM` class.
- **atm_controller.py**: Contains the `ATMController` class.
- **main.py**: Contains the `main()` function and runs the program.

### 2. Update `atm.py`

```python
# atm.py

class ATM:
    def __init__(self):
        self.balance = 0

    def check_balance(self):
        return self.balance

    def deposit(self, amount):
        if amount <= 0:
            raise ValueError('Deposit amount must be positive.')
        self.balance += amount

    def withdraw(self, amount):
        if amount <= 0:
            raise ValueError('Withdrawal amount must be positive.')
        if amount > self.balance:
            raise ValueError('Insufficient funds.')
        self.balance -= amount
```

### 3. Update `atm_controller.py`

```python
# atm_controller.py

from atm import ATM

class ATMController:
    def __init__(self):
        self.atm = ATM()

    def get_number(self, prompt):
        while True:
            try:
                number = float(input(prompt))
                return number
            except ValueError:
                print('Please enter a valid number.')

    def display_menu(self):
        print('\nWelcome to the ATM!')
        print('1. Check Balance')
        print('2. Deposit')
        print('3. Withdraw')
        print('4. Exit')

    def check_balance(self):
        balance = self.atm.check_balance()
        print(f'Your current balance is: ${balance}')

    def deposit(self):
        while True:
            try:
                amount = self.get_number('Enter the amount to deposit: ')
                self.atm.deposit(amount)
                print(f'Successfully deposited ${amount}.')
                break
            except ValueError as error:
                print(error)

    def withdraw(self):
        while True:
            try:
                amount = self.get_number('Enter the amount to withdraw: ')
                self.atm.withdraw(amount)
                print(f'Successfully withdrew ${amount}.')
                break
            except ValueError as error:
                print(error)

    def run(self):
        while True:
            self.display_menu()
            choice = input('Please choose an option: ')
            if choice == '1':
                self.check_balance()
            elif choice == '2':
                self.deposit()
            elif choice == '3':
                self.withdraw()
            elif choice == '4':
                print('Thank you for using the ATM.')
                break
            else:
                print('Invalid choice. Please try again.')
```

### 4. Update `main.py`

```python
# main.py

from atm_controller import ATMController

def main():
    atm_controller = ATMController()
    atm_controller.run()

if __name__ == '__main__':
    main()
```

### 5. Run the Program

- Execute `main.py` to run the ATM simulation.
- Ensure that all module files (`atm.py`, `atm_controller.py`, and `main.py`) are in the same directory or properly referenced.

**Benefits of Using Modules:**

- **Code Organization**: Separates code logically, making it easier to navigate.
- **Reusability**: Classes can be imported and used in other programs.
- **Maintainability**: Changes in one module don't affect others unless necessary.

---

## Final Thoughts

By modularizing our code and using classes, we've created a robust ATM simulation that can be easily extended and maintained. Understanding the reasons behind each refactoring choice empowers you to write clean, efficient, and maintainable code.

---
