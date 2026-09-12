# Experiment: Minimum Path Sum

## 1. Aim

To find the **minimum path sum** from the top-left cell to the bottom-right cell of a given `N × M` grid containing non-negative integers.

---

## 2. Problem Statement

Given an `N × M` grid consisting of non-negative integers, find a path from the **top-left cell `(0,0)`** to the **bottom-right cell `(N-1,M-1)`** such that the sum of all values along the path is minimum.

From each cell, movement is allowed only in two directions:

* **Right** → `(i, j+1)`
* **Down** → `(i+1, j)`

### Example

Consider:

```text
1 3 1
1 5 1
4 2 1
```

A minimum-sum path is:

```text
1 → 3 → 1 → 1 → 1
```

The path is:

```text
(0,0)
   ↓
(0,1)
   ↓
(0,2)
   ↓
(1,2)
   ↓
(2,2)
```

Sum:

```text
1 + 3 + 1 + 1 + 1 = 7
```

Therefore:

```text
Minimum Path Sum = 7
```

---

# 3. Theory

The **Minimum Path Sum** problem is a classic **Dynamic Programming** problem.

At any cell `(i,j)`, there are at most two ways to reach it:

1. From the **top**: `(i-1,j)`
2. From the **left**: `(i,j-1)`

Therefore, the minimum path sum to the current cell is obtained by taking the minimum of these two possible previous paths.

Let:

```text
dp[i][j]
```

represent the minimum path sum required to reach cell `(i,j)` from `(0,0)`.

The recurrence relation is:

For the first cell:

```text
dp[0][0] = grid[0][0]
```

For other cells:

```text
dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])
```

The final answer is:

```text
dp[N-1][M-1]
```

---

# 4. Approach / Logic

### Step 1: Initialize the starting cell

The minimum path sum at `(0,0)` is simply its own value.

```text
dp[0][0] = grid[0][0]
```

### Step 2: First row

In the first row, we can only move from **left to right**.

Therefore:

```text
dp[0][j] = dp[0][j-1] + grid[0][j]
```

### Step 3: First column

In the first column, we can only move **from top to bottom**.

Therefore:

```text
dp[i][0] = dp[i-1][0] + grid[i][0]
```

### Step 4: Remaining cells

For every other cell, choose the smaller path from:

* Top
* Left

Therefore:

```text
dp[i][j] =
grid[i][j] + min(dp[i-1][j], dp[i][j-1])
```

### Step 5

The value at the bottom-right cell gives the minimum path sum.

---

# 5. Algorithm

1. Start.
2. Read `N` and `M`.
3. Read the `N × M` grid.
4. Create a DP table of size `N × M`.
5. Set:

   ```text
   dp[0][0] = grid[0][0]
   ```
6. Fill the first row using:

   ```text
   dp[0][j] = dp[0][j-1] + grid[0][j]
   ```
7. Fill the first column using:

   ```text
   dp[i][0] = dp[i-1][0] + grid[i][0]
   ```
8. For every remaining cell:

   ```text
   dp[i][j] = grid[i][j] +
              min(dp[i-1][j], dp[i][j-1])
   ```
9. Return `dp[N-1][M-1]`.
10. Stop.

---

# 6. Flowchart

```text
                 ┌─────────────┐
                 │    START    │
                 └──────┬──────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Read N, M and      │
             │ the grid           │
             └──────────┬─────────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Create DP table    │
             └──────────┬─────────┘
                        │
                        ▼
             ┌────────────────────┐
             │ dp[0][0] =         │
             │ grid[0][0]         │
             └──────────┬─────────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Fill first row     │
             └──────────┬─────────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Fill first column  │
             └──────────┬─────────┘
                        │
                        ▼
                ┌──────────────┐
                │ Process each │
                │ remaining    │
                │ cell         │
                └──────┬───────┘
                       │
                       ▼
             ┌─────────────────────┐
             │ Find minimum of     │
             │ top and left path   │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ dp[i][j] = grid[i][j│
             │ + minimum path      │
             └──────────┬──────────┘
                        │
                        ▼
             ┌─────────────────────┐
             │ All cells processed?│
             └──────────┬──────────┘
                    Yes │
                        ▼
             ┌─────────────────────┐
             │ Print dp[N-1][M-1]  │
             └──────────┬──────────┘
                        │
                        ▼
                  ┌───────────┐
                  │   STOP    │
                  └───────────┘
```

---

# 7. C++ Program

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int minPathSum(vector<vector<int>>& grid, int n, int m)
{
    vector<vector<int>> dp(n, vector<int>(m));

    // Starting cell
    dp[0][0] = grid[0][0];

    // First row
    for (int j = 1; j < m; j++)
    {
        dp[0][j] = dp[0][j - 1] + grid[0][j];
    }

    // First column
    for (int i = 1; i < n; i++)
    {
        dp[i][0] = dp[i - 1][0] + grid[i][0];
    }

    // Remaining cells
    for (int i = 1; i < n; i++)
    {
        for (int j = 1; j < m; j++)
        {
            dp[i][j] = grid[i][j] +
                       min(dp[i - 1][j], dp[i][j - 1]);
        }
    }

    return dp[n - 1][m - 1];
}

