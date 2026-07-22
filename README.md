# 🧮 Math Quiz Game

A console-based arithmetic quiz application written in **C++**. The game generates randomized math questions based on a chosen difficulty level and operation type, evaluates the player's answers in real time, and reports a final pass/fail result with a full score summary.

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Sample Gameplay](#sample-gameplay)
- [Getting Started](#getting-started)
  - [Prerequisites](#prerequisites)
  - [Build & Run](#build--run)
- [How It Works](#how-it-works)
- [Project Structure](#project-structure)
- [Enumerations](#enumerations)

## Overview

Math Quiz Game lets a player configure a short arithmetic quiz — how many questions to answer, the difficulty level, and the type of operation(s) to be tested on — and then presents randomly generated questions one at a time. Each answer is checked immediately, the console color changes to reflect a correct or incorrect response, and a final results screen summarizes the session.

## Features

- ✅ Configurable quiz length (1–10 questions)
- ✅ Four difficulty levels: **Easy**, **Medium**, **Hard**, and **Mixed**
- ✅ Five operation modes: **Addition**, **Subtraction**, **Multiplication**, **Division**, and **Mixed**
- ✅ Randomized operand generation scaled to the selected difficulty
- ✅ Instant answer validation with color-coded console feedback (green for correct, red for incorrect, with an alert sound)
- ✅ End-of-quiz summary with right/wrong answer counts and a pass/fail verdict
- ✅ Replay loop — play as many rounds as you like without restarting the program

## Sample Gameplay

```
How many Questions do you want to answer ? 3
Enter Questions Level [1] Easy, [2] Med, [3] Hard, [4] Mix ? 1
Enter Operation Type Level [1] Add, [2] Sub, [3] Mul, [4] Div, [5] Mix ? 1

Question [1/3]

7
4  +
______________________
11
Right Answer :

____________________________________

Final Results is Pass :-)
____________________________________

Number of Questions: 3
Questions Level    : Easy
OpType             : +
Number of Right Answers: 3
Number of Wrong Answers: 0
____________________________________
```

## Getting Started

### Prerequisites

- A C++ compiler supporting C++11 or later (e.g., MSVC, g++, or clang++)
- **Windows** is recommended, since the project uses `system("cls")` and `system("color ...")` for screen clearing and color output

### Build & Run

**Using Visual Studio**
1. Create a new empty C++ Console Application project.
2. Add `Project_2_solution.cpp` to the project.
3. Build and run (Ctrl+F5).

**Using g++ (Windows / MinGW)**
```bash
g++ -std=c++11 -o MathQuizGame.exe Project_2_solution.cpp
MathQuizGame.exe
```

> **Note:** On non-Windows systems, the `system("cls")` and `system("color ...")` calls will not work as intended, since these are Windows console commands. Replace them with cross-platform equivalents (e.g., ANSI escape codes) if compiling on Linux or macOS.

## How It Works

1. **Setup** – The player is prompted for the number of questions, difficulty level, and operation type.
2. **Generation** – `GenerateQuizzQuestions` builds the requested number of `stQuestion` entries via `GenerateQuestion`, which picks random operands within a range determined by difficulty (Easy: 1–10, Medium: 10–50, Hard: 50–100) and resolves "Mixed" selections into a concrete level/operation per question.
3. **Play** – `AskAndCorrectQuestionListAnswers` walks through each question, printing it, capturing the player's answer, and immediately grading it.
4. **Scoring** – Each answer updates a running count of right/wrong answers; the quiz is marked as a **Pass** once right answers are greater than or equal to wrong answers.
5. **Results** – `PrintQuizzResults` displays a final summary, after which the player can choose to play again.

## Project Structure

The project is currently implemented as a single translation unit:

```
Project_2_solution.cpp   # All game logic, data structures, and entry point
```

Logical sections within the file:

| Section | Responsibility |
|---|---|
| Enums (`enQuestionsLevel`, `enOperationType`) | Represent difficulty and operation choices |
| Utility functions (`RandomNumber`, `GetOpTypeSymbol`, `GetQuestionLevelText`, `SetScreenColor`) | Small, reusable helpers |
| Input functions (`ReadHowManyQuestions`, `ReadQuestionsLevel`, `ReadOpType`, `ReadQuestionAnswer`) | Validate and collect user input |
| `stQuestion` / `stQuizz` structs | Model a single question and the overall quiz session |
| Question generation (`GenerateQuestion`, `GenerateQuizzQuestions`) | Build randomized questions |
| Quiz flow (`AskAndCorrectQuestionListAnswers`, `CorrectTheQuestionAnswer`, `PrintTheQuestion`, `PrintQuizzResults`) | Drive gameplay and scoring |
| Game loop (`PlayMathGame`, `StartGame`, `ResetScreen`, `main`) | Orchestrate a session and support replay |

## Enumerations

**`enQuestionsLevel`**

| Value | Meaning |
|---|---|
| `EasyLevel` (1) | Operands between 1–10 |
| `MedLevel` (2) | Operands between 10–50 |
| `HardLevel` (3) | Operands between 50–100 |
| `Mix` (4) | A random level is chosen per question |

**`enOperationType`**

| Value | Meaning |
|---|---|
| `Add` (1) | Addition |
| `Sub` (2) | Subtraction |
| `Mult` (3) | Multiplication |
| `Div` (4) | Division |
| `MixOp` (5) | A random operation is chosen per question |

## Programming Concepts Used

This project was built as a practical exercise applying core procedural C++ concepts:

| Concept | Where it's used |
|---|---|
| **Enumerations (`enum`)** | `enQuestionsLevel` and `enOperationType` model fixed sets of choices (difficulty and operation) instead of magic numbers |
| **Structs** | `stQuestion` models a single question's data; `stQuizz` composes a full session, including a struct array (`QuestionList[100]`) |
| **Functions & modular decomposition** | Logic is split into small, single-purpose functions (input, generation, scoring, output) rather than one large block |
| **Function parameters passed by reference (`&`)** | `stQuizz& Quizz` is passed by reference throughout so functions can read and update the session state directly |
| **Return by value vs. return by reference** | Functions like `GenerateQuestion` return a populated struct by value, while `PrintQuizzResults` takes `stQuizz` by value (a copy) since it only reads |
| **`switch` statements** | Used in `GetOpTypeSymbol`, `SimpleCalculator`, and `GenerateQuestion` to branch on enum values |
| **Loops (`for`, `do...while`)** | `for` loops iterate through question lists; `do...while` loops validate repeated user input and control the "play again" cycle |
| **Input validation** | `ReadHowManyQuestions`, `ReadQuestionsLevel`, and `ReadOpType` all loop until a valid value is entered |
| **Random number generation** | `rand()` and `srand(time(NULL))` produce pseudo-random operands and randomly resolve "Mixed" difficulty/operation choices |
| **Type casting** | Explicit casts like `(enQuestionsLevel)QuestionLevel` convert raw `int` input into strongly-typed enum values |
| **Arrays** | A fixed-size array of `stQuestion` stores the full set of generated questions within `stQuizz` |
| **Boolean logic & state tracking** | `AnswerResult` and `isPass` track correctness and overall outcome using simple boolean flags |
| **Separation of concerns** | Input, business logic (question generation/scoring), and output (printing) are kept in distinct functions rather than mixed together |



