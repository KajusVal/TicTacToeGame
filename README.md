# Tic Tac Toe Game

## Introduction
#### What is your application?

The application is a Tic Tac Toe game and it is implemented in Python. It can be played either as a single-player (against AI) or as a multi-player game (against other opponent).

#### How to run the program?

To run the program, you must execute Python script. It starts on cmd (if played on Windows).

#### How to use the program?

Players make turns by choosing number from 1-9. Their chosen number shows the exact position they are picking. After every move, the table updates and shows the updated table. Players choose free spaces in order to connect 3 X or O symbols. If neither is able to connect their chosen symbols, then the game is finished as draw.

## Body/Analysis

#### Object-Oriented Programming Pillars

1. **Polymorphism**: Polymorphism allows objects of different classes to be treated as objects of a common superclass. In the Tic Tac Toe program, polymorphism is demonstrated through the `make_move()` method in the `Player` base class and its subclasses `HumanPlayer` and `AIPlayer`. Despite having different implementations, they can be used interchangeably within the game.

```python
class Player:
    def make_move(self, board):
        pass

class HumanPlayer(Player):
    def make_move(self, board):
        # Human player input handling
        pass

class AIPlayer(Player):
    def make_move(self, board):
        # AI player move generation
        pass
```

2. **Abstraction**: Abstraction allows the hiding of complex implementation details and showing only the essential features of an object. In our program, abstraction is achieved through the `Player` base class, which defines the common attributes and methods shared by all player types without exposing the specific implementation details.

```python
class Player:
    def __init__(self, symbol):
        self.symbol = symbol

    def make_move(self, board):
        pass
```

3. **Inheritance**: Inheritance allows a new class to inherit properties and behaviour from an existing class. For instance, `HumanPlayer` and `AIPlayer` inherit from the `Player` base class, inheriting its attributes and methods. This promotes code reuse and enhances maintainability.

```python
class HumanPlayer(Player):
    def make_move(self, board):
        # Human player input handling
        pass

class AIPlayer(Player):
    def make_move(self, board):
        # AI player move generation
        pass
```

4. **Encapsulation**: Encapsulation involves bundling the data and methods that operate on the data into a single unit, restricting access to some of the object's components. In our program, encapsulation is demonstrated through the private attributes of the `Game` class, such as `board`, `current_player`, and `players`, which are accessed and modified only through designated methods.

```python
class Game:
    def __init__(self):
        self.board = [' ' for _ in range(9)]
        self.current_player = None
        self.players = []

    def initialize_players(self):
        # Player initialization
        pass
```

#### Design patterns

1. **Factory Method Pattern**: The Factory Method Pattern is used to encapsulate the creation of objects and allows subclasses to alter the type of objects that will be created. In our program, the `PlayerFactory` class serves as a factory method that creates player objects based on the player type provided as input, either **human** or **ai**.

```python
class PlayerFactory:
    @staticmethod
    def create_player(player_type, symbol):
        if player_type == 'human':
            return HumanPlayer(symbol)
        elif player_type == 'ai':
            return AIPlayer(symbol)
        else:
            raise ValueError("Invalid player type")
```

2. **Singleton Pattern**: The Singleton Pattern ensures that a class has only one instance and provides a global point of access to that instance. In our program, the `Game` class is implemented as a Singleton to ensure that only one instance of the game exists throughout its lifecycle, maintaining consistency and avoiding unintended multiple game instances.
```python
class Game:
    _instance = None

    @staticmethod
    def get_instance():
        if Game._instance is None:
            Game._instance = Game()
        return Game._instance
```

#### File I/O Operations
The Tic Tac Toe program includes file I/O functions for importing and exporting player statistics stored in a CSV file. These functions allow saving and retrieving game results, enabling persistence and analysis of player performance over time.
```python
def read_csv(filename):
    # Read player statistics from CSV
    pass

def write_csv(filename, data):
    # Write player statistics to CSV
    pass
```

