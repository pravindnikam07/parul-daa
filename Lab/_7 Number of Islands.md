# Experiment 7: Number of Islands

## 1. Aim

To find the number of islands in a given `N × M` grid consisting of `0`s (Water) and `1`s (Land).

---

## 2. Problem Statement

Given a grid of size `N × M`, where:

- `0` represents **Water**
- `1` represents **Land**

An **island** is a group of connected land cells (`1`s). Cells are considered connected if they are adjacent **horizontally or vertically**.

Find the total number of islands in the grid.

### Example

Input:

```text
1 1 0 0 0
0 1 0 0 1
1 0 0 1 1
0 0 0 0 0
1 0 1 1 0
```

Output:

```text
4
```

The four islands are:

```text
Island 1: (0,0), (0,1), (1,1)

Island 2: (1,4), (2,4), (2,3)

Island 3: (2,0)

Island 4: (4,0)

Island 5: (4,2), (4,3)
```

**Correction:** The above grid actually contains **5 islands**, so the correct output is:

```text
5
```

---

# 3. Theory

The **Number of Islands** problem is a standard graph traversal problem.

Each land cell (`1`) can be considered as a vertex, and adjacent land cells form connections.

Whenever we encounter an unvisited land cell:

1. We have found a **new island**.
2. Increase the island count by `1`.
3. Traverse all connected land cells using **DFS (Depth First Search)** or **BFS (Breadth First Search)**.
4. Mark them as visited so that the same island is not counted again.

Here, we use **DFS**.

### Four Possible Directions

From a cell `(i, j)`, we can move to:

```text
        Up
        (-1,0)

Left    (i,j)    Right
(-1,0)           (0,1)

        Down
        (1,0)
```

More precisely:

```text
Up     → (i-1, j)
Down   → (i+1, j)
Left   → (i, j-1)
Right  → (i, j+1)
```

Diagonal cells are **not considered connected**.

---

# 4. Approach / Logic

We traverse every cell of the grid.

### Step 1

If the current cell is `0`, it is water, so ignore it.

### Step 2

If the current cell is `1` and has not been visited, a new island has been found.

### Step 3

Increment the island count.

### Step 4

Perform DFS from that cell and visit every connected land cell.

### Step 5

Mark visited cells so they are not processed again.

### Step 6

After scanning the entire grid, the island count is the answer.

---

# 5. Algorithm

### DFS Algorithm

1. Start.
2. Read `N` and `M`.
3. Read the `N × M` grid.
4. Create a `visited[N][M]` array initialized to `false`.
5. Set `count = 0`.
6. Traverse every cell `(i, j)`:

   - If `grid[i][j] == 1` and it is not visited:

     - Increment `count`.
     - Call DFS on `(i, j)`.

7. In DFS:

   - Mark the current cell as visited.
   - Check its four neighboring cells.
   - If a neighbor is inside the grid, contains `1`, and is not visited, recursively call DFS.

8. Print `count`.
9. Stop.

---

# 6. Flowchart

```text
             ┌─────────────┐
             │    START    │
             └──────┬──────┘
                    │
                    ▼
          ┌───────────────────┐
          │ Read N, M and     │
          │ the grid          │
          └────────┬──────────┘
                   │
                   ▼
          ┌───────────────────┐
          │ count = 0         │
          │ visited = false   │
          └────────┬──────────┘
                   │
                   ▼
          ┌───────────────────┐
          │ Traverse every    │
          │ cell (i,j)        │
          └────────┬──────────┘
                   │
                   ▼
            ┌──────────────┐
            │ grid[i][j]=1 │
            │ and unvisited│
            │      ?       │
            └──────┬───────┘
                 Yes│       │No
                    │       │
                    ▼       │
             ┌────────────┐ │
             │ count++    │ │
             └─────┬──────┘ │
                   │         │
                   ▼         │
             ┌────────────┐  │
             │ DFS(i, j)  │  │
             └─────┬──────┘  │
                   │         │
                   └────┬────┘
                        │
                        ▼
                 ┌─────────────┐
                 │ More cells? │
                 └──────┬──────┘
                      Yes│  │No
                         │  │
                         └──┘
                            │
                            ▼
                    ┌────────────┐
                    │ Print count│
                    └─────┬──────┘
                          │
                          ▼
                    ┌────────────┐
                    │    STOP    │
                    └────────────┘
```

---

# 7. C++ Program

