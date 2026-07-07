# Experiment 5: Cycle Detection in an Undirected Graph Using DFS

## Aim

To write a program to determine whether a given **undirected graph** contains a **cycle** or not using **Depth First Search (DFS)**.

---

# Theory

A **cycle** in an undirected graph is a path that starts and ends at the same vertex without repeating any edge.

To detect a cycle, we use **Depth First Search (DFS)**.

### Logic

* Start DFS from any unvisited vertex.
* Mark the current vertex as visited.
* Visit all adjacent vertices.
* If an adjacent vertex is already visited **and is not the parent** of the current vertex, then a cycle exists.
* Repeat for all disconnected components.

### Example

```text
Vertices = 5
Edges = 5

0 -- 1
|    |
2 -- 3
     |
     4
```

Cycle:

```text
0 → 1 → 3 → 2 → 0
```

Hence,

```text
Graph contains a cycle.
```

---

# Algorithm

### Algorithm: Detect Cycle Using DFS

**Step 1:** Start

**Step 2:** Input the number of vertices `V`

**Step 3:** Input the number of edges `E`

**Step 4:** Create an adjacency list.

**Step 5:** Read all edges and add them to the adjacency list.

**Step 6:** Create a visited array initialized as false.

**Step 7:** For each unvisited vertex

* Perform DFS(vertex, parent)

**Step 8:** During DFS

* Mark current vertex as visited.
* Traverse all adjacent vertices.
* If adjacent vertex is not visited

  * Call DFS recursively.
* Else if adjacent vertex is visited and is not the parent

  * Cycle found.

**Step 9:** If cycle exists

* Display **"Graph contains a cycle."**

Else

* Display **"Graph does not contain a cycle."**

**Step 10:** Stop

---

# Flowchart

```text
 ┌─────────┐
 │ Start   │
 └────┬────┘
      │
      ▼
 ┌─────────────────┐
 │ Input V and E   │
 └────┬────────────┘
      │
      ▼
 ┌─────────────────┐
 │ Build Graph     │
 └────┬────────────┘
      │
      ▼
 ┌─────────────────┐
 │ Mark all        │
 │ vertices        │
 │ unvisited       │
 └────┬────────────┘
      │
      ▼
 ┌─────────────────┐
 │ DFS on each     │
 │ unvisited node  │
 └────┬────────────┘
      │
      ▼
 ┌────────────────────────┐
 │ Adjacent vertex visited│
 │ and not parent ?       │
 └───┬───────────────┬────┘
     │Yes            │No
     ▼               ▼
┌──────────────┐   Continue DFS
│ Cycle Found  │
└──────┬───────┘
       ▼
 ┌──────────────┐
 │ Print Result │
 └──────┬───────┘
        ▼
    ┌────────┐
    │ Stop   │
    └────────┘
```

---

# C++ Program

```cpp
#include <iostream>
#include <vector>
using namespace std;

bool dfs(int node, int parent, vector<vector<int>> &adj, vector<bool> &visited)
{
    visited[node] = true;

    for (int neighbor : adj[node])
    {
        if (!visited[neighbor])
        {
            if (dfs(neighbor, node, adj, visited))
                return true;
        }
        else if (neighbor != parent)
        {
            return true;
        }
    }

    return false;
}

int main()
{
    int V, E;

    cout << "Enter number of vertices: ";
    cin >> V;

    cout << "Enter number of edges: ";
    cin >> E;

    vector<vector<int>> adj(V);

    cout << "Enter edges (u v):\n";

    for (int i = 0; i < E; i++)
    {
        int u, v;
        cin >> u >> v;

        adj[u].push_back(v);
        adj[v].push_back(u);
    }

    vector<bool> visited(V, false);

    bool cycle = false;

    for (int i = 0; i < V; i++)
    {
        if (!visited[i])
        {
            if (dfs(i, -1, adj, visited))
            {
                cycle = true;
                break;
            }
        }
    }

    if (cycle)
        cout << "Graph contains a cycle.";
    else
        cout << "Graph does not contain a cycle.";

    return 0;
}
```

---

# Java Program

```java
import java.util.*;

public class CycleDetection {

    static boolean dfs(int node, int parent,
                       ArrayList<ArrayList<Integer>> adj,
                       boolean[] visited) {

        visited[node] = true;

        for (int neighbor : adj.get(node)) {

            if (!visited[neighbor]) {

                if (dfs(neighbor, node, adj, visited))
                    return true;

            } else if (neighbor != parent) {

                return true;
            }
        }

        return false;
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of vertices: ");
        int V = sc.nextInt();

        System.out.print("Enter number of edges: ");
        int E = sc.nextInt();

        ArrayList<ArrayList<Integer>> adj = new ArrayList<>();

        for (int i = 0; i < V; i++)
            adj.add(new ArrayList<>());

        System.out.println("Enter edges (u v):");

        for (int i = 0; i < E; i++) {

            int u = sc.nextInt();
            int v = sc.nextInt();

            adj.get(u).add(v);
            adj.get(v).add(u);
        }

        boolean[] visited = new boolean[V];

        boolean cycle = false;

        for (int i = 0; i < V; i++) {

            if (!visited[i]) {

                if (dfs(i, -1, adj, visited)) {

                    cycle = true;
                    break;
                }
            }
        }

        if (cycle)
            System.out.println("Graph contains a cycle.");
        else
            System.out.println("Graph does not contain a cycle.");

        sc.close();
    }
}
```

---

# Sample Output 1

```text
Enter number of vertices: 5
Enter number of edges: 5

Enter edges:
0 1
1 2
2 3
3 0
3 4

Graph contains a cycle.
```

---

# Sample Output 2

```text
Enter number of vertices: 5
Enter number of edges: 4

Enter edges:
0 1
1 2
2 3
3 4

Graph does not contain a cycle.
```

---

# Dry Run

### Input

```text
Vertices = 5
Edges:

0 1
1 2
2 3
3 0
3 4
```

### DFS Traversal

```text
Start from 0

0 → 1 → 2 → 3

At vertex 3

Adjacent vertex = 0

0 is already visited and is not the parent of 3.

Cycle Found.
```

---

# Time Complexity

* DFS Traversal: **O(V + E)**

Overall:

```text
O(V + E)
```

---

# Space Complexity

* Visited Array: **O(V)**
* Recursion Stack: **O(V)**
* Adjacency List: **O(V + E)**

Overall:

```text
O(V + E)
```

---

# Design Technique

**Depth First Search (DFS)**

DFS explores each vertex and edge exactly once. A cycle is detected when a visited vertex is encountered that is **not the parent** of the current vertex.

---

# Conclusion

The program successfully detects whether an **undirected graph** contains a cycle using **Depth First Search (DFS)**. The algorithm efficiently traverses the graph while keeping track of visited vertices and parent nodes, achieving a time complexity of **O(V + E)**, making it suitable for both connected and disconnected graphs.
