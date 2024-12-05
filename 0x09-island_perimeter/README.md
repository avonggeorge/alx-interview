# Island Perimeter

The **Island Perimeter** problem is a common coding exercise where you calculate the perimeter of an island represented as a 2D grid. The grid consists of 0s and 1s, where:

-   `1` represents land.
-   `0` represents water.

The task is to compute the total perimeter of the island. An island is formed by connected land cells (1s), and the grid cells are connected horizontally or vertically (not diagonally). Each land cell contributes 4 to the perimeter initially, and then shared edges between two land cells reduce the perimeter.

### Example Problem

Given the grid:
```
grid = [    [0, 1, 0, 0],
    [1, 1, 1, 0],
    [0, 1, 0, 0],
    [1, 1, 0, 0]
]
```
**Steps to Calculate the Perimeter:**

1.  Iterate through the grid.
2.  For each land cell (1):
    -   Add 4 to the perimeter.
    -   Subtract 2 for each shared edge with another land cell.

**Solution in Python:**
```
def island_perimeter(grid):
    rows = len(grid)
    cols = len(grid[0])
    perimeter = 0

    for r in range(rows):
        for c in range(cols):
            if grid[r][c] == 1:  # Land cell
                perimeter += 4
                # Check for shared edges
                if r > 0 and grid[r-1][c] == 1:  # Up
                    perimeter -= 2
                if c > 0 and grid[r][c-1] == 1:  # Left
                    perimeter -= 2
    return perimeter

# Test
grid = [
    [0, 1, 0, 0],
    [1, 1, 1, 0],
    [0, 1, 0, 0],
    [1, 1, 0, 0]
]
print(island_perimeter(grid))  # Output: 16
```



### Key Points:

1.  **Perimeter Calculation**:
    -   Each land cell starts with a perimeter of 4.
    -   Reduce the perimeter by 2 for each shared edge with adjacent land cells.
2.  **Edge Cases**:
    -   The grid is empty.
    -   All cells are water (`0`).
    -   The island is completely surrounded by water.

This approach ensures an efficient solution with O(n×m)O(n \times m)O(n×m) time complexity for a grid with nnn rows and mmm columns.

### Concepts Needed:

1.  **2D Arrays (Matrices)**:
    
    -   Accessing and iterating over elements in a 2D array.
    -   Understanding how to navigate through adjacent cells (horizontally and vertically).
2.  **Conditional Logic**:
    
    -   Applying conditions to determine whether a cell contributes to the perimeter of the island.
3.  **Counting Techniques**:
    
    -   Developing a method to count the edges that contribute to the island’s perimeter.
4.  **Problem-Solving Strategies**:
    
    -   Breaking down the problem into smaller tasks, such as identifying land cells and calculating their contribution to the perimeter.
5.  **Python Programming**:
    
    -   Nested loops for iterating over grid cells.
    -   Conditional statements to check the status of adjacent cells.

### Resources:

-   **Python Official Documentation**:
    
    -   [Nested Lists](https://intranet.alxswe.com/rltoken/8SPalOgoGDWQChVbct0p1g "Nested Lists"): Understanding how to work with lists within lists in Python.
-   **GeeksforGeeks Articles**:
    
    -   [Python Multi-dimensional Arrays](https://intranet.alxswe.com/rltoken/IYcYmeVlCfF-F7Szn1fzfQ "Python Multi-dimensional Arrays"): A guide to working with 2D arrays in Python effectively.
-   **TutorialsPoint**:
    
    -   [Python Lists](https://intranet.alxswe.com/rltoken/TZ8UtQaRxN5cFf8c1TB-rw "Python Lists"): Explains how to create, access, and manipulate lists in Python, which is essential for working with a grid.
-   **YouTube Tutorials**:
    
    -   [Python 2D arrays and lists](https://intranet.alxswe.com/rltoken/H7SwlI_XYDpwYonNYKXQfg "Python 2D arrays and lists")

By understanding these concepts and utilizing the provided resources, you will be equipped to approach the problem methodically. You’ll need to iterate over the grid, apply logical operations to identify the perimeter of the island, and account for the specific conditions described in the task. This project not only tests your algorithmic thinking but also reinforces your ability to manipulate data structures and apply logical reasoning to solve problems.
