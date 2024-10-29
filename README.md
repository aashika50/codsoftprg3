#include <iostream>
#include <vector>

// Function to display the Tic-Tac-Toe board
void displayBoard(const std::vector<char>& board) {
    std::cout << "\n";
    for (int i = 0; i < 9; i += 3) {
        std::cout << " " << board[i] << " | " << board[i + 1] << " | " << board[i + 2] << "\n";
        if (i < 6) std::cout << "---|---|---\n";
    }
    std::cout << "\n";
}

// Function to check for a win
bool checkWin(const std::vector<char>& board, char player) {
    // Winning combinations
    const int winCombinations[8][3] = {
        {0, 1, 2}, {3, 4, 5}, {6, 7, 8},  // rows
        {0, 3, 6}, {1, 4, 7}, {2, 5, 8},  // columns
        {0, 4, 8}, {2, 4, 6}              // diagonals
    };

    for (auto& combination : winCombinations) {
        if (board[combination[0]] == player && 
            board[combination[1]] == player && 
            board[combination[2]] == player) {
            return true;
        }
    }
    return false;
}

// Function to check for a draw
bool checkDraw(const std::vector<char>& board) {
    for (char cell : board) {
        if (cell == ' ') return false;
    }
    return true;
}

int main() {
    char playAgain = 'y';
    while (playAgain == 'y' || playAgain == 'Y') {
        // Initialize game board with empty spaces
        std::vector<char> board(9, ' ');

        char currentPlayer = 'X';
        bool gameWon = false;
        bool gameDraw = false;

        std::cout << "Welcome to Tic-Tac-Toe!\n";
        displayBoard(board);

        while (!gameWon && !gameDraw) {
            int move;
            std::cout << "Player " << currentPlayer << ", enter your move (1-9): ";
            std::cin >> move;
            move--;  // Adjust for 0-based index

            // Validate move
            if (move < 0 || move >= 9 || board[move] != ' ') {
                std::cout << "Invalid move. Try again.\n";
                continue;
            }

            // Update the board
            board[move] = currentPlayer;
            displayBoard(board);

            // Check for win or draw
            gameWon = checkWin(board, currentPlayer);
            gameDraw = checkDraw(board);

            if (gameWon) {
                std::cout << "Player " << currentPlayer << " wins!\n";
            } else if (gameDraw) {
                std::cout << "The game is a draw!\n";
            } else {
                // Switch player
                currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
            }
        }

        // Ask if players want to play again
        std::cout << "Do you want to play again? (y/n): ";
        std::cin >> playAgain;
    }

    return 0;
}
