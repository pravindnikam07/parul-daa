# Practical 12: Unique Paths

## 1. Aim

To write a program to find the number of **unique paths** for a robot to move from the top-left corner of an `m × n` grid to the bottom-right corner, where the robot can move only **right** or **down**.

---

## 2. Theory

A robot is initially positioned at:

```text
(0, 0)
```

and needs to reach:

```text
(m - 1, n - 1)
```

At every position, the robot can make only two movements:

1. **Right**
2. **Down**

For example, for a `3 × 3` grid:

```text
Start
  ↓
+---+---+---+
| S |   |   |
+---+---+---+
|   |   |   |
+---+---+---+
|   |   | E |
+---+---+---+
              ↑
             End
```

The robot must make:

- `m - 1` down movements
- `n - 1` right movements

For a `3 × 3` grid:

- Down = 2
- Right = 2

Total movements = 4.

The number of unique arrangements of these movements is:

```text
6
```

### Dynamic Programming Approach

Let:

```text
dp[i][j]
```

represent the number of unique paths to reach cell `(i, j)`.

A cell can be reached either:

- From the **top**
- From the **left**

Therefore:

```text
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

The first row and first column contain only one possible path.

---

## 3. Approach / Logic

We use **Dynamic Programming**.

### Initialization

For the first row:

```text
dp[0][j] = 1
```

because the robot can only move right.

For the first column:

```text
dp[i][0] = 1
```

because the robot can only move down.

### Recurrence

For every other cell:

```text
dp[i][j] = dp[i-1][j] + dp[i][j-1]
```

Finally:

```text
dp[m-1][n-1]
```

contains the total number of unique paths.

---

# 4. Algorithm

1. Read the values of `m` and `n`.
2. Create an `m × n` DP table.
3. Initialize every cell in the first row to `1`.
4. Initialize every cell in the first column to `1`.
5. Traverse the remaining cells.
6. For each cell `(i, j)`, calculate:

   ```text
   dp[i][j] = dp[i-1][j] + dp[i][j-1]
   ```

7. Return `dp[m-1][n-1]`.
8. Display the number of unique paths.

---

# 5. Flowchart

```text
              START
                |
                v
          Read m and n
                |
                v
        Create DP[m][n]
                |
                v
     Initialize first row = 1
                |
                v
   Initialize first column = 1
                |
                v
          i = 1
                |
                v
           Is i < m?
          /          \
        No            Yes
        |              |
        v              v
       END            j = 1
                       |
                       v
                  Is j < n?
                 /         \
               No           Yes
               |             |
               |             v
               |      dp[i][j] =
               |      dp[i-1][j]
               |      + dp[i][j-1]
               |             |
               |             v
               |           j++
               |             |
               |             +-----> Is j < n?
               |
               v
             i++
               |
               +-----> Is i < m?
                         |
                         v
                 Print dp[m-1][n-1]
                         |
                         v
                        END
```

---

# 6. C++ Program

```cpp
#include <iostream>
#include <vector>

using namespace std;

