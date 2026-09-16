# Unit 6: Backtracking and Branch & Bound

## Topic 6.3 — 0/1 Knapsack Problem

The **0/1 Knapsack Problem** is an optimization problem where each item can either be **selected completely (1)** or **not selected (0)**.

It can be solved using different techniques. In this topic, we focus on **Backtracking and Branch & Bound**.

---

## 1. Problem Definition

Given:

* `n` items
* Weight `w[i]` of each item
* Profit `p[i]` of each item
* Knapsack capacity `W`

Select items such that:

1. Total weight does not exceed `W`.
2. Total profit is maximum.
3. Each item is either selected or rejected.

### Mathematical Form

Maximize:

$$
\sum_{i=1}^{n} p_i x_i
$$

Subject to:

$$
\sum_{i=1}^{n} w_i x_i \leq W
$$

where:

$$
x_i \in \{0,1\}
$$

---

# 2. Why "0/1"?

For every item:

```text
x[i] = 1 → Include the item
x[i] = 0 → Exclude the item
```

There is no partial selection.

For example:

```text
Item 1 → Take OR Don't Take
Item 2 → Take OR Don't Take
Item 3 → Take OR Don't Take
```

Unlike **Fractional Knapsack**, we cannot take half an item.

---

# 3. Example

Consider:

| Item | Weight | Profit |
| ---- | -----: | -----: |
| 1    |      2 |     40 |
| 2    |      3 |     50 |
| 3    |      4 |     65 |
| 4    |      5 |     70 |

Capacity:

```text
W = 7
```

Possible selections include:

```text
Item 1 + Item 2
Weight = 2 + 3 = 5
Profit = 40 + 50 = 90
```

```text
Item 2 + Item 3
Weight = 3 + 4 = 7
Profit = 50 + 65 = 115
```

```text
Item 1 + Item 4
Weight = 2 + 5 = 7
Profit = 40 + 70 = 110
```

The objective is to find the combination having the **maximum profit without exceeding capacity**.

---

# 4. State-Space Tree

For every item, there are two choices:

```text
                 Start
                /     \
             Take     Don't Take
             Item 1    Item 1
             /           \
          Take           Don't Take
         Item 2          Item 2
         /   \            /   \
       ...   ...        ...   ...
```

For `n` items, there can be up to:

$$
2^n
$$

possible selections.

This is why brute-force solution becomes expensive as `n` increases.

---

# 5. 0/1 Knapsack Using Backtracking

Backtracking explores the choices recursively.

At each item:

```text
Include item
OR
Exclude item
```

If the current weight exceeds capacity:

```text
Prune
```

because adding more items cannot make the solution valid again.

---

## 6. Backtracking Algorithm

```text
Knapsack(i, weight, profit)

    if weight > capacity:
        return

    if i == n:
        update maximum profit
        return

    Include item i
        Knapsack(i + 1,
                 weight + w[i],
                 profit + p[i])

    Exclude item i
        Knapsack(i + 1,
                 weight,
                 profit)
```

This is basic backtracking.

A stronger backtracking solution can also use an upper bound to prune branches that cannot beat the current best.

---

# 7. Dry Run

Consider:

```text
Capacity = 5

Item       Weight     Profit
1             2          40
2             3          50
3             4          65
```

State-space tree:

```text
                       Start
                      W=0 P=0
                     /       \
                Take 1      Skip 1
                W=2 P=40    W=0 P=0
                 /    \       /    \
             Take 2  Skip 2 Take 2 Skip 2
             W=5 P=90 W=2 P=40 W=3 P=50 W=0 P=0
```

Now consider Item 3.

From:

```text
W=5, P=90
```

including Item 3 gives:

```text
Weight = 5 + 4 = 9
```

Since:

```text
9 > 5
```

the branch is pruned.

The solution:

```text
Item 1 + Item 2
```

has:

```text
Weight = 5
Profit = 90
```

---

# 8. Branch and Bound for 0/1 Knapsack

Backtracking can be improved using **Branch and Bound**.

Instead of pruning only when:

```text
Weight > Capacity
```

