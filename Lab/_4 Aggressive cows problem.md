# Experiment 4: Aggressive Cows Problem

## Aim

To write a program to place **C cows** in **N stalls** such that the **minimum distance between any two cows is maximized**.

---

# Theory

Given the positions of **N stalls** on a straight line and **C cows**, the objective is to place the cows in the stalls so that the **minimum distance between any two cows is as large as possible**.

Since the stall positions are not necessarily sorted, they must first be sorted.

The problem is solved using:

- **Sorting**
- **Binary Search** on the answer (minimum distance)
- **Greedy Approach** to check whether cows can be placed.

### Example

```text
Stalls = [1, 2, 4, 8, 9]
Cows = 3
```

Possible Placement:

```text
Cow 1 → Stall 1
Cow 2 → Stall 4
Cow 3 → Stall 8
```

Minimum Distance = **3**

Hence, the answer is:

```text
3
```

---

# Theory Behind the Algorithm

The minimum distance can vary from:

- **Low = 1**
- **High = Maximum Stall Position − Minimum Stall Position**

Using **Binary Search**, we check whether a distance `mid` is possible.

If possible:

- Try a larger distance.

Otherwise:

- Try a smaller distance.

This gives the maximum possible minimum distance.

---

# Algorithm

### Algorithm: Aggressive Cows

**Step 1:** Start

**Step 2:** Input the number of stalls `N`

**Step 3:** Input the stall positions

**Step 4:** Input the number of cows `C`

**Step 5:** Sort the stall positions

**Step 6:** Initialize

- `low = 1`
- `high = last stall − first stall`
- `answer = 0`

**Step 7:** Repeat while `low <= high`

- Calculate

  `mid = (low + high) / 2`

- Check if cows can be placed with minimum distance = `mid`

If Yes

- Store `answer = mid`
- Search in the right half
- `low = mid + 1`

Else

- Search in the left half
- `high = mid - 1`

**Step 8:** Print `answer`

**Step 9:** Stop

---

# Flowchart

```text
 ┌─────────┐
 │ Start   │
 └────┬────┘
      │
      ▼
 ┌────────────────────┐
 │ Input N, stalls, C │
 └────┬───────────────┘
      │
      ▼
 ┌────────────────────┐
 │ Sort stall array   │
 └────┬───────────────┘
      │
      ▼
 ┌────────────────────┐
 │ low=1              │
 │ high=max-min       │
 └────┬───────────────┘
      │
      ▼
 ┌────────────────────┐
 │ low <= high ?      │
 └───┬───────────┬────┘
     │Yes        │No
     ▼           ▼
┌──────────────┐ ┌──────────────┐
│ mid=(low+    │ │ Print answer │
│ high)/2      │ └──────┬───────┘
└─────┬────────┘        │
      ▼                 ▼
┌────────────────────┐ ┌────────┐
│ Can place cows ?   │ │ Stop   │
└───┬───────────┬────┘ └────────┘
    │Yes        │No
    ▼           ▼
┌────────────┐ ┌────────────┐
│answer=mid  │ │high=mid-1  │
│low=mid+1   │ └─────┬──────┘
└─────┬──────┘       │
      └──────────────┘
           Repeat
```

---

# C++ Program

```cpp
#include <iostream>
#include <algorithm>
using namespace std;

bool canPlace(int stalls[], int n, int cows, int dist) {
    int count = 1;
    int lastPos = stalls[0];

    for (int i = 1; i < n; i++) {
        if (stalls[i] - lastPos >= dist) {
            count++;
            lastPos = stalls[i];

            if (count == cows)
                return true;
        }
    }
    return false;
}

int aggressiveCows(int stalls[], int n, int cows) {
    sort(stalls, stalls + n);

    int low = 1;
    int high = stalls[n - 1] - stalls[0];
    int ans = 0;

    while (low <= high) {
        int mid = low + (high - low) / 2;

        if (canPlace(stalls, n, cows, mid)) {
            ans = mid;
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }

    return ans;
}

int main() {
    int n, cows;

    cout << "Enter number of stalls: ";
    cin >> n;

    int stalls[n];

    cout << "Enter stall positions: ";
    for (int i = 0; i < n; i++)
        cin >> stalls[i];

    cout << "Enter number of cows: ";
    cin >> cows;

    cout << "Largest Minimum Distance = "
         << aggressiveCows(stalls, n, cows);

    return 0;
}
```

---

# Java Program

```java
import java.util.Arrays;
import java.util.Scanner;

public class AggressiveCows {

    static boolean canPlace(int[] stalls, int cows, int dist) {
        int count = 1;
        int lastPos = stalls[0];

        for (int i = 1; i < stalls.length; i++) {
            if (stalls[i] - lastPos >= dist) {
                count++;
                lastPos = stalls[i];

                if (count == cows)
                    return true;
            }
        }
        return false;
    }

    static int aggressiveCows(int[] stalls, int cows) {
        Arrays.sort(stalls);

        int low = 1;
        int high = stalls[stalls.length - 1] - stalls[0];
        int ans = 0;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (canPlace(stalls, cows, mid)) {
                ans = mid;
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return ans;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter number of stalls: ");
        int n = sc.nextInt();

        int[] stalls = new int[n];

        System.out.print("Enter stall positions: ");
        for (int i = 0; i < n; i++) {
            stalls[i] = sc.nextInt();
        }

        System.out.print("Enter number of cows: ");
        int cows = sc.nextInt();

        System.out.println("Largest Minimum Distance = "
                + aggressiveCows(stalls, cows));

        sc.close();
    }
}
```

---

# Sample Output

### Sample 1

```text
Enter number of stalls: 5
Enter stall positions: 1 2 4 8 9
Enter number of cows: 3
Largest Minimum Distance = 3
```

### Sample 2

```text
Enter number of stalls: 6
Enter stall positions: 2 4 7 10 13 18
Enter number of cows: 3
Largest Minimum Distance = 8
```

---

# Dry Run

### Input

```text
Stalls = [1, 2, 4, 8, 9]
Cows = 3
```

After Sorting:

```text
[1, 2, 4, 8, 9]
```

Binary Search:

| Low | High | Mid | Can Place? | Answer |
| --- | ---- | --- | ---------- | ------ |
| 1   | 8    | 4   | No         | 0      |
| 1   | 3    | 2   | Yes        | 2      |
| 3   | 3    | 3   | Yes        | 3      |

Final Answer:

```text
Largest Minimum Distance = 3
```

---

# Time Complexity

- Sorting: **O(N log N)**
- Binary Search: **O(log D)**, where **D = maxPosition − minPosition**
- Feasibility Check: **O(N)**

Overall:

```text
O(N log N + N log D)
```

---

# Space Complexity

```text
O(1)
```

_(Ignoring the space used by the sorting algorithm.)_

---

# Design Technique

**Binary Search + Greedy Algorithm**

- **Binary Search** is used to search the maximum feasible minimum distance.
- **Greedy Algorithm** is used to place cows in the leftmost possible stalls while maintaining the required minimum distance.

---

# Conclusion

The program successfully determines the **largest possible minimum distance** between any two cows by combining **sorting**, **binary search**, and a **greedy placement strategy**. This approach is highly efficient and significantly faster than checking every possible arrangement, making it suitable for large input sizes.