int uniquePaths(int m, int n)
{
    vector<vector<long long>> dp(m, vector<long long>(n, 1));

    // Calculate number of paths
    for (int i = 1; i < m; i++)
    {
        for (int j = 1; j < n; j++)
        {
            dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
    }

    return dp[m - 1][n - 1];
}

int main()
{
    int m, n;

    cout << "Enter number of rows: ";
    cin >> m;

    cout << "Enter number of columns: ";
    cin >> n;

    long long result = uniquePaths(m, n);

    cout << "Number of unique paths = " << result << endl;

    return 0;
}
```

> `long long` is used instead of `int` because the number of paths can become large.

---

# 7. Java Program

```java
import java.util.*;

public class UniquePaths {

    static long uniquePaths(int m, int n) {

        long[][] dp = new long[m][n];

        // Initialize first row
        for (int j = 0; j < n; j++) {
            dp[0][j] = 1;
        }

        // Initialize first column
        for (int i = 0; i < m; i++) {
            dp[i][0] = 1;
        }

        // Calculate remaining cells
        for (int i = 1; i < m; i++) {

            for (int j = 1; j < n; j++) {

                dp[i][j] =
                    dp[i - 1][j] +
                    dp[i][j - 1];
            }
        }

        return dp[m - 1][n - 1];
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of rows: ");
        int m = sc.nextInt();

        System.out.print("Enter number of columns: ");
        int n = sc.nextInt();

        long result = uniquePaths(m, n);

        System.out.println(
            "Number of unique paths = " + result
        );

        sc.close();
    }
}
```

---

# 8. Sample Output

### Example 1

```text
Enter number of rows: 3
Enter number of columns: 7

Number of unique paths = 28
```

### Example 2

```text
Enter number of rows: 3
Enter number of columns: 3

Number of unique paths = 6
```

### Example 3

```text
Enter number of rows: 2
Enter number of columns: 2

Number of unique paths = 2
```

---

# 9. Dry Run

Consider:

```text
m = 3
n = 3
```

Initially:

```text
1 1 1
1 1 1
1 1 1
```

Now calculate the remaining cells.

### Cell `(1,1)`

```text
dp[1][1] = dp[0][1] + dp[1][0]
         = 1 + 1
         = 2
```

### Cell `(1,2)`

```text
dp[1][2] = dp[0][2] + dp[1][1]
         = 1 + 2
         = 3
```

### Cell `(2,1)`

```text
dp[2][1] = dp[1][1] + dp[2][0]
         = 2 + 1
         = 3
```

### Cell `(2,2)`

```text
dp[2][2] = dp[1][2] + dp[2][1]
         = 3 + 3
         = 6
```

Final DP table:

```text
       0   1   2
     +---+---+---+
  0  | 1 | 1 | 1 |
     +---+---+---+
  1  | 1 | 2 | 3 |
     +---+---+---+
  2  | 1 | 3 | 6 |
     +---+---+---+
```

Therefore:

```text
Number of unique paths = 6
```

---

# 10. Alternative Mathematical Approach

The robot must make:

```text
m - 1
```

down movements and:

```text
n - 1
```

right movements.

Therefore, the total number of movements is:

```text
m + n - 2
```

The number of unique arrangements can be calculated using combinations:

For `m = 3` and `n = 3`:

```text
C(4, 2) = 6
```

The DP approach is generally easier to implement and understand.

---

# 11. Time Complexity

There are `m × n` cells, and each cell is processed once.

```text
Time Complexity = O(m × n)
```

---

# 12. Space Complexity

The DP table requires:

```text
m × n
```

memory locations.

```text
Space Complexity = O(m × n)
```

### Space Optimization

We can optimize the DP solution to use only one array of size `n`:

```text
Space Complexity = O(n)
```

But the 2D DP approach is useful for understanding and tracing the solution.

---

# 13. Design Technique

**Dynamic Programming**

The problem has two important properties:

### 1. Overlapping Subproblems

The number of paths to a cell is repeatedly used when calculating paths to later cells.

### 2. Optimal Substructure

The number of paths to the current cell can be obtained from the solutions of its neighboring cells:

```text
Paths from Top + Paths from Left
```

Therefore, Dynamic Programming is suitable for this problem.

---

# 14. Important Test Cases

| `m` | `n` | Unique Paths |
| --: | --: | -----------: |
|   1 |   1 |            1 |
|   1 |   5 |            1 |
|   5 |   1 |            1 |
|   2 |   2 |            2 |
|   2 |   3 |            3 |
|   3 |   3 |            6 |
|   3 |   7 |           28 |
|   4 |   4 |           20 |

### Special Case: `1 × 1`

If:

```text
m = 1
n = 1
```

the robot is already at the destination.

Therefore:

```text
Number of unique paths = 1
```

---

# 15. Conclusion

The **Unique Paths** problem can be efficiently solved using **Dynamic Programming**. The number of paths to each cell is calculated as the sum of the paths from the cell above and the cell to the left.

For a `3 × 3` grid:

```text
1 1 1
1 2 3
1 3 6
```

Therefore, the robot has **6 unique paths** from the top-left to the bottom-right corner.