int main()
{
    int n, m;

    cout << "Enter number of rows and columns: ";
    cin >> n >> m;

    vector<vector<int>> grid(n, vector<int>(m));

    cout << "Enter the grid:\n";

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            cin >> grid[i][j];
        }
    }

    int result = minPathSum(grid, n, m);

    cout << "Minimum Path Sum = " << result << endl;

    return 0;
}
```

---

# 8. Java Program

```java
import java.util.*;

public class MinimumPathSum {

    static int minPathSum(int[][] grid, int n, int m) {

        int[][] dp = new int[n][m];

        // Starting cell
        dp[0][0] = grid[0][0];

        // First row
        for (int j = 1; j < m; j++) {
            dp[0][j] = dp[0][j - 1] + grid[0][j];
        }

        // First column
        for (int i = 1; i < n; i++) {
            dp[i][0] = dp[i - 1][0] + grid[i][0];
        }

        // Remaining cells
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {

                dp[i][j] = grid[i][j]
                        + Math.min(dp[i - 1][j], dp[i][j - 1]);
            }
        }

        return dp[n - 1][m - 1];
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of rows and columns: ");
        int n = sc.nextInt();
        int m = sc.nextInt();

        int[][] grid = new int[n][m];

        System.out.println("Enter the grid:");

        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
                grid[i][j] = sc.nextInt();
            }
        }

        int result = minPathSum(grid, n, m);

        System.out.println("Minimum Path Sum = " + result);

        sc.close();
    }
}
```

---

# 9. Sample Input

```text
Enter number of rows and columns: 3 3
Enter the grid:
1 3 1
1 5 1
4 2 1
```

# 10. Sample Output

```text
Minimum Path Sum = 7
```

---

# 11. Dry Run

Given grid:

```text
1 3 1
1 5 1
4 2 1
```

### Step 1: Starting Cell

```text
dp[0][0] = 1
```

### Step 2: First Row

```text
dp[0][1] = 1 + 3 = 4
dp[0][2] = 4 + 1 = 5
```

First row becomes:

```text
1 4 5
```

### Step 3: First Column

```text
dp[1][0] = 1 + 1 = 2
dp[2][0] = 2 + 4 = 6
```

### Step 4: Remaining Cells

For `(1,1)`:

```text
grid[1][1] = 5

top  = 4
left = 2

dp[1][1] = 5 + min(4,2)
          = 5 + 2
          = 7
```

For `(1,2)`:

```text
grid[1][2] = 1

top  = 5
left = 7

dp[1][2] = 1 + min(5,7)
          = 6
```

For `(2,1)`:

```text
grid[2][1] = 2

top  = 7
left = 6

dp[2][1] = 2 + min(7,6)
          = 8
```

For `(2,2)`:

```text
grid[2][2] = 1

top  = 6
left = 8

dp[2][2] = 1 + min(6,8)
          = 7
```

### Final DP Table

```text
1  4  5
2  7  6
6  8  7
```

Therefore:

```text
Minimum Path Sum = dp[2][2] = 7
```

The corresponding minimum path is:

```text
1 → 3 → 1 → 1 → 1
```

---

# 12. Time Complexity

Every cell of the grid is processed exactly once.

For an `N × M` grid:

**Time Complexity:**

```text
O(N × M)
```

---

# 13. Space Complexity

The DP table contains `N × M` elements.

**Space Complexity:**

```text
O(N × M)
```

The space can be optimized to `O(M)` using a one-dimensional DP array, but the 2D version is easier to understand and trace.

---

# 14. Design Technique

**Design Technique: Dynamic Programming**

The problem has:

* **Overlapping subproblems**
* **Optimal substructure**

The minimum path to the current cell depends on the minimum paths to the cells immediately above and to the left.

---

# 15. Important Points

* The grid contains **non-negative integers**.
* Start position is always the **top-left cell**.
* Destination is the **bottom-right cell**.
* Movement is allowed only:

  * Right
  * Down
* Diagonal movement is not allowed.
* The value of both the starting and destination cells is included in the sum.
* Dynamic Programming avoids recalculating the same subproblems.

---

# 16. Conclusion

The **Minimum Path Sum** problem is efficiently solved using **Dynamic Programming**. For each cell, we calculate the minimum cost of reaching it from the top-left by considering the smaller of the paths from the top and left. The final cell contains the minimum possible path sum. The algorithm runs in **O(N × M)** time and uses **O(N × M)** space.
