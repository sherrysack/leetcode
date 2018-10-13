## 37 Sudoku Solver

Write a program to solve a Sudoku puzzle by filling the empty cells.

A sudoku solution must satisfy **all of the following rules**:

1. Each of the digits `1-9` must occur exactly once in each row.
2. Each of the digits `1-9` must occur exactly once in each column.
3. Each of the the digits `1-9` must occur exactly once in each of the 9 `3x3` sub-boxes of the grid.

Empty cells are indicated by the character `'.'`.

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/f/ff/Sudoku-by-L2G-20050714.svg/250px-Sudoku-by-L2G-20050714.svg.png)
A sudoku puzzle...

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/3/31/Sudoku-by-L2G-20050714_solution.svg/250px-Sudoku-by-L2G-20050714_solution.svg.png)
...and its solution numbers marked in red.

**Note:**

- The given board contain only digits `1-9` and the character `'.'`.
- You may assume that the given Sudoku puzzle will have a single unique solution.
- The given board size is always `9x9`.



```java
public class Solution {
    public void solveSudoku(char[][] board) {
        if(board == null || board.length == 0) {
            return;
        }
        solve(board);
    }

    public boolean solve(char[][] board) {
        for(int i = 0; i < board.length; i++) {
            for(int j = 0; j < board.length; j++) {
                if(board[i][j] == '.') {
                    for(char c = '1'; c <= '9'; c++) {
                        if(isValid(board, i, j, c)) {
                            board[i][j] = c;
                            if(solve(board))
                                return true;
                            else
                            //this number is wrong, try the next one
                                board[i][j] = '.';
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    } 

    private boolean isValid(char[][] board, int row, int col, char c) {
        for(int i = 0; i < 9; i++) {
            if(board[i][col] != '.' && board[i][col] == c) return false;
            if(board[row][i] != '.' && board[row][i] == c) return false;
            //check whether c has existed in the square
            int diagrow = 3*(row/3) +i/3;
            int diagcol = 3*(col/3) +i%3;
            if(board[diagrow][diagcol] != '.' && board[diagrow][diagcol] == c) return false;
        }
        return true;
    }
}
```

