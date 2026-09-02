# Experiment 8: Rotten Oranges

## 1. Aim

To determine the **minimum time required for all fresh oranges to become rotten** in an `N × M` grid using **Breadth First Search (BFS)**.

---

## 2. Problem Statement

Given a grid of dimensions `N × M`, each cell contains one of the following values:

* `0` → Empty cell
* `1` → Fresh orange
* `2` → Rotten orange

A rotten orange can rot an adjacent fresh orange in **one unit of time**.

Only the following four directions are considered:

```text
        Up
        (i-1,j)

Left   (i,j)   Right
(i,j-1)        (i,j+1)

        Down
        (i+1,j)
```

Find the **minimum time required to rot all fresh oranges**.

If some fresh oranges can never become rotten, return `-1`.

---

# 3. Theory

This problem is a classic application of **Breadth First Search (BFS)**.

Since all initially rotten oranges spread simultaneously, we process the grid **level by level**.

### Why BFS?

Suppose:

```text
2 1 1 1
```

At time `0`:

```text
2 1 1 1
```

At time `1`:

```text
2 2 1 1
```

At time `2`:

```text
2 2 2 1
```

At time `3`:

```text
2 2 2 2
```

Each BFS level represents **one unit of time**.

Therefore, BFS gives the minimum time required.

---

# 4. Approach / Logic

The algorithm uses a **queue**.

### Step 1: Store all rotten oranges

Traverse the complete grid and insert the position of every rotten orange (`2`) into the queue.

At the same time, count the number of fresh oranges (`1`).

### Step 2: Process the queue

For every BFS level:

* Remove all rotten oranges currently in the queue.
* Check their four neighboring cells.
* If a neighboring cell contains a fresh orange:

  * Make it rotten.
  * Decrease the fresh-orange count.
  * Insert it into the queue.

### Step 3: Increase time

After processing one complete BFS level, increase the time by `1`.

### Step 4: Check remaining fresh oranges

After BFS:

* If `fresh == 0`, all oranges have become rotten.
* Otherwise, some fresh oranges cannot be reached, so return `-1`.

---

# 5. Algorithm

1. Start.
2. Read `N` and `M`.
3. Read the grid.
4. Initialize an empty queue.
5. Traverse the grid:

   * If cell is `1`, increment `fresh`.
   * If cell is `2`, insert its coordinates into the queue.
6. Set `time = 0`.
7. While the queue is not empty:

   * Store the current queue size.
   * Process all oranges at the current level.
   * For each orange, check its four neighbors.
   * If a neighbor contains a fresh orange:

     * Change it to `2`.
     * Decrease `fresh`.
     * Add its position to the queue.
   * If at least one fresh orange was rotten during this level, increment `time`.
8. If `fresh > 0`, print `-1`.
9. Otherwise, print `time`.
10. Stop.

---

# 6. Flowchart

```text
                 ┌─────────────┐
                 │    START    │
                 └──────┬──────┘
                        │
                        ▼
              ┌──────────────────┐
              │ Read N, M and    │
              │ the grid         │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ Find all rotten  │
              │ oranges and put  │
              │ them in queue    │
              │ Count fresh      │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ time = 0         │
              └────────┬─────────┘
                       │
                       ▼
               ┌───────────────┐
               │ Queue empty?  │
               └───────┬───────┘
                    No │    │ Yes
                       │    │
                       ▼    ▼
             ┌──────────────┐
             │ Process all  │
             │ oranges at   │
             │ current level│
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │ Check 4      │
             │ neighbors    │
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │ Fresh orange?│
             └──────┬───────┘
                  Yes│
                     ▼
             ┌──────────────┐
             │ Rot orange   │
             │ fresh--      │
             │ Add to queue │
             └──────┬───────┘
                    │
                    ▼
             ┌──────────────┐
             │ Increase time│
             └──────┬───────┘
                    │
                    └───────────────►
                             Queue check
                                  │
                                  ▼
                         ┌────────────────┐
                         │ fresh > 0 ?    │
                         └───────┬────────┘
                              Yes│    │No
                                 │    │
                                 ▼    ▼
                          ┌──────────┐ ┌──────────┐
                          │ Print -1 │ │Print time│
                          └────┬─────┘ └────┬─────┘
                               │            │
                               └──────┬─────┘
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
#include <queue>
using namespace std;

int rottenOranges(vector<vector<int>>& grid, int n, int m)
{
    queue<pair<int, int>> q;
    int fresh = 0;

    // Find fresh and rotten oranges
    for (int i = 0; i < n; i++)
    {
        for (int j = 0; j < m; j++)
        {
            if (grid[i][j] == 1)
            {
                fresh++;
            }
            else if (grid[i][j] == 2)
            {
                q.push({i, j});
            }
        }
    }

    int time = 0;

    int dr[] = {-1, 1, 0, 0};
    int dc[] = {0, 0, -1, 1};

    // BFS
    while (!q.empty() && fresh > 0)
    {
        int size = q.size();

        for (int i = 0; i < size; i++)
        {
            int row = q.front().first;
            int col = q.front().second;
            q.pop();

            for (int k = 0; k < 4; k++)
            {
                int newRow = row + dr[k];
                int newCol = col + dc[k];

                if (newRow >= 0 && newRow < n &&
                    newCol >= 0 && newCol < m &&
                    grid[newRow][newCol] == 1)
                {
                    grid[newRow][newCol] = 2;
                    fresh--;

                    q.push({newRow, newCol});
                }
            }
        }

        time++;
    }

    // If fresh oranges remain, they cannot be rotten
    if (fresh > 0)
        return -1;

    return time;
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

    int result = rottenOranges(grid, n, m);

    cout << "Minimum time required = " << result << endl;

    return 0;
}
```

