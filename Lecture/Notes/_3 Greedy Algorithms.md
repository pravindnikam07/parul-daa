# Unit 3 – Greedy Algorithms

# 1. Introduction to Greedy Algorithms

## Definition

A **Greedy Algorithm** is an algorithmic paradigm that builds a solution step by step by always choosing the option that appears best at the current moment.

The choice made is **locally optimal**, with the hope that these local optimum choices lead to a **globally optimal solution**.

Unlike Dynamic Programming, Greedy Algorithms do not reconsider previously selected choices.

---

## Basic Idea

At each step:

1. Choose the best available option.
2. Add it to the solution.
3. Never change the decision.
4. Continue until the solution is complete.

---

## Example

Suppose we want to make ₹67 using coins:

```text
Coins Available: 50, 10, 5, 2, 1
```

Greedy Selection:

```text
67 = 50 + 10 + 5 + 2
```

The algorithm always chooses the largest possible coin.

---

# Characteristics of Greedy Algorithms

* Makes decisions one step at a time.
* Decisions are irreversible.
* Usually simple and efficient.
* Does not always guarantee optimal solution.
* Works only when the problem satisfies specific properties.

---

# Advantages

* Easy to implement
* Fast execution
* Less memory usage
* Efficient for optimization problems

---

# Disadvantages

* May not always produce optimal solutions
* Difficult to prove correctness
* Works only for specific problem classes

---

# 2. Elements of Greedy Strategy

For a problem to be solved using a Greedy approach, it must satisfy the following properties.

---

## 1. Greedy Choice Property

A globally optimal solution can be obtained by making a locally optimal choice.

### Example

Activity Selection Problem

Choosing the activity that finishes earliest leaves maximum time for remaining activities.

---

## 2. Optimal Substructure

An optimal solution contains optimal solutions to its subproblems.

### Example

Shortest path problems.

If the shortest path from A to C passes through B, then the path from B to C must also be shortest.

---

## General Structure of Greedy Algorithm

```text
GreedyAlgorithm()

Initialize solution

while solution not complete

    Select best candidate

    if candidate feasible

        Add candidate to solution

Return solution
```

---

# 3. Minimum Spanning Tree (MST)

## Definition

A **Spanning Tree** of a graph is a tree that:

* Includes all vertices.
* Contains no cycles.
* Has exactly (V − 1) edges.

A **Minimum Spanning Tree (MST)** is a spanning tree whose total edge weight is minimum.

---

## Example

Graph:

```text
      A
    /   \
   4     2
  /       \
 B----1----C
```

Possible spanning trees:

Tree 1:

```text
AB + AC = 6
```

Tree 2:

```text
AB + BC = 5
```

Tree 3:

```text
AC + BC = 3
```

Minimum Spanning Tree:

```text
AC + BC = 3
```

---

# Properties of MST

For a graph with V vertices:

```text
Edges in MST = V - 1
```

MST contains:

* No cycles
* Minimum possible cost
* All vertices connected

---

# 4. Kruskal's Algorithm

## Idea

Select edges in increasing order of weight while avoiding cycles.

---

## Steps

1. Sort edges by weight.
2. Select smallest edge.
3. Check cycle formation.
4. If no cycle, include edge.
5. Repeat until V−1 edges selected.

---

## Algorithm

```text
Kruskal(G)

Sort edges by increasing weight

for each edge

    if adding edge does not form cycle

        add edge to MST

Return MST
```

---

## Example

| Edge | Weight |
| ---- | ------ |
| BC   | 1      |
| AC   | 2      |
| AB   | 4      |

Sorted order:

```text
BC → AC → AB
```

Choose:

```text
BC, AC
```

MST Weight:

```text
1 + 2 = 3
```

---

## C++ Program (Kruskal)

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

struct Edge
{
    int u,v,w;
};

bool compare(Edge a, Edge b)
{
    return a.w < b.w;
}

int parent[100];

int find(int i)
{
    while(parent[i] != i)
        i = parent[i];
    return i;
}

void unionSet(int a,int b)
{
    parent[a] = b;
}

int main()
{
    int n = 4;
    int e = 5;

    Edge edges[] =
    {
        {0,1,10},
        {0,2,6},
        {0,3,5},
        {1,3,15},
        {2,3,4}
    };

    sort(edges, edges+e, compare);

    for(int i=0;i<n;i++)
        parent[i]=i;

    cout<<"Edges in MST:\n";

    for(int i=0;i<e;i++)
    {
        int a=find(edges[i].u);
        int b=find(edges[i].v);

        if(a!=b)
        {
            cout<<edges[i].u<<" - "
                <<edges[i].v
                <<" : "<<edges[i].w<<endl;

            unionSet(a,b);
        }
    }
}
```

---

## Complexity

Sorting edges:

[
O(E \log E)
]

Using Union-Find:

[
O(E \log V)
]

Overall:

[
O(E \log V)
]

---

# 5. Prim's Algorithm

## Idea

Start from any vertex and repeatedly add the minimum weight edge connecting the MST to a new vertex.

---

## Steps

1. Start with one vertex.
2. Select smallest edge connected to MST.
3. Add new vertex.
4. Repeat until all vertices included.

---

## Algorithm

```text
Prim(G)

Select start vertex

while MST not complete

    Choose minimum edge

    Add edge to MST
```

---

## C++ Program

```cpp
#include <iostream>
#include <climits>
using namespace std;

#define V 5

int minKey(int key[], bool mstSet[])
{
    int min = INT_MAX, minIndex;

    for(int v=0;v<V;v++)
    {
        if(!mstSet[v] && key[v] < min)
        {
            min = key[v];
            minIndex = v;
        }
    }
    return minIndex;
}