#### Unit Testing with `unittest`
To ensure the core functionality of the Tic Tac Toe game is reliable, unit tests are implemented using Python's `unittest` framework. These tests cover key aspects of the game's behaviour and verify that it meets the defined objectives and functional requirements.

#### Step 1: Import Required Modules and Classes

```python
import unittest
from unittest.mock import patch
import io
import os
from TicTacToeGame import Game
```
#### Step 2: Set Up Test Cases
Before running each test case, the game instance is initialized with an empty board and no players.
```python
class TestTicTacToeGame(unittest.TestCase):

    def setUp(self):
        self.game = Game.get_instance()
        self.game.board = [' ' for _ in range(9)]
        self.game.players = []
```
#### Step 3: Test for Winning Conditions
A test case is created to verify the functionality of the `check_winner()` method, ensuring it correctly identifies winning conditions on the game board.
```python
    def test_check_winner(self):
        # Test horizontal win
        self.game.board = ['X', 'X', 'X', ' ', ' ', ' ', ' ', ' ', ' ']
        self.assertTrue(self.game.check_winner('X'))
```
#### Step 4: Test for Draw Condition
Another test case checks the `check_draw()` method to confirm that it accurately detects a draw scenario on the game board.
```python
    def test_check_draw(self):
        # Test for draw
        self.game.board = ['X', 'O', 'X', 'O', 'X', 'O', 'O', 'X', 'O']
        self.assertTrue(self.game.check_draw())

        # Test for not draw
        self.game.board = ['X', ' ', 'X', 'O', 'X', 'O', 'O', 'X', 'O']
        self.assertFalse(self.game.check_draw())
```
#### Step 5: Simulate Game Play
A test case is created to simulate a complete game play, using mock input for symbol selection and player types, and capturing the game's output to verify correct behaviour.
```python
    @patch('sys.stdout', new_callable=io.StringIO)
    @patch('builtins.input', side_effect=['X', 'human', 'O', 'ai'])
    def test_play_game(self, mock_input, mock_stdout):
        self.game.play()
        output = mock_stdout.getvalue()
        self.assertIn("It's a draw!", output)
```
#### Step 6: Clean Up After Testing
After all test cases have been executed, the `tearDown()` method ensures any temporary files created during testing are removed, maintaining a clean testing environment.
```python
    def tearDown(self):
        if os.path.exists('game_results.csv'):
            os.remove('game_results.csv')
```
#### Implementation Explanation
**Unit Testing:** The `unittest` framework is utilized to test various components of the Tic Tac Toe game, ensuring each function behaves as expected under different conditions.

**Set Up:** Before running each test case, the game instance is initialized with an empty board and no players, ensuring a consistent starting point for testing.

**Test Cases:** Test cases cover critical aspects of the game's behaviour, including winning conditions, draw detection, and overall game play.

**Mock Input:** Mock input is used to simulate user interactions during game play, allowing for thorough testing of game logic and output.

**Clean Up:** The `tearDown()` method removes any temporary files created during testing, ensuring a clean testing environment for subsequent test runs.



## Results
* The implementation of the Tic Tac Toe game program resulted in a functional game that allows players to compete against each other or an AI opponent.
*  Making this program made me realise, that I enjoy programming, even though it requires a lot of learning and you must always follow all the updates.
* Had a lot of trouble with `unittest` and could not fix all of them. This problem resulted in only 2 out of 3 tests working because I could not figure out what the problem was with `test_play_game` outcome.
* Looking forward to future, game program could be implemented with levels of difficulty when playing against AI.

## Conclusion
The coursework has resulted in the development of a interactive Tic Tac Toe game implemented in Python. Through the use of object-oriented programming principles, design patterns and unit testing, the program  achieves its objectives and functional requirements effectively. Almost all of the requirements were achieved (except for one test, which  does not work). Looking to future prospects, we can talk about many upgrades, for example, cross-platform gameplay, enhanced AI,  integrating graphical user interface (GUI) to make the game visually good-looking and additional features such as customizable symbols or boards.
