# Experiment: Edit Distance

## 1. Aim

To find the **minimum number of edit operations** required to convert one string `str1` into another string `str2`, where the allowed operations are:

* **Insert** a character
* **Remove** a character
* **Replace** a character

All operations have equal cost of **1**.

---

# 2. Problem Statement

Given two strings `str1` and `str2`, determine the minimum number of operations required to convert `str1` into `str2`.

### Allowed Operations

| Operation | Description                       | Example      |
| --------- | --------------------------------- | ------------ |
| Insert    | Add a character                   | `cat → cart` |
| Remove    | Delete a character                | `cart → cat` |
| Replace   | Change one character into another | `cat → cut`  |

Each operation has a cost of **1**.

### Example

```text
str1 = "horse"
str2 = "ros"
```

Minimum operations:

```text
horse → rorse    Replace h with r
rorse → rose     Remove r
rose  → ros      Remove e
```

Therefore:

```text
Answer = 3
```

---

# 3. Theory

This problem is known as the **Edit Distance** or **Levenshtein Distance** problem.

It is a classic **Dynamic Programming** problem.

The main idea is to break the problem into smaller subproblems.

Let:

```text
dp[i][j]
```

represent the minimum number of operations required to convert the first `i` characters of `str1` into the first `j` characters of `str2`.

For example:

```text
dp[3][4]
```

represents the minimum operations required to convert:

```text
str1[0...2]
```

into:

```text
str2[0...3]
```

---

# 4. Approach / Logic

Consider the last characters of the two strings.

### Case 1: Last characters are equal

If:

```text
str1[i-1] == str2[j-1]
```

No operation is required.

Therefore:

```text
dp[i][j] = dp[i-1][j-1]
```

---

### Case 2: Last characters are different

We have three choices.

#### 1. Insert

Insert the required character into `str1`.

```text
dp[i][j-1] + 1
```

#### 2. Remove

Remove the last character from `str1`.

```text
dp[i-1][j] + 1
```

#### 3. Replace

Replace the last character of `str1` with the required character.

```text
dp[i-1][j-1] + 1
```

We select the minimum:

```text
dp[i][j] = 1 + min(
    dp[i][j-1],
    dp[i-1][j],
    dp[i-1][j-1]
)
```

---

# 5. Base Cases

If `str1` is empty:

```text
str1 = ""
str2 = "abc"
```

We need three insertions.

Therefore:

```text
dp[0][3] = 3
```

In general:

```text
dp[0][j] = j
```

If `str2` is empty:

```text
str1 = "abc"
str2 = ""
```

We need three removals.

Therefore:

```text
dp[3][0] = 3
```

In general:

```text
dp[i][0] = i
```

---

# 6. Algorithm

1. Start.
2. Read `str1` and `str2`.
3. Let:

   * `n = length of str1`
   * `m = length of str2`
4. Create a DP table of size `(n+1) × (m+1)`.
5. Initialize the first column:

   ```text
   dp[i][0] = i
   ```
6. Initialize the first row:

   ```text
   dp[0][j] = j
   ```
7. Traverse the table from `i = 1` to `n`.
8. For each `i`, traverse `j = 1` to `m`.
9. If `str1[i-1] == str2[j-1]`:

   ```text
   dp[i][j] = dp[i-1][j-1]
   ```
10. Otherwise:

    ```text
    dp[i][j] =
    1 + min(
        dp[i-1][j],
        dp[i][j-1],
        dp[i-1][j-1]
    )
    ```
11. Print `dp[n][m]`.
12. Stop.

---

# 7. Flowchart

```text
                 ┌─────────────┐
                 │    START    │
                 └──────┬──────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Read str1 and str2 │
             └──────────┬─────────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Create DP table    │
             │ (n+1) × (m+1)      │
             └──────────┬─────────┘
                        │
                        ▼
             ┌────────────────────┐
             │ Initialize first   │
             │ row and column     │
             └──────────┬─────────┘
                        │
                        ▼
                 ┌────────────┐
                 │ i = 1      │
                 └─────┬──────┘
                       │
                       ▼
                 ┌────────────┐
                 │ j = 1      │
                 └─────┬──────┘
                       │
                       ▼
              ┌──────────────────┐
              │ str1[i-1] ==     │
              │ str2[j-1] ?      │
              └───────┬──────────┘
                   Yes│       │No
                      │       │
                      ▼       ▼
              ┌────────────┐ ┌─────────────────────┐
              │ dp[i][j] = │ │ Find minimum of     │
              │ dp[i-1][j-1│ │ Insert, Remove and  │
              └──────┬─────┘ │ Replace + 1         │
                     │        └──────────┬──────────┘
                     │                   │
                     └────────┬──────────┘
                              │
                              ▼
                     ┌────────────────┐
                     │ More columns?  │
                     └───────┬────────┘
                         Yes  │  No
                              │
                              ▼
                     ┌────────────────┐
                     │ More rows?     │
                     └───────┬────────┘
                         Yes  │  No
                              │
                              ▼
                     ┌────────────────┐
                     │ Print dp[n][m] │
                     └───────┬────────┘
                             │
                             ▼
                       ┌───────────┐
                       │   STOP    │
                       └───────────┘
```

---

# 8. C++ Program

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;

