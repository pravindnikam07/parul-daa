# Experiment 3: Minimum Number of Candies Distribution

## Aim

To write a program to calculate the minimum number of candies required to distribute among N children based on their ratings such that:

1. Each child gets at least one candy.
2. A child with a higher rating than an adjacent child receives more candies than that neighbour.

---

# Theory

Given an array of ratings representing the ratings of N children standing in a line, distribute candies according to the following rules:

* Every child must receive at least one candy.
* Children with higher ratings than their immediate neighbours must receive more candies.

### Example

```text
Ratings = [1, 0, 2]
```

Candy Distribution:

```text
[2, 1, 2]
```

Total Candies:

```text
2 + 1 + 2 = 5
```

### Approach

Use a **Greedy Algorithm** with two passes:

#### Left to Right Pass

* If the current child's rating is greater than the previous child's rating, assign one more candy than the previous child.

#### Right to Left Pass

* If the current child's rating is greater than the next child's rating, ensure it has at least one more candy than the next child.

The sum of all candies gives the minimum candies required.

---

# Algorithm

### Algorithm: Minimum Candies Distribution

**Step 1:** Start

**Step 2:** Input number of children `N`

**Step 3:** Input ratings array

**Step 4:** Create an array `candies[]` of size N and initialize all elements to 1

**Step 5:** Traverse from Left to Right

* For `i = 1` to `N-1`

  * If `ratings[i] > ratings[i-1]`

    * `candies[i] = candies[i-1] + 1`

**Step 6:** Traverse from Right to Left

* For `i = N-2` to `0`

  * If `ratings[i] > ratings[i+1]`

    * `candies[i] = max(candies[i], candies[i+1] + 1)`

**Step 7:** Calculate total candies by summing all elements of `candies[]`

**Step 8:** Display total candies

**Step 9:** Stop

---

# Flowchart

```text
 ┌─────────┐
 │ Start   │
 └────┬────┘
      │
      ▼
 ┌────────────────┐
 │ Input N and    │
 │ ratings array  │
 └────┬───────────┘
      │
      ▼
 ┌────────────────┐
 │ Initialize all │
 │ candies = 1    │
 └────┬───────────┘
      │
      ▼
 ┌────────────────────┐
 │ Left to Right Pass │
 └────┬───────────────┘
      │
      ▼
 ┌────────────────────┐
 │ Right to Left Pass │
 └────┬───────────────┘
      │
      ▼
 ┌──────────────────┐
 │ Sum all candies  │
 └────┬─────────────┘
      │
      ▼
 ┌──────────────────┐
 │ Display Total    │
 └────┬─────────────┘
      │
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
using namespace std;

int minCandies(vector<int>& ratings) {
    int n = ratings.size();

    vector<int> candies(n, 1);

    // Left to Right
    for (int i = 1; i < n; i++) {
        if (ratings[i] > ratings[i - 1]) {
            candies[i] = candies[i - 1] + 1;
        }
    }

    // Right to Left
    for (int i = n - 2; i >= 0; i--) {
        if (ratings[i] > ratings[i + 1]) {
            candies[i] = max(candies[i], candies[i + 1] + 1);
        }
    }

    int total = 0;
    for (int candy : candies) {
        total += candy;
    }

    return total;
}

int main() {
    int n;

    cout << "Enter number of children: ";
    cin >> n;

    vector<int> ratings(n);

    cout << "Enter ratings: ";
    for (int i = 0; i < n; i++) {
        cin >> ratings[i];
    }

    cout << "Minimum Candies Required: "
         << minCandies(ratings);

    return 0;
}
```

---

# Java Program

```java
import java.util.Scanner;

public class CandyDistribution {

    public static int minCandies(int[] ratings) {
        int n = ratings.length;

        int[] candies = new int[n];

        for (int i = 0; i < n; i++) {
            candies[i] = 1;
        }

        // Left to Right
        for (int i = 1; i < n; i++) {
            if (ratings[i] > ratings[i - 1]) {
                candies[i] = candies[i - 1] + 1;
            }
        }

        // Right to Left
        for (int i = n - 2; i >= 0; i--) {
            if (ratings[i] > ratings[i + 1]) {
                candies[i] = Math.max(candies[i],
                                      candies[i + 1] + 1);
            }
        }

        int total = 0;

        for (int candy : candies) {
            total += candy;
        }

        return total;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of children: ");
        int n = sc.nextInt();

        int[] ratings = new int[n];

        System.out.print("Enter ratings: ");
        for (int i = 0; i < n; i++) {
            ratings[i] = sc.nextInt();
        }

        System.out.println("Minimum Candies Required: "
                           + minCandies(ratings));

        sc.close();
    }
}
```

---

# Sample Output 1

```text
Enter number of children: 3
Enter ratings: 1 0 2
Minimum Candies Required: 5
```

### Candy Distribution

```text
Ratings : 1 0 2
Candies : 2 1 2
```

---

# Sample Output 2

```text
Enter number of children: 3
Enter ratings: 1 2 2
Minimum Candies Required: 4
```

### Candy Distribution

```text
Ratings : 1 2 2
Candies : 1 2 1
```

---

# Dry Run

### Input

```text
Ratings = [1, 0, 2]
```

### Initial Candies

```text
[1, 1, 1]
```

### Left to Right Pass

```text
[1, 1, 2]
```

### Right to Left Pass

```text
[2, 1, 2]
```

### Total

```text
2 + 1 + 2 = 5
```

---

# Time Complexity

* Left Pass: O(N)
* Right Pass: O(N)

Overall:

```text
O(N)
```

---

# Space Complexity

```text
O(N)
```

---

# Design Technique

**Greedy Algorithm**

The solution greedily assigns candies while satisfying both neighbour conditions using two traversals of the ratings array.

---

# Conclusion

The program successfully computes the minimum number of candies required to distribute among children according to their ratings. By using a Greedy approach with two passes, the solution ensures all constraints are satisfied while achieving an efficient time complexity of **O(N)**.
