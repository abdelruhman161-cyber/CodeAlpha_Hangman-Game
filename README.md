# Text-based Hangman Game
# This script runs entirely in the console and meets the CodeAlpha internship requirements.

import random

# Hardcoded list of exactly 5 words used for the secret word selection.
WORD_LIST = ["food", "apple", "banana", "dog", "cat"]

# Maximum number of incorrect guesses allowed before the player loses.
MAX_INCORRECT_GUESSES = 6


def choose_secret_word():
    """Randomly select and return one word from the word list."""
    return random.choice(WORD_LIST)


def display_current_progress(secret_word, guessed_letters):
    """Return a string showing the secret word with underscores for unguessed letters."""
    displayed = []
    for letter in secret_word:
        if letter in guessed_letters:
            displayed.append(letter)
        else:
            displayed.append("_")
    return " ".join(displayed)


def is_word_guessed(secret_word, guessed_letters):
    """Check if every letter in the secret word has been guessed."""
    for letter in secret_word:
        if letter not in guessed_letters:
            return False
    return True


def play_hangman():
    """Run the main Hangman game loop in the console."""
    secret_word = choose_secret_word()
    guessed_letters = []
    incorrect_guesses = 0

    print("Welcome to Hangman!")
    print("Guess the word before you reach 6 incorrect guesses.")
    print(f"The secret word has {len(secret_word)} letters.")

    while incorrect_guesses < MAX_INCORRECT_GUESSES:
        print("\n" + display_current_progress(secret_word, guessed_letters))
        guess = input("Enter a single letter: ").strip().lower()

        if len(guess) != 1 or not guess.isalpha():
            print("Please enter exactly one letter.")
            continue

        if guess in guessed_letters:
            print("You already guessed that letter. Try a different one.")
            continue

        guessed_letters.append(guess)

        if guess in secret_word:
            print(f"Good job! '{guess}' is in the word.")
        else:
            incorrect_guesses += 1
            remaining = MAX_INCORRECT_GUESSES - incorrect_guesses
            print(f"Sorry, '{guess}' is not in the word.")
            print(f"Incorrect guesses: {incorrect_guesses} / {MAX_INCORRECT_GUESSES}")
            print(f"You have {remaining} guesses left.")

        if is_word_guessed(secret_word, guessed_letters):
            print("\n" + display_current_progress(secret_word, guessed_letters))
            print("Congratulations! You guessed the word and won the game.")
            break

    else:
        print(f"\nYou have used all {MAX_INCORRECT_GUESSES} incorrect guesses.")
        print(f"Game over. The secret word was '{secret_word}'.")


if __name__ == "__main__":
    play_hangman()