int editDistance(string str1, string str2)
{
    int n = str1.length();
    int m = str2.length();

    vector<vector<int>> dp(n + 1, vector<int>(m + 1));

    // Convert empty str1 to str2
    for (int j = 0; j <= m; j++)
    {
        dp[0][j] = j;
    }

    // Convert str1 to empty str2
    for (int i = 0; i <= n; i++)
    {
        dp[i][0] = i;
    }

    // Fill DP table
    for (int i = 1; i <= n; i++)
    {
        for (int j = 1; j <= m; j++)
        {
            if (str1[i - 1] == str2[j - 1])
            {
                // No operation required
                dp[i][j] = dp[i - 1][j - 1];
            }
            else
            {
                int insertOperation = dp[i][j - 1];
                int removeOperation = dp[i - 1][j];
                int replaceOperation = dp[i - 1][j - 1];

                dp[i][j] = 1 + min({
                    insertOperation,
                    removeOperation,
                    replaceOperation
                });
            }
        }
    }

    return dp[n][m];
}

int main()
{
    string str1, str2;

    cout << "Enter first string: ";
    cin >> str1;

    cout << "Enter second string: ";
    cin >> str2;

    int result = editDistance(str1, str2);

    cout << "Minimum number of edits = " << result << endl;

    return 0;
}
```

---

# 9. Java Program

```java
import java.util.*;

public class EditDistance {

    static int editDistance(String str1, String str2) {

        int n = str1.length();
        int m = str2.length();

        int[][] dp = new int[n + 1][m + 1];

        // Convert empty str1 to str2
        for (int j = 0; j <= m; j++) {
            dp[0][j] = j;
        }

        // Convert str1 to empty str2
        for (int i = 0; i <= n; i++) {
            dp[i][0] = i;
        }

        // Fill DP table
        for (int i = 1; i <= n; i++) {

            for (int j = 1; j <= m; j++) {

                if (str1.charAt(i - 1) == str2.charAt(j - 1)) {

                    // No operation required
                    dp[i][j] = dp[i - 1][j - 1];

                } else {

                    int insertOperation = dp[i][j - 1];
                    int removeOperation = dp[i - 1][j];
                    int replaceOperation = dp[i - 1][j - 1];

                    dp[i][j] = 1 + Math.min(
                            insertOperation,
                            Math.min(removeOperation, replaceOperation)
                    );
                }
            }
        }

        return dp[n][m];
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter first string: ");
        String str1 = sc.nextLine();

        System.out.print("Enter second string: ");
        String str2 = sc.nextLine();

        int result = editDistance(str1, str2);

        System.out.println("Minimum number of edits = " + result);

        sc.close();
    }
}
```

---

# 10. Sample Input

```text
Enter first string: horse
Enter second string: ros
```

# 11. Sample Output

```text
Minimum number of edits = 3
```

---

# 12. Dry Run

Consider:

```text
str1 = "cat"
str2 = "cut"
```

We need to convert:

```text
cat → cut
```

The first character `c` is the same.

The last character `t` is also the same.

Only the middle character is different:

```text
a → u
```

So one replacement is sufficient:

```text
cat
 ↓
cut
```

Therefore:

```text
Minimum number of edits = 1
```

### DP Table

For `cat` and `cut`:

```text
       ""   c   u   t
   ""   0   1   2   3
   c    1   0   1   2
   a    2   1   1   2
   t    3   2   2   1
```

The final answer is:

```text
dp[3][3] = 1
```

---

# 13. Another Example

Consider:

```text
str1 = "horse"
str2 = "ros"
```

One optimal sequence is:

```text
horse
  ↓ Replace h with r
rorse
  ↓ Remove r
rose
  ↓ Remove e
ros
```

Therefore:

```text
Minimum edits = 3
```

---

# 14. Time Complexity

The DP table contains `(n + 1) × (m + 1)` cells.

Each cell takes constant time to calculate.

Therefore:

**Time Complexity:**

```text
O(n × m)
```

where:

* `n` = length of `str1`
* `m` = length of `str2`

---

# 15. Space Complexity

The DP table requires:

```text
(n + 1) × (m + 1)
```

memory.

Therefore:

**Space Complexity:**

```text
O(n × m)
```

---

# 16. Design Technique

**Design Technique: Dynamic Programming**

The problem has:

1. **Overlapping subproblems** — the same smaller string-conversion problems occur repeatedly.
2. **Optimal substructure** — the optimal solution for larger strings can be constructed from optimal solutions of smaller prefixes.

Therefore, Dynamic Programming is suitable for solving the problem efficiently.

---

# 17. Important Points

* **Insertion** costs `1`.
* **Removal** costs `1`.
* **Replacement** costs `1`.
* If corresponding characters are equal, no operation is required.
* The final answer is stored in `dp[n][m]`.
* If both strings are identical, the answer is `0`.
* If one string is empty, the answer is the length of the other string.
* The standard solution uses **2D Dynamic Programming**.

---

# 18. Conclusion

The minimum number of edits required to convert `str1` into `str2` can be found using **Dynamic Programming**. For every pair of prefixes, we consider insertion, removal, and replacement and select the operation requiring the minimum number of edits. The algorithm has **O(n × m) time complexity** and **O(n × m) space complexity**.
