# SudokuGame
Create a program in the C programming language where the content is a method for playing and working on the Sudoku game.  
#include <stdio.h>

#define SIZE 9

// Function prototypes
void initializeBoard(int board[SIZE][SIZE]);
void displayBoard(int board[SIZE][SIZE]);
int isValidMove(int board[SIZE][SIZE], int row, int col, int num);
int isBoardFull(int board[SIZE][SIZE]);
void playGame();

int main() {
    playGame();
    return 0;
}

// Initialize the board with a sample Sudoku puzzle
void initializeBoard(int board[SIZE][SIZE]) {
    int sample[SIZE][SIZE] = {
        {5,3,0, 0,7,0, 0,0,0},
        {6,0,0, 1,9,5, 0,0,0},
        {0,9,8, 0,0,0, 0,6,0},
        
        {8,0,0, 0,6,0, 0,0,3},
        {4,0,0, 8,0,3, 0,0,1},
        {7,0,0, 0,2,0, 0,0,6},
        
        {0,6,0, 0,0,0, 2,8,0},
        {0,0,0, 4,1,9, 0,0,5},
        {0,0,0, 0,8,0, 0,7,9}
    };
    
    for(int i=0;i<SIZE;i++)
        for(int j=0;j<SIZE;j++)
            board[i][j] = sample[i][j];
}

// Display the board
void displayBoard(int board[SIZE][SIZE]) {
    for(int i=0;i<SIZE;i++) {
        if(i % 3 == 0 && i != 0)
            printf("------+-------+------\n");
        for(int j=0;j<SIZE;j++) {
            if(j % 3 == 0 && j != 0)
                printf("| ");
            if(board[i][j] == 0)
                printf("_ ");
            else
                printf("%d ", board[i][j]);
        }
        printf("\n");
    }
}

// Check if a move is valid
int isValidMove(int board[SIZE][SIZE], int row, int col, int num) {
    // Check row and column
    for(int i=0;i<SIZE;i++) {
        if(board[row][i] == num) return 0;
        if(board[i][col] == num) return 0;
    }
    
    // Check 3x3 box
    int startRow = row - row % 3;
    int startCol = col - col % 3;
    for(int i=0;i<3;i++)
        for(int j=0;j<3;j++)
            if(board[startRow+i][startCol+j] == num)
                return 0;
                
    return 1;
}

// Check if the board is full
int isBoardFull(int board[SIZE][SIZE]) {
    for(int i=0;i<SIZE;i++)
        for(int j=0;j<SIZE;j++)
            if(board[i][j] == 0)
                return 0;
    return 1;
}

// Main game loop
void playGame() {
    int board[SIZE][SIZE];
    initializeBoard(board);
    
    while(!isBoardFull(board)) {
        displayBoard(board);
        int row, col, num;
        printf("Enter row (1-9), column (1-9), number (1-9): ");
        scanf("%d %d %d", &row, &col, &num);
        row--; col--; // Adjust for 0-indexed array
        
        if(row<0 || row>=9 || col<0 || col>=9 || num<1 || num>9) {
            printf("Invalid input! Try again.\n");
            continue;
        }
        
        if(board[row][col] != 0) {
            printf("Cell already filled! Try another cell.\n");
            continue;
        }
        
        if(isValidMove(board, row, col, num)) {
            board[row][col] = num;
        } else {
            printf("Invalid move! Violates Sudoku rules.\n");
        }
    }
    
    displayBoard(board);
    printf("Congratulations! You completed the Sudoku puzzle!\n");
}
