# Experiment 6: Critical Connections in a Network

## Aim

To write a program to find all **critical connections (bridges)** in an undirected network.

A critical connection is an edge which, if removed, causes some servers to become unreachable from other servers.

---

# Theory

A **critical connection**, also called a **bridge**, is an edge in an undirected graph whose removal increases the number of connected components of the graph.

### Example

Consider the graph:

```text
0 ----- 1
|       |
|       |
2 ----- 3 ----- 4
```

The connection:

```text
3 ----- 4
```

is a critical connection because removing it makes server `4` unreachable from servers `0, 1, 2, 3`.

However, the edges forming the cycle:

```text
0 -- 1
|    |
2 -- 3
```

are not critical connections because there is an alternate path between the servers.

---

# Approach

The problem can be solved efficiently using **Tarjan's Algorithm**, which is based on **Depth First Search (DFS)**.

For every vertex, maintain:

### 1. Discovery Time

`disc[u]`

The time at which vertex `u` is first visited during DFS.

### 2. Lowest Discovery Time

`low[u]`

The earliest discovered vertex that can be reached from `u` or its DFS subtree using:

* Tree edges
* At most one back edge

For an edge `(u, v)` where `v` is a DFS child of `u`:

```text
low[v] > disc[u]
```

then `(u, v)` is a **critical connection**.

### Why?

If `low[v] > disc[u]`, there is no alternate path from the subtree rooted at `v` back to `u` or any ancestor of `u`.

Therefore, removing `(u, v)` disconnects the graph.

---

# Algorithm

### Algorithm: Find Critical Connections

**Step 1:** Start.

**Step 2:** Input the number of servers `n`.

**Step 3:** Input the number of connections.

**Step 4:** Create an adjacency list for the graph.

**Step 5:** Initialize:

* `disc[] = -1`
* `low[] = -1`
* `time = 0`

**Step 6:** Perform DFS for every unvisited vertex.

**Step 7:** During DFS:

* Mark the current vertex `u` as visited.
* Set:

```text
disc[u] = low[u] = time
```

* Increment `time`.

**Step 8:** For every adjacent vertex `v`:

### Case 1: `v` is the parent

Ignore this edge.

### Case 2: `v` is unvisited

Perform DFS on `v`.

After returning:

```text
low[u] = min(low[u], low[v])
```

Then check:

```text
if low[v] > disc[u]
```

If true, `(u, v)` is a critical connection.

### Case 3: `v` is already visited

This is a back edge.

Update:

```text
low[u] = min(low[u], disc[v])
```

**Step 9:** Continue until all vertices have been visited.

**Step 10:** Display all critical connections.

**Step 11:** Stop.

---

# Flowchart

```text
                         ┌───────────┐
                         │   Start   │
                         └─────┬─────┘
                               │
                               ▼
                    ┌────────────────────┐
                    │ Input n and edges  │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │ Build Adjacency    │
                    │ List               │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │ Initialize disc[]  │
                    │ and low[] = -1     │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │ Start DFS(u)       │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │ Set disc[u] and    │
                    │ low[u]             │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │ Check adjacent v   │
                    └─────────┬──────────┘
                              │
                              ▼
                    ┌────────────────────┐
                    │ v == parent ?      │
                    └──────┬───────┬─────┘
                           │Yes    │No
                           ▼       ▼
                        Ignore  ┌──────────────┐
                                │ v visited ?  │
                                └───┬──────┬───┘
                                    │No    │Yes
                                    ▼      ▼
                              ┌──────────┐ ┌───────────┐
                              │ DFS(v)   │ │ Update    │
                              │          │ │ low[u]    │
                              └────┬─────┘ └─────┬─────┘
                                   │             │
                                   ▼             │
                         ┌───────────────────┐   │
                         │ low[v] > disc[u]? │   │
                         └─────┬────────┬────┘   │
                               │Yes     │No      │
                               ▼        │        │
                         ┌──────────┐   │        │
                         │ Critical │   │        │
                         │ Edge     │   │        │
                         └────┬─────┘   │        │
                              └──────────┴────────┘
                                         │
                                         ▼
                              ┌────────────────────┐
                              │ More vertices/edges│
                              │ to process?        │
                              └──────┬────────┬────┘
                                     │Yes     │No
                                     │        ▼
                                     │   ┌─────────────┐
                                     │   │ Print all   │
                                     │   │ bridges     │
                                     │   └──────┬──────┘
                                     │          │
                                     └──────────┤
                                                ▼
                                           ┌─────────┐
                                           │  Stop   │
                                           └─────────┘
```

---

# C++ Program

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

class Solution {
private:
    int timer;
    vector<int> disc;
    vector<int> low;
    vector<vector<int>> adj;
    vector<vector<int>> bridges;

    void dfs(int u, int parent) {
        disc[u] = low[u] = timer++;

        for (int v : adj[u]) {

            // Ignore the edge back to the parent
            if (v == parent)
                continue;

            // If v is not visited
            if (disc[v] == -1) {

                dfs(v, u);

                // Update low value
                low[u] = min(low[u], low[v]);

                // Bridge condition
                if (low[v] > disc[u]) {
                    bridges.push_back({u, v});
                }
            }
            else {
                // Back edge
                low[u] = min(low[u], disc[v]);
            }
        }
    }

public:
    vector<vector<int>> criticalConnections(
        int n,
        vector<vector<int>>& connections) {

        adj.resize(n);
        disc.assign(n, -1);
        low.assign(n, -1);

        // Build adjacency list
        for (auto &edge : connections) {
            int u = edge[0];
            int v = edge[1];

            adj[u].push_back(v);
            adj[v].push_back(u);
        }

        timer = 0;

        // Handle disconnected graphs
        for (int i = 0; i < n; i++) {
            if (disc[i] == -1) {
                dfs(i, -1);
            }
        }

        return bridges;
    }
};