void primMST(int graph[V][V])
{
    int parent[V];
    int key[V];
    bool mstSet[V];

    for(int i=0;i<V;i++)
    {
        key[i]=INT_MAX;
        mstSet[i]=false;
    }

    key[0]=0;
    parent[0]=-1;

    for(int count=0;count<V-1;count++)
    {
        int u=minKey(key,mstSet);
        mstSet[u]=true;

        for(int v=0;v<V;v++)
        {
            if(graph[u][v] &&
               !mstSet[v] &&
               graph[u][v] < key[v])
            {
                parent[v]=u;
                key[v]=graph[u][v];
            }
        }
    }

    cout<<"Edge Weight\n";

    for(int i=1;i<V;i++)
        cout<<parent[i]
            <<" - "<<i
            <<" = "<<graph[i][parent[i]]
            <<endl;
}
```

---

## Complexity

Adjacency Matrix:

[
O(V^2)
]

Priority Queue Version:

[
O(E \log V)
]

---

# Difference Between Kruskal and Prim

| Kruskal                | Prim                  |
| ---------------------- | --------------------- |
| Edge based             | Vertex based          |
| Sorts edges            | Grows MST             |
| Uses Union-Find        | Uses Priority Queue   |
| Good for sparse graphs | Good for dense graphs |

---

# 6. Dijkstra's Algorithm

## Definition

Dijkstra's Algorithm finds the shortest path from a source vertex to all other vertices in a weighted graph.

Condition:

```text
Edge weights must be non-negative
```

---

## Working

1. Start from source vertex.
2. Assign distance 0 to source.
3. Assign infinity to others.
4. Select nearest unvisited vertex.
5. Update neighboring distances.
6. Repeat.

---

## Example

```text
A --4-- B
|       |
1       2
|       |
C --5-- D
```

Source:

```text
A
```

Shortest paths calculated incrementally.

---

## Complexity

Using Priority Queue:

[
O(E \log V)
]

---

## Applications

* GPS Navigation
* Routing Protocols
* Network Optimization

---

# 7. Fractional Knapsack Problem

## Problem Statement

A bag has capacity W.

Each item has:

* Weight
* Profit

Fractions of items are allowed.

Goal:

```text
Maximize Profit
```

---

## Greedy Strategy

Select item with highest:

[
\frac{Profit}{Weight}
]

ratio first.

---

## Example

| Item | Profit | Weight | Ratio |
| ---- | ------ | ------ | ----- |
| 1    | 60     | 10     | 6     |
| 2    | 100    | 20     | 5     |
| 3    | 120    | 30     | 4     |

Capacity:

```text
50
```

Select:

```text
Item1 + Item2 + 2/3 Item3
```

Maximum Profit:

```text
240
```

---

## Algorithm

```text
Sort by profit/weight ratio

for each item

    if weight fits

        take whole item

    else

        take fraction

Return profit
```

---

## Complexity

Sorting:

[
O(n \log n)
]

---

# 8. Activity Selection Problem

## Problem Statement

Select maximum number of non-overlapping activities.

Each activity has:

* Start time
* Finish time

---

## Greedy Strategy

Always select activity with earliest finish time.

---

## Example

| Activity | Start | Finish |
| -------- | ----- | ------ |
| A1       | 1     | 2      |
| A2       | 3     | 4      |
| A3       | 0     | 6      |
| A4       | 5     | 7      |
| A5       | 8     | 9      |

Selected:

```text
A1, A2, A4, A5
```

---

## Algorithm

```text
Sort activities by finish time

Select first activity

for remaining activities

    if start >= previous finish

        select activity
```

---

## Complexity

Sorting:

[
O(n \log n)
]

Selection:

[
O(n)
]

Total:

[
O(n \log n)
]

---

# 9. Huffman Coding

## Definition

Huffman Coding is a lossless data compression technique.

It assigns:

* Shorter codes to frequent characters.
* Longer codes to rare characters.

---

## Steps

1. Create leaf nodes.
2. Insert into priority queue.
3. Remove two smallest frequencies.
4. Merge them.
5. Reinsert merged node.
6. Repeat until one node remains.

---

## Example

| Character | Frequency |
| --------- | --------- |
| A         | 5         |
| B         | 9         |
| C         | 12        |
| D         | 13        |
| E         | 16        |
| F         | 45        |

Construct Huffman Tree and assign codes.

Example Codes:

```text
F = 0
C = 100
D = 101
A = 1100
B = 1101
E = 111
```

---

## Complexity

Priority Queue Operations:

[
O(n \log n)
]

---

# Applications of Huffman Coding

* ZIP Files
* JPEG Compression
* MP3 Compression
* Data Transmission

---

# Complexity Summary

| Algorithm           | Time Complexity |
| ------------------- | --------------- |
| Kruskal             | O(E log V)      |
| Prim                | O(E log V)      |
| Dijkstra            | O(E log V)      |
| Fractional Knapsack | O(n log n)      |
| Activity Selection  | O(n log n)      |
| Huffman Coding      | O(n log n)      |

---

# Important Exam Questions

1. Define Greedy Algorithm and explain its characteristics.
2. Explain Greedy Choice Property and Optimal Substructure.
3. Explain Kruskal's Algorithm with example and complexity.
4. Explain Prim's Algorithm with example and complexity.
5. Differentiate between Prim's and Kruskal's Algorithm.
6. Explain Dijkstra's shortest path algorithm.
7. Solve Fractional Knapsack using Greedy approach.
8. Explain Activity Selection Problem with suitable example.
9. Explain Huffman Coding and its applications.
10. Compare Greedy Algorithms and Divide & Conquer Algorithms.