we also ask:

> "Even under the best possible situation, can this branch beat the current best profit?"

If the answer is no, prune it.

---

# 9. Upper Bound

For the 0/1 Knapsack maximization problem, we calculate an **upper bound** on the profit possible from a node.

A common method is to calculate the bound using the **Fractional Knapsack idea**.

The bound assumes that remaining items can be taken fractionally.

Therefore:

```text
Upper Bound
     ≥
Best possible 0/1 profit
```

This makes it useful for deciding whether a branch can be discarded.

---

# 10. Example of Bounding

Suppose:

```text
Current Best Profit = 100
```

For a particular node:

```text
Upper Bound = 130
```

Since:

```text
130 > 100
```

the branch **may** contain a better solution.

Therefore:

```text
Explore
```

Another node:

```text
Current Best = 100
Upper Bound = 90
```

Since:

```text
90 ≤ 100
```

even the optimistic bound cannot beat the current best.

Therefore:

```text
Prune
```

---

# 11. Branch and Bound Algorithm

```text
1. Sort items according to profit/weight ratio.

2. Create the root node.

3. Calculate the upper bound of the root.

4. Select a live node.

5. Create two branches:
       Include current item
       Exclude current item

6. Calculate the bound for each child.

7. If the child is feasible and its profit
   is better than the current best:
       Update best profit.

8. If its bound can improve the current best:
       Keep the node for further exploration.

9. Otherwise:
       Prune the node.

10. Repeat until no promising nodes remain.
```

---

# 12. Profit/Weight Ratio

For calculating the bound, items are commonly considered according to:

$$
\frac{Profit}{Weight}
$$

Example:

| Item | Weight | Profit | Profit/Weight |
| ---- | -----: | -----: | ------------: |
| 1    |      2 |     40 |            20 |
| 2    |      3 |     50 |         16.67 |
| 3    |      4 |     65 |         16.25 |

Higher ratio means more profit per unit of weight.

**Important:** Sorting by ratio is used for calculating the bound; it does **not** turn the 0/1 Knapsack problem into Fractional Knapsack.

---

# 13. Bound Calculation Example

Suppose:

```text
Capacity = 7
```

Current state:

```text
Weight = 2
Profit = 40
```

Remaining capacity:

```text
7 - 2 = 5
```

Suppose the next items have:

```text
Item 2 → weight 3, profit 50
Item 3 → weight 4, profit 65
```

We can take Item 2 completely:

```text
Weight = 2 + 3 = 5
Profit = 40 + 50 = 90
```

Remaining capacity:

```text
7 - 5 = 2
```

For the bound, we can optimistically take:

```text
2/4 of Item 3
```

Additional profit:

$$
65 \times \frac{2}{4}=32.5
$$

Therefore:

$$
Bound = 90+32.5=122.5
$$

So:

```text
Upper Bound = 122.5
```

This branch may potentially produce a solution better than the current best.

---

# 14. Backtracking vs Branch and Bound in Knapsack

| Feature                 | Backtracking                   | Branch & Bound       |
| ----------------------- | ------------------------------ | -------------------- |
| Branching               | Include/Exclude                | Include/Exclude      |
| Constraint pruning      | Yes                            | Yes                  |
| Bound calculation       | Not required                   | Required             |
| Objective-based pruning | Optional                       | Main idea            |
| Goal                    | Find feasible/optimal solution | Optimize efficiently |
| Knapsack bound          | Not necessary                  | Upper bound          |
| Search                  | Usually DFS                    | DFS/BFS/Best-Bound   |

---

# 15. Java Implementation — Branch and Bound

