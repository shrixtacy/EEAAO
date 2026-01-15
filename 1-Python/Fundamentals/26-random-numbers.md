# Python Random Numbers - Complete Guide

## 📚 Table of Contents
1. [random Module](#random-module)
2. [Random Number Generation](#random-number-generation)
3. [Random Choices](#random-choices)
4. [Shuffling and Sampling](#shuffling-and-sampling)
5. [Madlib Game](#madlib-game)
6. [Dice Rolling Simulator](#dice-rolling-simulator)
7. [Practice Questions](#practice-questions)

---

## 1. random Module

```python
import random

# Random float between 0 and 1
print(random.random())

# Random integer
print(random.randint(1, 10))  # 1 to 10 inclusive

# Random float in range
print(random.uniform(1.0, 10.0))

# Random choice from list
colors = ['red', 'green', 'blue']
print(random.choice(colors))
```

---

## 2. Random Number Generation

```python
import random

# random() - float between 0.0 and 1.0
print(random.random())  # 0.7234...

# randint(a, b) - integer between a and b (inclusive)
print(random.randint(1, 6))  # Dice roll

# randrange(start, stop, step)
print(random.randrange(0, 100, 5))  # 0, 5, 10, ..., 95

# uniform(a, b) - float between a and b
print(random.uniform(1.5, 10.5))

# Random number with seed (reproducible)
random.seed(42)
print(random.random())  # Always same result with seed 42
```

---

## 3. Random Choices

```python
import random

# choice() - single random element
fruits = ['apple', 'banana', 'cherry', 'date']
print(random.choice(fruits))

# choices() - multiple elements with replacement
print(random.choices(fruits, k=3))

# choices() with weights
print(random.choices(fruits, weights=[10, 1, 1, 1], k=5))
# Apple is 10x more likely
```

---

## 4. Shuffling and Sampling

```python
import random

# shuffle() - shuffle list in place
deck = ['A', '2', '3', '4', '5']
random.shuffle(deck)
print(deck)

# sample() - multiple unique elements
numbers = list(range(1, 50))
lottery = random.sample(numbers, 6)
print(sorted(lottery))  # 6 unique lottery numbers
```

---

## 5. Madlib Game

```python
import random

def madlib_game():
    """Interactive Madlib Game"""
    print("=" * 50)
    print("WELCOME TO THE MADLIB GAME!")
    print("=" * 50)
    print()
    
    # Get user inputs
    adjective1 = input("Enter an adjective: ")
    noun1 = input("Enter a noun: ")
    verb1 = input("Enter a verb (past tense): ")
    adjective2 = input("Enter another adjective: ")
    noun2 = input("Enter another noun: ")
    verb2 = input("Enter another verb: ")
    adjective3 = input("Enter one more adjective: ")
    noun3 = input("Enter one more noun: ")
    animal = input("Enter an animal: ")
    food = input("Enter a food: ")
    
    # Create the story
    story = f"""
    Once upon a time, in a {adjective1} land, there lived a {noun1}.
    Every day, the {noun1} would {verb1} to the {adjective2} {noun2}.
    One day, while trying to {verb2}, the {noun1} met a {adjective3} {animal}.
    The {animal} was eating {food} near the {noun3}.
    They became best friends and lived {adjective1}ly ever after!
    """
    
    print("\n" + "=" * 50)
    print("YOUR MADLIB STORY:")
    print("=" * 50)
    print(story)

# Run the game
if __name__ == "__main__":
    madlib_game()
    
    # Ask to play again
    while input("\nPlay again? (yes/no): ").lower() == 'yes':
        print()
        madlib_game()
```

### Advanced Madlib with Multiple Stories

```python
import random

def madlib_advanced():
    """Madlib with random story selection"""
    
    stories = {
        "adventure": {
            "prompts": ["adjective", "noun", "verb", "place", "animal"],
            "template": "The {adjective} {noun} decided to {verb} to the {place} where a wild {animal} lived."
        },
        "school": {
            "prompts": ["adjective", "noun", "verb", "subject", "teacher_name"],
            "template": "At the {adjective} school, students learned to {verb} in {subject} class with Professor {teacher_name}."
        },
        "space": {
            "prompts": ["adjective", "planet", "verb", "alien_name", "spaceship"],
            "template": "The {adjective} astronaut flew to {planet} in a {spaceship} and met an alien named {alien_name} who loved to {verb}."
        }
    }
    
    # Choose random story
    story_type = random.choice(list(stories.keys()))
    story = stories[story_type]
    
    print(f"\n=== {story_type.upper()} MADLIB ===\n")
    
    # Collect inputs
    inputs = {}
    for prompt in story["prompts"]:
        inputs[prompt] = input(f"Enter a {prompt.replace('_', ' ')}: ")
    
    # Generate story
    result = story["template"].format(**inputs)
    
    print("\n" + "=" * 50)
    print("YOUR STORY:")
    print("=" * 50)
    print(result)
    print()

# Run advanced madlib
if __name__ == "__main__":
    madlib_advanced()
```

### Madlib with Random Word Suggestions

```python
import random

def madlib_with_hints():
    """Madlib that suggests random words"""
    
    word_bank = {
        "adjective": ["silly", "sparkly", "grumpy", "mysterious", "fluffy"],
        "noun": ["dragon", "pizza", "robot", "unicorn", "banana"],
        "verb": ["danced", "exploded", "whispered", "juggled", "teleported"],
        "place": ["castle", "volcano", "library", "moon", "jungle"]
    }
    
    print("=== MADLIB WITH HINTS ===\n")
    
    inputs = {}
    for word_type in ["adjective", "noun", "verb", "place"]:
        hint = random.choice(word_bank[word_type])
        word = input(f"Enter a {word_type} (hint: {hint}): ")
        inputs[word_type] = word if word else hint
    
    story = f"""
    In a {inputs['adjective']} {inputs['place']}, there was a magical {inputs['noun']}.
    Every night, it {inputs['verb']} under the stars.
    """
    
    print("\n" + "=" * 40)
    print(story)

if __name__ == "__main__":
    madlib_with_hints()
```

---

## 6. Dice Rolling Simulator

```python
import random

def roll_dice(num_dice=1, num_sides=6):
    """Simulate rolling dice"""
    rolls = []
    for _ in range(num_dice):
        roll = random.randint(1, num_sides)
        rolls.append(roll)
    return rolls

def dice_game():
    """Interactive dice rolling game"""
    print("=== DICE ROLLING SIMULATOR ===\n")
    
    while True:
        num_dice = int(input("How many dice? (1-10): "))
        num_sides = int(input("How many sides? (4, 6, 8, 10, 12, 20): "))
        
        rolls = roll_dice(num_dice, num_sides)
        
        print(f"\nYou rolled: {rolls}")
        print(f"Total: {sum(rolls)}")
        
        # Visual representation
        for i, roll in enumerate(rolls, 1):
            print(f"Die {i}: {'●' * roll}")
        
        if input("\nRoll again? (yes/no): ").lower() != 'yes':
            break

if __name__ == "__main__":
    dice_game()
```

---

## 7. Practice Questions

### Beginner Level

**Question 1**: Generate 10 random numbers between 1 and 100.
```python
# Your code here
```

**Question 2**: Create a simple coin flip simulator.
```python
# Your code here
```

**Question 3**: Generate a random password with letters and numbers.
```python
# Your code here
```

**Question 4**: Create a number guessing game.
```python
# Your code here
```

**Question 5**: Simulate rolling two dice 100 times and count how many times you get doubles.
```python
# Your code here
```

### Intermediate Level

**Question 6**: Create a lottery number generator (6 unique numbers from 1-49).
```python
# Your code here
```

**Question 7**: Build a random quote generator that displays a random quote from a list.
```python
# Your code here
```

**Question 8**: Create a card deck shuffler and deal 5 cards.
```python
# Your code here
```

**Question 9**: Build a random maze generator using random choices.
```python
# Your code here
```

**Question 10**: Create a weather simulator that generates random weather conditions.
```python
# Your code here
```

### Advanced Level

**Question 11**: Build a complete Madlib game with multiple story templates.
```python
# Your code here
```

**Question 12**: Create a Monte Carlo simulation for calculating Pi.
```python
# Your code here
```

**Question 13**: Build a random dungeon generator for a game.
```python
# Your code here
```

**Question 14**: Create a genetic algorithm using random mutations.
```python
# Your code here
```

**Question 15**: Build a random art generator that creates ASCII art.
```python
# Your code here
```

---

## 🎯 Key Takeaways

1. **random.random()** - float between 0 and 1
2. **random.randint(a, b)** - integer between a and b
3. **random.choice()** - random element from sequence
4. **random.shuffle()** - shuffle list in place
5. **random.sample()** - multiple unique elements
6. **random.seed()** - reproducible random numbers
7. Use random module for games and simulations
8. Madlib games combine user input with random stories
9. Always import random module first
10. Random numbers are pseudo-random (deterministic with seed)

---

## 📚 Additional Resources

- Python random Module: https://docs.python.org/3/library/random.html
- Random Number Generation: https://realpython.com/python-random/
- Madlib Game Ideas: Create your own story templates!

---

**Next Topic**: Multithreading
**Previous Topic**: Exception Handling

Happy Coding! 🐍