```cpp
#include <iostream>
#include <vector>
using namespace std;

void dfs(int row, int col, vector<vector<int>>& grid,
         vector<vector<bool>>& visited, int n, int m)
{
    visited[row][col] = true;

    int dr[] = {-1, 1, 0, 0};
    int dc[] = {0, 0, -1, 1};

    for (int k = 0; k < 4; k++)
    {
        int newRow = row + dr[k];
        int newCol = col + dc[k];

        if (newRow >= 0 && newRow < n &&
            newCol >= 0 && newCol < m &&
            grid[newRow][newCol] == 1 &&
            !visited[newRow][newCol])
        {
            dfs(newRow, newCol, grid, visited, n, m);
        }
    }
}

int countIslands(vector<vector<int>>& grid, int n, int m)
{
    vector<vector<bool>> visited(n, vector<bool>(m, false));

    int count = 0;

    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            if (grid[i][j] == 1 && !visited[i][j])
            {
                count++;

                dfs(i, j, grid, visited, n, m);
            }
        }
    }

    return count;
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

    int result = countIslands(grid, n, m);

    cout << "Number of islands = " << result << endl;

    return 0;
}
```

---

# 8. Java Program

```java
import java.util.*;

public class NumberOfIslands {

    static void dfs(int row, int col, int[][] grid,
                    boolean[][] visited, int n, int m) {

        visited[row][col] = true;

        int[] dr = {-1, 1, 0, 0};
        int[] dc = {0, 0, -1, 1};

        for (int k = 0; k < 4; k++) {

            int newRow = row + dr[k];
            int newCol = col + dc[k];

            if (newRow >= 0 && newRow < n &&
                newCol >= 0 && newCol < m &&
                grid[newRow][newCol] == 1 &&
                !visited[newRow][newCol]) {

                dfs(newRow, newCol, grid, visited, n, m);
            }
        }
    }

    static int countIslands(int[][] grid, int n, int m) {

        boolean[][] visited = new boolean[n][m];

        int count = 0;

        for (int i = 0; i < n; i++) {

            for (int j = 0; j < m; j++) {

                if (grid[i][j] == 1 && !visited[i][j]) {

                    count++;

                    dfs(i, j, grid, visited, n, m);
                }
            }
        }

        return count;
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

        int result = countIslands(grid, n, m);

        System.out.println("Number of islands = " + result);

        sc.close();
    }
}
```

---

# 9. Sample Input

```text
Enter number of rows and columns: 5 5
Enter the grid:
1 1 0 0 0
0 1 0 0 1
1 0 0 1 1
0 0 0 0 0
1 0 1 1 0
```

# 10. Sample Output

```text
Number of islands = 5
```

---

# 11. Dry Run

Consider:

```text
1 1 0 0 0
0 1 0 0 1
1 0 0 1 1
0 0 0 0 0
1 0 1 1 0
```

### First Island

Starting at `(0,0)`:

```text
(0,0) → (0,1) → (1,1)
```

All three cells belong to one island.

```text
count = 1
```

### Second Island

Starting at `(1,4)`:

```text
(1,4) → (2,4) → (2,3)
```

```text
count = 2
```

### Third Island

Cell `(2,0)` is isolated.

```text
count = 3
```

### Fourth Island

Cell `(4,0)` is isolated.

```text
count = 4
```

### Fifth Island

Cells `(4,2)` and `(4,3)` are connected.

```text
count = 5
```

Therefore:

```text
Number of islands = 5
```

---

# 12. Time Complexity

There are `N × M` cells.

Each cell is visited at most once.

**Time Complexity:**

```text
O(N × M)
```

---

# 13. Space Complexity

The `visited` matrix requires:

```text
O(N × M)
```

DFS recursion can also use up to `O(N × M)` stack space in the worst case.

Therefore:

**Space Complexity:**

```text
O(N × M)
```

---

# 14. Design Technique

**Design Technique: Graph Traversal – Depth First Search (DFS)**

The grid can be treated as a graph where each land cell is connected to its adjacent land cells. DFS is used to explore an entire island before searching for the next island.

---

# 15. Important Points

- `1` represents **land**.
- `0` represents **water**.
- Only **up, down, left, and right** connections are considered.
- Diagonal connections are **not considered**.
- Every new unvisited land cell starts a new DFS and represents one new island.
- The algorithm also works correctly when the grid contains multiple disconnected islands.

---

# 16. Conclusion

The number of islands in an `N × M` grid can be efficiently determined using **DFS graph traversal**. Each unvisited land cell starts a DFS that marks all connected land cells as visited. Since every cell is processed at most once, the algorithm runs in **O(N × M)** time.
