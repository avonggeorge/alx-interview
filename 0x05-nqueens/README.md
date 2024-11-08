# N Queens

The **N Queens problem** is a classic algorithmic challenge where the objective is to place N queens on an N×NN \times NN×N chessboard so that no two queens threaten each other. This means:

1.  No two queens should be on the same row.
2.  No two queens should be on the same column.
3.  No two queens should be on the same diagonal.

### Solution Approaches

The N Queens problem is often solved using **Backtracking**, a technique that incrementally builds candidates to the solution and abandons a candidate ("backtracks") as soon as it determines the candidate cannot lead to a valid solution.

Here's a step-by-step breakdown of how backtracking is used for the N Queens problem:

1.  **Start on the first row** and try placing a queen in each column of the row, checking if it's a valid position.
2.  **Move to the next row** and attempt to place a queen in a position that doesn’t conflict with any queens already placed.
3.  If a conflict arises (no valid position in the current row), **backtrack** to the previous row and move the queen to the next column.
4.  Repeat this process until you either:
    -   Place all N queens on the board (solution found).
    -   Exhaust all options (no solution for current configuration).

### Code Example (Python)

Here's a Python implementation using backtracking to solve the N Queens problem:).
```
def solve_n_queens(n):
    def is_valid(board, row, col):
        # Check column
        for i in range(row):
            if board[i] == col:
                return False
        # Check upper left diagonal
        for i, j in zip(range(row, -1, -1), range(col, -1, -1)):
            if board[i] == j:
                return False
        # Check upper right diagonal
        for i, j in zip(range(row, -1, -1), range(col, n)):
            if board[i] == j:
                return False
        return True

    def place_queen(row, board, solutions):
        if row == n:
            solutions.append(board[:])
            return
        for col in range(n):
            if is_valid(board, row, col):
                board[row] = col
                place_queen(row + 1, board, solutions)
                board[row] = -1  # Backtrack

    solutions = []
    board = [-1] * n  # -1 indicates no queen placed in that row
    place_queen(0, board, solutions)
    return solutions

# Display solutions
n = 4  # Change n to any number to test with different board sizes
solutions = solve_n_queens(n)
for solution in solutions:
    for row in solution:
        print("".join("Q" if i == row else "." for i in range(n)))
    print()
```
### Explanation of Code

-   **`is_valid(board, row, col)`**: Checks if placing a queen at `(row, col)` is safe.
-   **`place_queen(row, board, solutions)`**: Recursively attempts to place queens row-by-row. If all rows are filled, the solution is appended to the `solutions` list.
-   **`solutions`**: Stores all valid board configurations.

### Visual Representation of Solutions

For example, for N=4N = 4N=4, a solution might look like this:
```
. Q . .
. . . Q
Q . . .
. . Q .
```
Each `Q` represents a queen, and each `.` represents an empty space on the board.