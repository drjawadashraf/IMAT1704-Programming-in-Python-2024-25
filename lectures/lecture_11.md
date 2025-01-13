<table>
<tr>
<td></td>
<td>&nbsp;</td>  
<td>
<span style="font-size:32px;">Building and Refactoring a Rock, Paper, Scissors Game in Python</span>
<br>
<span> Prepared by: Dr Jawad Ashraf </span>
<br>
<span> Year: 2024-25 </span>
</td>
</tr>
</table>


***

## Introduction

In this lecture, we'll develop a **Rock, Paper, Scissors** game using Python. We'll start from scratch, outlining the problem and writing pseudocode. Then, we'll incrementally build our program, improving and refactoring it along the way. We'll explain **why** certain refactoring choices are made to enhance the code's quality, readability, and maintainability.

---

## Problem Description

We need to write a program that simulates a game of Rock, Paper, Scissors with the following requirements:

- **User Interaction**: Prompt the player to choose rock (`'r'`), paper (`'p'`), or scissors (`'s'`).
- **Computer Choice**: The computer randomly selects its choice.
- **Display Choices**: Show both choices using emojis for better user experience.
- **Determine Winner**: Decide the winner based on the standard rules of the game.
- **Replay Option**: Allow the player to play multiple rounds until they decide to quit.

---

## Step 1: Writing Pseudocode

Before coding, let's outline the steps our program needs to perform.

1. **Initialize Game Variables**
   - Define constants for rock, paper, and scissors.
   - Map each choice to an emoji for display purposes.
   - Create a list or tuple of valid choices.

2. **Main Game Loop**
   - **Get User Choice**
     - Prompt the user for input.
     - Validate the input.
   - **Get Computer Choice**
     - Randomly select a choice from the valid options.
   - **Display Choices**
     - Show both the user's and computer's choices with emojis.
   - **Determine Winner**
     - Compare choices and decide the outcome.
     - Announce if the user wins, loses, or ties.
   - **Replay Option**
     - Ask the user if they want to play again.
     - Exit the loop if they choose not to continue.

---

## Step 2: Initial Implementation

Let's start coding the basic version of the game without any functions.

```python
import random

# Initialize game variables
choices = ['r', 'p', 's']
emojis = {'r': '🪨', 'p': '📃', 's': '✂️'}

# Main game loop
while True:
    # Get user choice
    user_choice = input("Rock, paper, or scissors? (r/p/s): ").lower()
    if user_choice not in choices:
        print("Invalid choice!")
        continue  # Restart loop if input is invalid

    # Get computer choice
    computer_choice = random.choice(choices)

    # Display choices
    print(f"You chose {emojis[user_choice]}")
    print(f"Computer chose {emojis[computer_choice]}")

    # Determine winner
    if user_choice == computer_choice:
        print("Tie!")
    elif (user_choice == 'r' and computer_choice == 's') or \
         (user_choice == 's' and computer_choice == 'p') or \
         (user_choice == 'p' and computer_choice == 'r'):
        print("You win")
    else:
        print("You lose")

    # Replay option
    should_continue = input("Continue? (y/n): ").lower()
    if should_continue == 'n':
        break
```

---

## Step 3: Improving Input Validation

Our initial code works, but the input validation can be improved. Instead of restarting the entire loop when the user inputs an invalid choice, let's prompt them again until they provide a valid one.

**Why Make This Change?**

- **User Experience**: Continuously prompting the user for valid input without restarting the game loop improves user experience.
- **Code Efficiency**: It avoids unnecessary iterations of the main game loop, making the code more efficient.

**Updated Code:**

```python
# Improved input validation
while True:
    user_choice = input("Rock, paper, or scissors? (r/p/s): ").lower()
    while user_choice not in choices:
        print("Invalid choice!")
        user_choice = input("Please enter 'r', 'p', or 's': ").lower()

    # Rest of the code remains the same
```

---

## Step 4: Modularizing the Code

To enhance readability and maintainability, we'll refactor the code by breaking it into functions. This way, each part of the program has a clear responsibility.

**Why Modularize?**

- **Readability**: Functions make the code easier to read and understand.
- **Reusability**: Functions can be reused in other parts of the program or in other programs.
- **Maintainability**: Easier to debug and modify specific parts without affecting the entire codebase.

### 4.1 Defining Constants and Variables

First, we'll define constants for the choices and their corresponding emojis.

**Why Use Constants?**

- **Clarity**: Constants like `ROCK`, `PAPER`, and `SCISSORS` make the code more understandable.
- **Error Prevention**: Reduces typos and mistakes in string literals scattered throughout the code.
- **Ease of Update**: If a value needs to change, it can be updated in one place.

```python
import random

# Constants
ROCK = 'r'
PAPER = 'p'
SCISSORS = 's'
choices = (ROCK, PAPER, SCISSORS)
emojis = {ROCK: '🪨', PAPER: '📃', SCISSORS: '✂️'}
```