int main() {
    int n, e;

    cout << "Enter number of servers: ";
    cin >> n;

    cout << "Enter number of connections: ";
    cin >> e;

    vector<vector<int>> connections(e, vector<int>(2));

    cout << "Enter connections (u v):\n";

    for (int i = 0; i < e; i++) {
        cin >> connections[i][0] >> connections[i][1];
    }

    Solution obj;

    vector<vector<int>> result =
        obj.criticalConnections(n, connections);

    cout << "\nCritical Connections:\n";

    if (result.empty()) {
        cout << "No critical connections found.\n";
    }
    else {
        for (auto &edge : result) {
            cout << edge[0] << " - " << edge[1] << endl;
        }
    }

    return 0;
}
```

---

# Java Program

```java
import java.util.*;

public class CriticalConnections {

    static int timer;
    static List<List<Integer>> adj;
    static int[] disc;
    static int[] low;
    static List<List<Integer>> bridges;

    static void dfs(int u, int parent) {

        disc[u] = low[u] = timer++;

        for (int v : adj.get(u)) {

            // Ignore the edge back to the parent
            if (v == parent)
                continue;

            // If v is not visited
            if (disc[v] == -1) {

                dfs(v, u);

                // Update low value
                low[u] = Math.min(low[u], low[v]);

                // Bridge condition
                if (low[v] > disc[u]) {
                    bridges.add(Arrays.asList(u, v));
                }
            }
            else {
                // Back edge
                low[u] = Math.min(low[u], disc[v]);
            }
        }
    }

    static List<List<Integer>> criticalConnections(
            int n,
            List<List<Integer>> connections) {

        adj = new ArrayList<>();

        for (int i = 0; i < n; i++) {
            adj.add(new ArrayList<>());
        }

        // Build adjacency list
        for (List<Integer> edge : connections) {

            int u = edge.get(0);
            int v = edge.get(1);

            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        disc = new int[n];
        low = new int[n];

        Arrays.fill(disc, -1);
        Arrays.fill(low, -1);

        bridges = new ArrayList<>();

        timer = 0;

        // Handle disconnected graphs
        for (int i = 0; i < n; i++) {

            if (disc[i] == -1) {
                dfs(i, -1);
            }
        }

        return bridges;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of servers: ");
        int n = sc.nextInt();

        System.out.print("Enter number of connections: ");
        int e = sc.nextInt();

        List<List<Integer>> connections = new ArrayList<>();

        System.out.println("Enter connections (u v):");

        for (int i = 0; i < e; i++) {

            int u = sc.nextInt();
            int v = sc.nextInt();

            connections.add(Arrays.asList(u, v));
        }

        List<List<Integer>> result =
                criticalConnections(n, connections);

        System.out.println("\nCritical Connections:");

        if (result.isEmpty()) {
            System.out.println("No critical connections found.");
        }
        else {
            for (List<Integer> edge : result) {
                System.out.println(
                    edge.get(0) + " - " + edge.get(1)
                );
            }
        }

        sc.close();
    }
}
```

---

# Sample Output 1

### Input

```text
Enter number of servers: 5
Enter number of connections: 5
Enter connections (u v):
0 1
1 2
2 0
1 3
3 4
```

### Output

```text
Critical Connections:
3 - 4
1 - 3
```

The connections `3-4` and `1-3` are critical because removing either one disconnects part of the network.

---

# Sample Output 2

### Input

```text
Enter number of servers: 4
Enter number of connections: 4
Enter connections (u v):
0 1
1 2
2 3
3 0
```

### Output

```text
Critical Connections:
No critical connections found.
```

Here, all vertices form a cycle, so removing any single connection still leaves another path between the servers.

---

# Dry Run

Consider:

```text
Servers = 5
Connections =
0 1
1 2
2 0
1 3
3 4
```

Graph:

```text
       0
      / \
     1---2
     |
     3
     |
     4
```

DFS traversal:

```text
0 → 1 → 2
    |
    3
    |
    4
```

For edge `(3,4)`:

```text
low[4] > disc[3]
```

Therefore:

```text
3 - 4 = Critical Connection
```

For edge `(1,3)`:

```text
low[3] > disc[1]
```

Therefore:

```text
1 - 3 = Critical Connection
```

The edges `0-1`, `1-2`, and `2-0` are part of a cycle, so they are not critical.

---

# Important Condition

The key condition used by Tarjan's algorithm is:

```text
low[v] > disc[u]
```

where `u` is the parent of `v` in the DFS tree.

If this condition is true, then the edge:

```text
u ─── v
```

is a **critical connection (bridge)**.

---

# Time Complexity

Building the adjacency list takes:

**O(V + E)**

DFS traversal takes:

**O(V + E)**

Therefore, the overall time complexity is:

**O(V + E)**

---

# Space Complexity

The algorithm uses:

* Adjacency list: **O(V + E)**
* `disc[]`: **O(V)**
* `low[]`: **O(V)**
* DFS recursion stack: **O(V)**
* Result array: **O(E)** in the worst case

Overall:

**O(V + E)**

---

# Design Technique

**Depth First Search (DFS) + Tarjan's Algorithm**

Tarjan's algorithm uses discovery time and low-link values to identify bridges in a single DFS traversal.

---

# Conclusion

The program successfully finds all **critical connections (bridges)** in an undirected network. Tarjan's algorithm efficiently identifies whether an edge is essential for maintaining network connectivity. The solution works in **O(V + E)** time, making it suitable for large networks.
