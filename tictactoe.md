# 🎮 Tic-Tac-Toe Game Assignment

## My flowchart:
<img width="1024" height="768" alt="Green and White Simple Colorful Flowchart Graph (1)" src="https://github.com/user-attachments/assets/06b359ff-89f6-4813-a0bc-b4d3564c23dc" />

## My Code:
```cpp
#include <iostream>
#include <limits>
using namespace std;

class TicTacToe {
private:
    char board[3][3];     // holds 'X', 'O', or spaces
    char currentPlayer;   // keeps track of whose turn it is
    int moveCount;        // total number of moves made in the game

public:
    // Constructor sets the board to empty and starts with Player X
    TicTacToe() {
        for (int r = 0; r < 3; r++) {
            for (int c = 0; c < 3; c++) {
                board[r][c] = ' ';
            }
        }
        currentPlayer = 'X';
        moveCount = 0;
    }

    // Prints the board in a simple grid so players can see what's going on
    void displayBoard() {
        cout << "\n   1   2   3\n";
        cout << "  -----------\n";
        for (int r = 0; r < 3; r++) {
            cout << r + 1 << " | ";
            for (int c = 0; c < 3; c++) {
                cout << board[r][c];
                if (c < 2) cout << " | ";
            }
            cout << "\n";
            if (r < 2) cout << "  -----------\n";
        }
        cout << "\n";
    }

    // Checks if the chosen spot is within range and not already taken
    bool isValidMove(int row, int col) {
        if (row < 0 || row >= 3 || col < 0 || col >= 3) {
            return false; // out of bounds
        }
        return board[row][col] == ' ';
    }

    // Actually places X or O onto the board
    void makeMove(int row, int col) {
        board[row][col] = currentPlayer;
        moveCount++;
    }

    // Looks for any winning combination for the current player
    bool checkWinner() {
        // check rows
        for (int r = 0; r < 3; r++) {
            if (board[r][0] == currentPlayer &&
                board[r][1] == currentPlayer &&
                board[r][2] == currentPlayer) {
                return true;
            }
        }

        // check columns
        for (int c = 0; c < 3; c++) {
            if (board[0][c] == currentPlayer &&
                board[1][c] == currentPlayer &&
                board[2][c] == currentPlayer) {
                return true;
            }
        }

        // check both diagonals
        if (board[0][0] == currentPlayer &&
            board[1][1] == currentPlayer &&
            board[2][2] == currentPlayer) {
            return true;
        }

        if (board[0][2] == currentPlayer &&
            board[1][1] == currentPlayer &&
            board[2][0] == currentPlayer) {
            return true;
        }

        return false;
    }

    // Game is a draw when all 9 spots are filled and no one won
    bool isDraw() {
        return moveCount == 9;
    }

    // Switch between X and O each turn
    void switchPlayer() {
        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    // Main loop that keeps the game running until someone wins or it's a draw
    void playGame() {
        cout << "Welcome to Tic-Tac-Toe!\n";
        cout << "Player X goes first.\n";

        bool finished = false;

        while (!finished) {
            displayBoard();
            cout << "Player " << currentPlayer << ", enter your move.\n";

            int row, col;

            while (true) {
                cout << "Row (1-3): ";
                cin >> row;
                cout << "Column (1-3): ";
                cin >> col;

                // If input is not a number, clear error and re-ask
                if (cin.fail()) {
                    cout << "Please enter valid numbers.\n";
                    cin.clear();
                    cin.ignore(numeric_limits<streamsize>::max(), '\n');
                    continue;
                }

                row -= 1;
                col -= 1;

                if (!isValidMove(row, col)) {
                    cout << "That spot isn't available. Try again.\n";
                } else {
                    break;
                }
            }

            makeMove(row, col);

            if (checkWinner()) {
                displayBoard();
                cout << "Player " << currentPlayer << " wins!\n";
                finished = true;
            } else if (isDraw()) {
                displayBoard();
                cout << "No more moves left. It's a draw.\n";
                finished = true;
            } else {
                switchPlayer();
            }
        }

        cout << "Thanks for playing!\n";
    }
};

int main() {
    TicTacToe game;
    game.playGame();
    return 0;
}
```

## By Sophia Ramirez
