#### N-Queens II

```java
class Solution {
  public boolean is_not_under_attack(int row, int col, int n,
                                     int [] rows,
                                     int [] hills,
                                     int [] dales) {
    int res = rows[col] + hills[row - col + 2 * n] + dales[row + col];
    return (res == 0) ? true : false;
  }

  public int backtrack(int row, int count, int n,
                       int [] rows,
                       int [] hills,
                       int [] dales) {
    for (int col = 0; col < n; col++) {
      if (is_not_under_attack(row, col, n, rows, hills, dales)) {
        // place_queen
        rows[col] = 1;
        hills[row - col + 2 * n] = 1;  // "hill" diagonals
        dales[row + col] = 1;   //"dale" diagonals    

        // if n queens are already placed
        if (row + 1 == n) count++;
        // if not proceed to place the rest
        else count = backtrack(row + 1, count, n,
                rows, hills, dales);

        // remove queen
        rows[col] = 0;
        hills[row - col + 2 * n] = 0;
        dales[row + col] = 0;
      }
    }
    return count;
  }

  public int totalNQueens(int n) {
    int rows[] = new int[n];
    // "hill" diagonals
    int hills[] = new int[4 * n - 1];
    // "dale" diagonals
    int dales[] = new int[2 * n - 1];

    return backtrack(0, 0, n, rows, hills, dales);
  }
}```


```python
class Solution:
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
        def is_not_under_attack(row, col):
            return not (rows[col] or hills[row - col] or dales[row + col])
        
        def place_queen(row, col):
            rows[col] = 1
            hills[row - col] = 1  # "hill" diagonals
            dales[row + col] = 1  # "dale" diagonals
        
        def remove_queen(row, col):
            rows[col] = 0
            hills[row - col] = 0  # "hill" diagonals
            dales[row + col] = 0  # "dale" diagonals
        
        def backtrack(row = 0, count = 0):
            for col in range(n):
                if is_not_under_attack(row, col):
                    place_queen(row, col)
                    if row + 1 == n:
                        count += 1
                    else:
                        count = backtrack(row + 1, count)
                    remove_queen(row, col)
            return count
        
        rows = [0] * n
        hills = [0] * (2 * n - 1)  # "hill" diagonals
        dales = [0] * (2 * n - 1)  # "dale" diagonals
        return backtrack()```


```java
class Solution {
  public int backtrack(int row, int hills, int next_row, int dales, int count, int n) {
    /** 
     row: current row to place the queen
     hills: "hill" diagonals occupation [1 = taken, 0 = free]
     next_row: free and taken slots for the next row [1 = taken, 0 = free]
     dales: "dale" diagonals occupation [1 = taken, 0 = free]
     count: number of all possible solutions
     */

    // all columns available for this board, 
    // i.e. n times '1' in binary representation
    // bin(cols) = 0b1111 for n = 4, bin(cols) = 0b111 for n = 3
    // [1 = available]
    int columns = (1 << n) - 1;

    if (row == n)   // if all n queens are already placed
      count++;  // we found one more solution
    else {
      // free columns in the current row
      // ! 0 and 1 are inversed with respect to hills, next_row and dales
      // [0 = taken, 1 = free]
      int free_columns = columns & ~(hills | next_row | dales);

      // while there's still a column to place next queen
      while (free_columns != 0) {
        // the first bit '1' in a binary form of free_columns
        // on this column we will place the current queen
        int curr_column = - free_columns & free_columns;

        // place the queen 
        // and exclude the column where the queen is placed
        free_columns ^= curr_column;

        count = backtrack(row + 1,
                (hills | curr_column) << 1,
                next_row | curr_column,
                (dales | curr_column) >> 1,
                count, n);
      }
    }

    return count;
  }
  public int totalNQueens(int n) {
    return backtrack(0, 0, 0, 0, 0, n);
  }
}```


```python
class Solution:
    def totalNQueens(self, n):
        """
        :type n: int
        :rtype: int
        """
        def backtrack(row = 0, hills = 0, next_row = 0, dales = 0, count = 0):
            """
            :type row: current row to place the queen
            :type hills: "hill" diagonals occupation [1 = taken, 0 = free]
            :type next_row: free and taken slots for the next row [1 = taken, 0 = free]
            :type dales: "dale" diagonals occupation [1 = taken, 0 = free]
            :rtype: number of all possible solutions
            """
            if row == n:  # if all n queens are already placed
                count += 1  # we found one more solution
            else:
                # free columns in the current row
                # ! 0 and 1 are inversed with respect to hills, next_row and dales
                # [0 = taken, 1 = free]
                free_columns = columns & ~(hills | next_row | dales)
                
                # while there's still a column to place next queen
                while free_columns:
                    # the first bit '1' in a binary form of free_columns
                    # on this column we will place the current queen
                    curr_column = - free_columns & free_columns
                    
                    # place the queen 
                    # and exclude the column where the queen is placed
                    free_columns ^= curr_column
                    
                    count = backtrack(row + 1, 
                                      (hills | curr_column) << 1, 
                                      next_row | curr_column, 
                                      (dales | curr_column) >> 1, 
                                      count)
            return count

        # all columns available for this board, 
        # i.e. n times '1' in binary representation
        # bin(cols) = 0b1111 for n = 4, bin(cols) = 0b111 for n = 3
        # [1 = available]
        columns = (1 << n) - 1
        return backtrack()
```

