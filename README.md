# Connect-Four-Game

import java.util.Scanner;

public class ConnectFour {
    private static final int ROWS = 6;
    private static final int COLUMNS = 7;
    private static final char EMPTY_SLOT = ' ';
    private static final char PLAYER_ONE_PIECE = 'X';
    private static final char PLAYER_TWO_PIECE = 'O';

    private char[][] board;

    public ConnectFour() {
        board = new char[ROWS][COLUMNS];
        initializeBoard();
    }

    private void initializeBoard() {
        for (int row = 0; row < ROWS; row++) {
            for (int col = 0; col < COLUMNS; col++) {
                board[row][col] = EMPTY_SLOT;
            }
        }
    }

    private void displayBoard() {
        for (int row = 0; row < ROWS; row++) {
            System.out.print("|");
            for (int col = 0; col < COLUMNS; col++) {
                System.out.print(board[row][col] + "|");
            }
            System.out.println();
        }
        System.out.println("---------------");
    }

    private boolean dropPiece(int column, char piece) {
        if (column < 0 || column >= COLUMNS) {
            System.out.println("Invalid column number!");
            return false;
        }

        for (int row = ROWS - 1; row >= 0; row--) {
            if (board[row][column] == EMPTY_SLOT) {
                board[row][column] = piece;
                return true;
            }
        }

        System.out.println("Column is full!");
        return false;
    }

    private boolean checkWin(char piece) {
        // Check horizontally
        for (int row = 0; row < ROWS; row++) {
            for (int col = 0; col <= COLUMNS - 4; col++) {
                if (board[row][col] == piece &&
                        board[row][col + 1] == piece &&
                        board[row][col + 2] == piece &&
                        board[row][col + 3] == piece) {
                    return true;
                }
            }
        }

        // Check vertically
        for (int row = 0; row <= ROWS - 4; row++) {
            for (int col = 0; col < COLUMNS; col++) {
                if (board[row][col] == piece &&
                        board[row + 1][col] == piece &&
                        board[row + 2][col] == piece &&
                        board[row + 3][col] == piece) {
                    return true;
                }
            }
        }

        // Check diagonally (top-left to bottom-right)
        for (int row = 0; row <= ROWS - 4; row++) {
            for (int col = 0; col <= COLUMNS - 4; col++) {
                if (board[row][col] == piece &&
                        board[row + 1][col + 1] == piece &&
                        board[row + 2][col + 2] == piece &&
                        board[row + 3][col + 3] == piece) {
                    return true;
                }
            }
        }

        // Check diagonally (top-right to bottom-left)
        for (int row = 0; row <= ROWS - 4; row++) {
            for (int col = 3; col < COLUMNS; col++) {
                if (board[row][col] == piece &&
                        board[row + 1][col - 1] == piece &&
                        board[row + 2][col - 2] == piece &&
                        board[row + 3][col - 3] == piece) {
                    return true;
                }
            }
        }

        return false;
    }

    public void playGame() {
        Scanner scanner = new Scanner(System.in);
        boolean playerOneTurn = true;

        System.out.println("Welcome to Connect Four!");
        displayBoard();

        while (true) {
            char piece = playerOneTurn ? PLAYER_ONE_PIECE : PLAYER_TWO_PIECE;
            int column;

            do {
                System.out.print("Player " + (playerOneTurn ? "1" : "2") + ", enter column (0-6): ");
                column = scanner.nextInt();
            } while (!dropPiece(column, piece));

            displayBoard();

            if (checkWin(piece)) {
                System.out.println("Player " + (playerOneTurn ? "1" : "2") + " wins!");
                break;
            }

            if (isBoardFull()) {
                System.out.println("It's a draw!");
                break;
            }

            playerOneTurn = !playerOneTurn;
        }

        scanner.close();
    }

    private boolean isBoardFull() {
        for (int col = 0; col < COLUMNS; col++) {
            if (board[0][col] == EMPTY_SLOT) {
                return false;
            }
        }
        return true;
    }

    public static void main(String[] args) {
        ConnectFour game = new ConnectFour();
        game.playGame();
    }
}

