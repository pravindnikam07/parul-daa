# Experiment 2: Search Insert Position Using Binary Search

## Aim

To write a program that returns the index of a target element in a sorted array. If the target is not present, return the index where it should be inserted to maintain the sorted order.

---

# Theory

Given a sorted array of distinct integers and a target value:

* If the target exists in the array, return its index.
* If the target does not exist, return the position where it can be inserted while maintaining the sorted order.

### Example

| Array        | Target | Output |
| ------------ | ------ | ------ |
| [1, 3, 5, 6] | 5      | 2      |
| [1, 3, 5, 6] | 2      | 1      |
| [1, 3, 5, 6] | 7      | 4      |
| [1, 3, 5, 6] | 0      | 0      |

### Logic

Since the array is already sorted, **Binary Search** can be used to efficiently find the target or its insertion position.

* If the target is found, return its index.
* If the target is not found, the value of `low` after the search ends represents the correct insertion position.

---

# Algorithm

### Algorithm: Search Insert Position

**Step 1:** Start

**Step 2:** Input the size of the array `n`

**Step 3:** Input the sorted array elements

**Step 4:** Input the target value

**Step 5:** Initialize:

* `low = 0`
* `high = n - 1`

**Step 6:** Repeat while `low <= high`

* Calculate:

  * `mid = low + (high - low) / 2`

* If `arr[mid] == target`

  * Return `mid`

* Else if `arr[mid] < target`

  * Set `low = mid + 1`

* Else

  * Set `high = mid - 1`

**Step 7:** Return `low` as the insertion position

**Step 8:** Stop

---

# Flowchart

```text
 ┌─────────┐
 │  Start  │
 └────┬────┘
      │
      ▼
 ┌─────────────────┐
 │ Input n, array  │
 │ and target      │
 └────┬────────────┘
      │
      ▼
 ┌─────────────────┐
 │ low=0           │
 │ high=n-1        │
 └────┬────────────┘
      │
      ▼
 ┌─────────────────┐
 │ low <= high ?   │
 └───┬─────────┬───┘
     │Yes      │No
     ▼         ▼
┌───────────┐  ┌─────────────┐
│ mid=(low+ │  │ Return low  │
│ high)/2   │  └──────┬──────┘
└─────┬─────┘         │
      ▼               ▼
┌─────────────────┐ ┌───────┐
│ arr[mid]==target│ │ Stop  │
└───┬─────────┬───┘ └───────┘
    │Yes      │No
    ▼         ▼
┌──────────┐ ┌─────────────────┐
│Return mid│ │ arr[mid]<target?│
└────┬─────┘ └───┬─────────┬───┘
     │           │Yes      │No
     │           ▼         ▼
     │    ┌──────────┐ ┌──────────┐
     │    │low=mid+1 │ │high=mid-1│
     │    └────┬─────┘ └────┬─────┘
     └─────────┴────────────┘
               │
               ▼
      (Repeat Loop)
```

---

# C++ Program

```cpp
#include <iostream>
using namespace std;

int searchInsert(int arr[], int n, int target)
{
    int low = 0;
    int high = n - 1;

    while (low <= high)
    {
        int mid = low + (high - low) / 2;

        if (arr[mid] == target)
            return mid;
        else if (arr[mid] < target)
            low = mid + 1;
        else
            high = mid - 1;
    }

    return low;
}

int main()
{
    int n, target;

    cout << "Enter size of array: ";
    cin >> n;

    int arr[n];

    cout << "Enter sorted array elements: ";
    for (int i = 0; i < n; i++)
        cin >> arr[i];

    cout << "Enter target value: ";
    cin >> target;

    cout << "Index: " << searchInsert(arr, n, target);

    return 0;
}
```

---

# Java Program

```java
import java.util.Scanner;

public class SearchInsertPosition {

    public static int searchInsert(int[] arr, int target) {
        int low = 0;
        int high = arr.length - 1;

        while (low <= high) {
            int mid = low + (high - low) / 2;

            if (arr[mid] == target)
                return mid;
            else if (arr[mid] < target)
                low = mid + 1;
            else
                high = mid - 1;
        }

        return low;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter size of array: ");
        int n = sc.nextInt();

        int[] arr = new int[n];

        System.out.print("Enter sorted array elements: ");
        for (int i = 0; i < n; i++) {
            arr[i] = sc.nextInt();
        }

        System.out.print("Enter target value: ");
        int target = sc.nextInt();

        System.out.println("Index: " + searchInsert(arr, target));

        sc.close();
    }
}
```

---

# Sample Output 1

```text
Enter size of array: 4
Enter sorted array elements: 1 3 5 6
Enter target value: 5
Index: 2
```

# Sample Output 2

```text
Enter size of array: 4
Enter sorted array elements: 1 3 5 6
Enter target value: 2
Index: 1
```

# Sample Output 3

```text
Enter size of array: 4
Enter sorted array elements: 1 3 5 6
Enter target value: 7
Index: 4
```

---

# Dry Run

### Input

```text
Array = [1, 3, 5, 6]
Target = 2
```

| Low | High | Mid | arr[Mid] | Action   |
| --- | ---- | --- | -------- | -------- |
| 0   | 3    | 1   | 3        | high = 0 |
| 0   | 0    | 0   | 1        | low = 1  |

Loop ends because `low > high`.

Insertion position = **1**

---

# Time Complexity

* Best Case: **O(1)**
* Average Case: **O(log n)**
* Worst Case: **O(log n)**

---

# Space Complexity

* **O(1)**

---

# Design Technique

**Divide and Conquer (Binary Search)**

Binary Search repeatedly divides the search space into two halves until the target is found or the insertion position is determined.

---

# Conclusion

The program successfully finds the position of a target element in a sorted array using Binary Search. If the target is not present, it correctly determines the index where the element should be inserted while maintaining the sorted order. This approach is efficient with a time complexity of **O(log n)**.