```java
import java.util.*;

class Item {
    int weight;
    int profit;
    double ratio;

    Item(int weight, int profit) {
        this.weight = weight;
        this.profit = profit;
        this.ratio = (double) profit / weight;
    }
}

class Node {
    int level;
    int weight;
    int profit;
    double bound;

    Node(int level, int weight, int profit, double bound) {
        this.level = level;
        this.weight = weight;
        this.profit = profit;
        this.bound = bound;
    }
}

public class KnapsackBB {

    static double bound(Node u, int capacity, Item[] items) {

        if (u.weight >= capacity)
            return 0;

        double profitBound = u.profit;
        int totalWeight = u.weight;
        int i = u.level + 1;

        while (i < items.length &&
               totalWeight + items[i].weight <= capacity) {

            totalWeight += items[i].weight;
            profitBound += items[i].profit;
            i++;
        }

        if (i < items.length) {
            profitBound +=
                (capacity - totalWeight)
                * items[i].ratio;
        }

        return profitBound;
    }

    static int knapsack(int capacity, Item[] items) {

        Arrays.sort(items,
            (a, b) -> Double.compare(b.ratio, a.ratio));

        PriorityQueue<Node> pq =
            new PriorityQueue<>(
                (a, b) ->
                    Double.compare(b.bound, a.bound)
            );

        Node root = new Node(-1, 0, 0, 0);
        root.bound = bound(root, capacity, items);

        pq.add(root);

        int maxProfit = 0;

        while (!pq.isEmpty()) {

            Node u = pq.poll();

            if (u.bound <= maxProfit)
                continue;

            int next = u.level + 1;

            if (next >= items.length)
                continue;

            // Include item
            Node v = new Node(
                next,
                u.weight + items[next].weight,
                u.profit + items[next].profit,
                0
            );

            if (v.weight <= capacity &&
                v.profit > maxProfit) {

                maxProfit = v.profit;
            }

            v.bound = bound(v, capacity, items);

            if (v.bound > maxProfit) {
                pq.add(v);
            }

            // Exclude item
            v = new Node(
                next,
                u.weight,
                u.profit,
                0
            );

            v.bound = bound(v, capacity, items);

            if (v.bound > maxProfit) {
                pq.add(v);
            }
        }

        return maxProfit;
    }

    public static void main(String[] args) {

        Item[] items = {
            new Item(2, 40),
            new Item(3, 50),
            new Item(4, 65),
            new Item(5, 70)
        };

        int capacity = 7;

        System.out.println(
            "Maximum Profit = " +
            knapsack(capacity, items)
        );
    }
}
```

### Output

```text
Maximum Profit = 115
```

The selected items correspond to:

```text
Item 2 + Item 3

Weight = 3 + 4 = 7
Profit = 50 + 65 = 115
```

---

# 16. C++ Implementation — Branch and Bound

```cpp
#include <iostream>
#include <vector>
#include <queue>
#include <algorithm>
using namespace std;

struct Item {
    int weight;
    int profit;
    double ratio;
};

struct Node {
    int level;
    int weight;
    int profit;
    double bound;
};

struct Compare {
    bool operator()(Node a, Node b) {
        return a.bound < b.bound;
    }
};

double bound(Node u, int capacity,
             vector<Item>& items) {

    if (u.weight >= capacity)
        return 0;

    double profitBound = u.profit;
    int totalWeight = u.weight;
    int i = u.level + 1;

    while (i < items.size() &&
           totalWeight + items[i].weight <= capacity) {

        totalWeight += items[i].weight;
        profitBound += items[i].profit;
        i++;
    }

    if (i < items.size()) {

        profitBound +=
            (capacity - totalWeight)
            * items[i].ratio;
    }

    return profitBound;
}

int knapsack(int capacity, vector<Item>& items) {

    sort(items.begin(), items.end(),
         [](Item a, Item b) {
             return a.ratio > b.ratio;
         });

    priority_queue<Node,
                   vector<Node>,
                   Compare> pq;

    Node root = {-1, 0, 0, 0};

    root.bound = bound(root, capacity, items);

    pq.push(root);

    int maxProfit = 0;

    while (!pq.empty()) {

        Node u = pq.top();
        pq.pop();

        if (u.bound <= maxProfit)
            continue;

        int next = u.level + 1;

        if (next >= items.size())
            continue;

        // Include item
        Node v;

        v.level = next;
        v.weight =
            u.weight + items[next].weight;
        v.profit =
            u.profit + items[next].profit;

        if (v.weight <= capacity &&
            v.profit > maxProfit) {

            maxProfit = v.profit;
        }

        v.bound =
            bound(v, capacity, items);

        if (v.bound > maxProfit)
            pq.push(v);

        // Exclude item
        v.level = next;
        v.weight = u.weight;
        v.profit = u.profit;

        v.bound =
            bound(v, capacity, items);

        if (v.bound > maxProfit)
            pq.push(v);
    }

    return maxProfit;
}

int main() {

    vector<Item> items = {
        {2, 40, 0},
        {3, 50, 0},
        {4, 65, 0},
        {5, 70, 0}
    };

    for (auto &item : items)
        item.ratio =
            (double)item.profit / item.weight;

    int capacity = 7;

    cout << "Maximum Profit = "
         << knapsack(capacity, items);

    return 0;
}
```