### 4.2 Creating Functions

#### `get_user_choice()`

Handles user input and validation.

**Why Create This Function?**

- **Separation of Concerns**: Isolates input logic from game flow.
- **Reusability**: Can be called whenever user input is needed.
- **Maintainability**: Changes to input logic only need to be made in one place.

```python
def get_user_choice():
    while True:
        user_choice = input('Rock, paper, or scissors? (r/p/s): ').lower()
        if user_choice in choices:
            return user_choice
        else:
            print('Invalid choice!')
```

#### `display_choices(user_choice, computer_choice)`

Displays both choices using emojis.

**Why Create This Function?**

- **Code Organization**: Keeps display logic separate from game logic.
- **Readability**: Makes the main game loop cleaner and more understandable.
- **Consistency**: Ensures the display format is consistent throughout the program.

```python
def display_choices(user_choice, computer_choice):
    print(f'You chose {emojis[user_choice]}')
    print(f'Computer chose {emojis[computer_choice]}')
```

#### `determine_winner(user_choice, computer_choice)`

Determines and announces the winner.

**Why Create This Function?**

- **Modularity**: Encapsulates the game rules in one place.
- **Ease of Updates**: Simplifies updating game logic (e.g., adding new rules or options).
- **Testing**: Makes it easier to test the game logic separately from the rest of the code.

```python
def determine_winner(user_choice, computer_choice):
    if user_choice == computer_choice:
        print('Tie!')
    elif (
        (user_choice == ROCK and computer_choice == SCISSORS) or 
        (user_choice == SCISSORS and computer_choice == PAPER) or 
        (user_choice == PAPER and computer_choice == ROCK)
    ):
        print('You win')
    else:
        print('You lose')
```

#### `play_game()`

Controls the flow of the game.

**Why Create This Function?**

- **Encapsulation**: Wraps the main game loop into a function.
- **Organization**: Keeps the global namespace clean.
- **Scalability**: Allows for future enhancements, such as adding menus or multiple game modes.

```python
def play_game():
    while True:
        user_choice = get_user_choice()
        computer_choice = random.choice(choices)
        display_choices(user_choice, computer_choice)
        determine_winner(user_choice, computer_choice)
        should_continue = input('Continue? (y/n): ').lower()
        if should_continue == 'n':
            break
```

---

## Step 5: Final Refactored Code

Putting it all together, our refactored code looks like this:

```python
import random

ROCK = 'r'
SCISSORS = 's'
PAPER = 'p'
emojis = {ROCK: '🪨', SCISSORS: '✂️', PAPER: '📃'}
choices = (ROCK, SCISSORS, PAPER)

def get_user_choice():
    while True:
        user_choice = input('Rock, paper, or scissors? (r/p/s): ').lower()
        if user_choice in choices:
            return user_choice      
        else:
            print('Invalid choice!')

def display_choices(user_choice, computer_choice):
    print(f'You chose {emojis[user_choice]}')
    print(f'Computer chose {emojis[computer_choice]}')

def determine_winner(user_choice, computer_choice):
    if user_choice == computer_choice:
        print('Tie!')
    elif (
        (user_choice == ROCK and computer_choice == SCISSORS) or 
        (user_choice == SCISSORS and computer_choice == PAPER) or 
        (user_choice == PAPER and computer_choice == ROCK)
    ):
        print('You win')
    else:
        print('You lose')  

def play_game():
    while True:
        user_choice = get_user_choice()
        computer_choice = random.choice(choices)
        display_choices(user_choice, computer_choice)
        determine_winner(user_choice, computer_choice)
        should_continue = input('Continue? (y/n): ').lower()
        if should_continue == 'n':
            break

play_game()
```

---

## Step 6: Explaining the Refactoring Choices

Let's discuss how each part of the original code was transformed into this final version and **why** these refactoring choices were made.

### 6.1 From Hardcoded Strings to Constants

**Before**:

```python
choices = ['r', 'p', 's']
emojis = {'r': '🪨', 'p': '📃', 's': '✂️'}
```

**After**:

```python
ROCK = 'r'
PAPER = 'p'
SCISSORS = 's'
choices = (ROCK, PAPER, SCISSORS)
emojis = {ROCK: '🪨', PAPER: '📃', SCISSORS: '✂️'}
```

**Why This Change?**

- **Readability**: Using constants like `ROCK`, `PAPER`, and `SCISSORS` makes the code more descriptive and easier to understand.
- **Maintainability**: If we need to change the representation of a choice (e.g., changing `'r'` to `'rock'`), we only need to update it in one place.
- **Error Reduction**: Reduces the risk of typos in string literals throughout the code.

### 6.2 Encapsulating Input Handling with `get_user_choice()`

**Before**:

```python
user_choice = input("Rock, paper, or scissors? (r/p/s): ").lower()
if user_choice not in choices:
    print("Invalid choice!")
    continue
```

**After**:

```python
def get_user_choice():
    while True:
        user_choice = input('Rock, paper, or scissors? (r/p/s): ').lower()
        if user_choice in choices:
            return user_choice
        else:
            print('Invalid choice!')
```

**Why This Change?**

- **Code Reusability**: By moving the input logic into a function, we can reuse it without duplicating code.
- **Improved Validation**: The function continuously prompts the user until a valid input is received.
- **Simplifies Main Loop**: Removes input handling from the main game loop, making it cleaner.

### 6.3 Separating Display Logic with `display_choices()`

**Before**:

```python
print(f"You chose {emojis[user_choice]}")
print(f"Computer chose {emojis[computer_choice]}")
```

**After**:

```python
def display_choices(user_choice, computer_choice):
    print(f'You chose {emojis[user_choice]}')
    print(f'Computer chose {emojis[computer_choice]}')
```

**Why This Change?**

- **Modularity**: Encapsulates all display-related code in one function.
- **Consistency**: Ensures that the display format remains consistent throughout the game.
- **Ease of Updates**: If we decide to change how choices are displayed, we only need to update this function.

### 6.4 Isolating Game Logic with `determine_winner()`

**Before**:

```python
if user_choice == computer_choice:
    print("Tie!")
elif (user_choice == 'r' and computer_choice == 's') or \
     (user_choice == 's' and computer_choice == 'p') or \
     (user_choice == 'p' and computer_choice == 'r'):
    print("You win")
else:
    print("You lose")
```

**After**:

```python
def determine_winner(user_choice, computer_choice):
    if user_choice == computer_choice:
        print('Tie!')
    elif (
        (user_choice == ROCK and computer_choice == SCISSORS) or 
        (user_choice == SCISSORS and computer_choice == PAPER) or 
        (user_choice == PAPER and computer_choice == ROCK)
    ):
        print('You win')
    else:
        print('You lose')  
```

**Why This Change?**

- **Clarity**: Moving the game logic into a function makes it clear that this function is responsible for determining the winner.
- **Ease of Modification**: If we need to change the game rules or add new options, we can do so within this function.
- **Testability**: We can independently test this function to ensure the game logic is correct.

### 6.5 Organizing the Game Flow with `play_game()`

**Before**:

The main game loop contains all the logic:

```python
while True:
    # Input handling
    # Computer choice
    # Display choices
    # Determine winner
    # Replay option
```

**After**:

```python
def play_game():
    while True:
        user_choice = get_user_choice()
        computer_choice = random.choice(choices)
        display_choices(user_choice, computer_choice)
        determine_winner(user_choice, computer_choice)
        should_continue = input('Continue? (y/n): ').lower()
        if should_continue == 'n':
            break

play_game()
```

**Why This Change?**

- **Encapsulation**: Encapsulating the game loop within `play_game()` keeps the global scope clean.
- **Scalability**: If we add more functions or features (like a main menu), they won't interfere with the game loop.
- **Improved Structure**: Clearly defines the starting point of the game, making the code easier to navigate.

### 6.6 General Benefits of Refactoring

- **Readability**: The code is easier to read and understand, with well-named functions and variables.
- **Maintainability**: Easier to fix bugs or add new features since related code is grouped together.
- **Reusability**: Functions can be reused or repurposed in other programs or modules.
- **Testing**: Individual functions can be tested separately, ensuring each part works correctly.

---

## Conclusion

By starting with a basic implementation and gradually refactoring, we've improved our code's structure and maintainability. Breaking the code into functions and using constants makes it more modular and easier to read, debug, and extend in the future.

**Key Takeaways:**

- **Use Constants**: Replace hardcoded values with constants for clarity and ease of maintenance.
- **Modularize Code**: Break down the program into functions, each handling a specific task.
- **Separate Concerns**: Keep input handling, display logic, and game logic in separate functions.
- **Encapsulate Game Flow**: Use a main function to control the game's execution flow.

---

## Additional Exercises

1. **Extend the Game**: Modify the game to include "Lizard" and "Spock" options, following the extended rules of Rock, Paper, Scissors, Lizard, Spock.
   - **Considerations**: How will you update the `determine_winner()` function? How will you manage the increased complexity?
2. **Score Tracking**: Implement a scoring system that keeps track of how many games the user has won, lost, or tied.
   - **Implementation**: Introduce global or passed variables to keep score and display them after each round.
3. **Input Enhancements**: Allow the user to type the full word ('rock', 'paper', 'scissors') in addition to the single-letter abbreviation.
   - **Approach**: Update `get_user_choice()` to accept multiple valid inputs for each choice.

---


By understanding the reasons behind each refactoring choice, you're better equipped to write clean, efficient, and maintainable code. Always consider how your code can be improved, not just to make it work, but to make it better.
