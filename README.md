# Tic-Tac-Toe-Game-Terminal-Based-C-

This project is a terminal-based Tic Tac Toe game written in C that supports both two-player mode and player vs. computer mode. The game uses a 3×3 character array to represent the board and is structured using modular functions for clarity and maintainability.

Players take turns placing their marks (X or O) by entering row and column values through the terminal. The program includes robust logic to validate user input, prevent invalid or duplicate moves, and determine game outcomes.

In computer mode, the program uses a randomized move generator seeded with the system clock to allow the computer to select available board positions automatically.

Key Features:
Two gameplay modes: Player vs Player and Player vs Computer
Terminal-based interactive game board
Win detection across rows, columns, and diagonals
Draw detection when the board is full
Input validation for out-of-range and occupied positions
Modular function design for game logic and display

Concepts & Techniques Used:
2D arrays for board representation
Functions and modular programming
Conditional logic and loops
Random number generation (rand, srand)
Input handling and validation
Boolean logic using <stdbool.h>

Challenges & Lessons Learned:
Handling real-time updates required careful use of system time functions and continuous looping without freezing or crashing the program.
Detecting a specific date and time (midnight on January 1st) helped reinforce the importance of precise conditional logic.
Formatting terminal output in a clean and readable way required multiple iterations.
This project helped me gain confidence working with time-based logic, formatted output, and program flow control in C.

Future Improvements:
Add decision to choose AI difficulty.
Replace random AI with a rule-based or Minimax decision algorithm.
Improve input robustness by handling malformed input and clearing the input buffer (scanf may fail on bad input).
Add replay functionality to allow multiple games in one session.
Improve terminal UI by clearing the screen between turns and enhancing board readability.

    #include <stdio.h>
    #include <stdbool.h>
    #include <stdlib.h>
    #include <time.h>

    void printBoard(char board[3][3]);
    bool checkWin(char board[3][3], char player);
    bool checkDraw(char board[3][3]);
    bool getMove(int *row, int *col);
    void computerMove(char board[3][3]);

    int main(void)
    {
    char board[3][3] = {
        {' ', ' ', ' '},
        {' ', ' ', ' '},
        {' ', ' ', ' '}
    };

    int mode;
    char currentPlayer = 'X';

    srand(time(NULL));

    printf("Select mode:\n");
    printf("1. Two Players\n");
    printf("2. Play Against Computer\n");
    printf("Choice: ");
    scanf("%d", &mode);

    while (1)
    {
        printBoard(board);

        int row, col;

        if (mode == 2 && currentPlayer == 'O')
        {
            computerMove(board);
        }
        else
        {
            printf("Player %c's turn\n", currentPlayer);

            if (!getMove(&row, &col))
            {
                printf("Invalid input!\n");
                continue;
            }

            row--;
            col--;

            if (row < 0 || row > 2 || col < 0 || col > 2)
            {
                printf("Invalid position!\n");
                continue;
            }

            if (board[row][col] != ' ')
            {
                printf("Spot already taken!\n");
                continue;
            }

            board[row][col] = currentPlayer;
        }

        if (checkWin(board, currentPlayer))
        {
            printBoard(board);
            printf("Player %c wins!\n", currentPlayer);
            break;
        }

        if (checkDraw(board))
        {
            printBoard(board);
            printf("It's a draw!\n");
            break;
        }

        currentPlayer = (currentPlayer == 'X') ? 'O' : 'X';
    }

    return 0;
    }

    void printBoard(char board[3][3])
    {
    for (int i = 0; i < 3; i++)
    {
        printf(" %c | %c | %c \n",
               board[i][0],
               board[i][1],
               board[i][2]);
        if (i < 2)
            printf("---+---+---\n");
    }
    printf("\n");
    }

    bool checkWin(char board[3][3], char player)
    {
    for (int i = 0; i < 3; i++)
    {
        if ((board[i][0] == player &&
             board[i][1] == player &&
             board[i][2] == player) ||

            (board[0][i] == player &&
             board[1][i] == player &&
             board[2][i] == player))
            return true;
    }

    if ((board[0][0] == player &&
         board[1][1] == player &&
         board[2][2] == player) ||

        (board[0][2] == player &&
         board[1][1] == player &&
         board[2][0] == player))
        return true;

    return false;
    }

    bool checkDraw(char board[3][3])
    {
    for (int i = 0; i < 3; i++)
        for (int j = 0; j < 3; j++)
            if (board[i][j] == ' ')
                return false;
    return true;
    }

    bool getMove(int *row, int *col)
    {
    printf("Enter row (1-3): ");
    if (scanf("%d", row) != 1)
        return false;

    printf("Enter column (1-3): ");
    if (scanf("%d", col) != 1)
        return false;

    return true;
    }

    void computerMove(char board[3][3])
    {
    int row, col;

    do
    {
        row = rand() % 3;
        col = rand() % 3;
    } while (board[row][col] != ' ');

    board[row][col] = 'O';
    }