### Output

```text
Maximum Profit = 115
```

---

# 17. Important Point About Complexity

For `n` items, there can be up to:

$$
2^n
$$

possible subsets.

Therefore, the worst-case complexity of Branch and Bound for 0/1 Knapsack can still be **exponential**.

Its practical advantage comes from **pruning unpromising branches**.

---

# 18. Backtracking vs Dynamic Programming

The same 0/1 Knapsack problem can also be solved using Dynamic Programming.

| Feature               | Backtracking               | Branch & Bound             | Dynamic Programming        |
| --------------------- | -------------------------- | -------------------------- | -------------------------- |
| Main approach         | Search                     | Search + bound             | Store subproblem results   |
| Typical complexity    | Exponential                | Exponential worst case     | O(nW)                      |
| Uses state-space tree | Yes                        | Yes                        | No                         |
| Uses bound            | No/optional                | Yes                        | No                         |
| Memory                | Search tree/recursion      | Live nodes                 | DP table                   |
| Suitable when         | Search space can be pruned | Optimization + good bounds | Capacity `W` is manageable |

---

# 19. Exam Points

Remember these statements:

> **0/1 Knapsack:** Every item is either completely selected or completely rejected.

> **Branching:** For every item, create Include and Exclude branches.

> **Bounding:** Estimate the maximum possible profit from a node.

> **Pruning:** If the upper bound cannot exceed the current best profit, discard the branch.

For maximization:

$$
Bound \le Best \Rightarrow Prune
$$

---

# 20. Student Task

Solve the following using a **state-space tree**.

| Item | Weight | Profit |
| ---- | -----: | -----: |
| 1    |      2 |     20 |
| 2    |      3 |     30 |
| 3    |      4 |     50 |
| 4    |      5 |     60 |

Capacity:

```text
W = 7
```

### Tasks

1. Calculate `profit/weight` ratio.
2. Arrange the items according to ratio.
3. Draw the Include/Exclude state-space tree.
4. Find the optimal profit.
5. Identify at least two branches that can be pruned.
6. Explain why those branches are pruned.

---

# 21. Viva Questions

1. What is the 0/1 Knapsack problem?
2. Why is it called 0/1 Knapsack?
3. What are the two branches generated for each item?
4. What is the purpose of a bound?
5. Why is fractional knapsack useful for calculating the bound?
6. What is an upper bound?
7. When is a branch pruned in maximization?
8. What is the worst-case complexity of Branch and Bound for 0/1 Knapsack?
9. What is the difference between 0/1 and fractional knapsack?
10. Why are items sorted by profit/weight ratio for Branch and Bound?
11. Does sorting by ratio itself solve the 0/1 Knapsack problem?
12. How is Branch and Bound different from Dynamic Programming?

---

## Quick Revision

```text
             0/1 Knapsack
                   |
             For each item
              /        \
          Include     Exclude
             |           |
         Calculate     Calculate
          weight        bound
             |           |
       Feasible?      Promising?
          /               \
        Yes               Yes
         ↓                 ↓
   Update best          Explore
                         |
                    Otherwise
                         ↓
                       Prune
```

**Key formula:**

$$
\boxed{Bound \le Best \Rightarrow Prune}
$$

for a maximization problem.