---

# 8. Java Program

```java
import java.util.*;

public class RottenOranges {

    static class Cell {
        int row;
        int col;

        Cell(int row, int col) {
            this.row = row;
            this.col = col;
        }
    }

    static int rottenOranges(int[][] grid, int n, int m) {

        Queue<Cell> queue = new LinkedList<>();

        int fresh = 0;

        // Find fresh and rotten oranges
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {

                if (grid[i][j] == 1) {
                    fresh++;
                }
                else if (grid[i][j] == 2) {
                    queue.add(new Cell(i, j));
                }
            }
        }

        int time = 0;

        int[] dr = {-1, 1, 0, 0};
        int[] dc = {0, 0, -1, 1};

        // BFS
        while (!queue.isEmpty() && fresh > 0) {

            int size = queue.size();

            for (int i = 0; i < size; i++) {

                Cell current = queue.poll();

                for (int k = 0; k < 4; k++) {

                    int newRow = current.row + dr[k];
                    int newCol = current.col + dc[k];

                    if (newRow >= 0 && newRow < n &&
                        newCol >= 0 && newCol < m &&
                        grid[newRow][newCol] == 1) {

                        grid[newRow][newCol] = 2;
                        fresh--;

                        queue.add(new Cell(newRow, newCol));
                    }
                }
            }

            time++;
        }

        // Fresh oranges remaining means impossible
        if (fresh > 0) {
            return -1;
        }

        return time;
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

        int result = rottenOranges(grid, n, m);

        System.out.println("Minimum time required = " + result);

        sc.close();
    }
}
```

---

# 9. Sample Input

```text
Enter number of rows and columns: 3 3
Enter the grid:
2 1 1
1 1 0
0 1 1
```

# 10. Sample Output

```text
Minimum time required = 4
```

---

# 11. Dry Run

Consider:

```text
2 1 1
1 1 0
0 1 1
```

Initially:

```text
Time = 0

2 1 1
1 1 0
0 1 1
```

There is one rotten orange at `(0,0)`.

### After 1 unit

The rotten orange infects `(0,1)` and `(1,0)`.

```text
2 2 1
2 1 0
0 1 1
```

```text
Time = 1
```

### After 2 units

The newly rotten oranges infect `(0,2)` and `(1,1)`.

```text
2 2 2
2 2 0
0 1 1
```

```text
Time = 2
```

### After 3 units

`(1,1)` infects `(2,1)`.

```text
2 2 2
2 2 0
0 2 1
```

```text
Time = 3
```

### After 4 units

`(2,1)` infects `(2,2)`.

```text
2 2 2
2 2 0
0 2 2
```

No fresh oranges remain.

Therefore:

```text
Minimum time required = 4
```

---

# 12. Time Complexity

Each cell is inserted into and removed from the queue at most once.

For an `N × M` grid:

**Time Complexity:**

```text
O(N × M)
```

---

# 13. Space Complexity

In the worst case, the queue can contain `N × M` cells.

**Space Complexity:**

```text
O(N × M)
```

No separate visited matrix is required because we change a fresh orange from `1` to `2` when it is visited.

---

# 14. Design Technique

**Design Technique: Breadth First Search (BFS)**

BFS is used because all rotten oranges spread simultaneously, and each BFS level represents exactly **one unit of time**.

This is also known as **Multi-Source BFS**, because the BFS starts from multiple rotten oranges simultaneously.

---

# 15. Important Edge Cases

### Case 1: No fresh oranges

```text
2 2
2 2
```

Output:

```text
0
```

No time is required because there are no fresh oranges.

### Case 2: Fresh oranges are unreachable

```text
2 0 1
0 0 0
```

Output:

```text
-1
```

The fresh orange can never be reached.

### Case 3: No rotten oranges

```text
1 1 1
1 1 1
```

Output:

```text
-1
```

There is no rotten orange to start the process.

### Case 4: Empty grid

```text
0 0 0
0 0 0
```

Output:

```text
0
```

---

# 16. Conclusion

The minimum time required to rot all oranges can be efficiently calculated using **Multi-Source BFS**. All initially rotten oranges are inserted into the queue, and the infection spreads level by level to adjacent fresh oranges. Each BFS level represents one unit of time, giving an overall time complexity of **O(N × M)**.
