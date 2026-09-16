# Practical 11: Remove K Digits

## 1. Aim

To write a program to remove exactly `K` digits from a given non-negative integer so that the resulting integer is the **smallest possible integer**, while preserving the relative order of the remaining digits.

---

## 2. Theory

Given a number represented as a string `num` and an integer `K`, we have to remove exactly `K` digits.

The order of the remaining digits cannot be changed.

### Example

```text
Input:
num = "1432219"
k = 3
```

We remove three digits optimally:

```text
1432219
 ↓
132219
 ↓
12219
 ↓
1219
```

Therefore:

```text
Output = 1219
```

### Important observations

To obtain the smallest possible number:

- If the current digit is smaller than the previous digit, removing the previous larger digit can make the number smaller.
- We can use a **stack** to keep the digits that are currently part of the smallest possible number.
- For every digit:

  - Compare it with the top digit of the stack.
  - If the top digit is greater and we still have digits to remove, remove the top digit.
  - Then insert the current digit.

- If `K` is still greater than zero after processing all digits, remove digits from the end.
- Finally, remove leading zeros.
- If no digits remain, return `"0"`.

---

# 3. Approach / Logic

The problem can be solved using a **Greedy Algorithm + Stack**.

Suppose:

```text
num = "1432219"
k = 3
```

We process the digits from left to right.

When we encounter a smaller digit after a larger digit, removing the larger digit gives a smaller number.

For example:

```text
4 3
```

Since `3 < 4`, removing `4` gives:

```text
3
```

which is smaller than:

```text
43
```

Therefore, whenever:

```text
stack.top() > current digit
```

and `k > 0`, we remove the top digit.

---

# 4. Algorithm

### Step 1

Read the number `num` as a string.

### Step 2

Read the number of digits to remove `k`.

### Step 3

Create an empty stack.

### Step 4

Traverse every digit of `num` from left to right.

### Step 5

For the current digit:

- While the stack is not empty.
- `k > 0`.
- The top digit of the stack is greater than the current digit.

Remove the top digit and decrease `k`.

### Step 6

Push the current digit into the stack.

### Step 7

After processing all digits, if `k > 0`, remove digits from the end of the stack.

### Step 8

Remove leading zeros from the resulting number.

### Step 9

If the result becomes empty, return:

```text
0
```

Otherwise, return the result.

---

# 5. Flowchart

```text
              START
                |
                v
       Read num and k
                |
                v
       Create empty stack
                |
                v
       Read next digit
                |
                v
     Is stack non-empty?
          /           \
        No             Yes
        |               |
        |               v
        |        Is k > 0 and
        |        top > digit?
        |             /    \
        |           Yes     No
        |            |       |
        |            v       |
        |       Remove top   |
        |       k = k - 1    |
        |            |        |
        |            +--------+
        |                 |
        v                 v
              Push current digit
                      |
                      v
              More digits?
                 /       \
               Yes        No
                |          |
                +----------+
                           |
                           v
                   Is k > 0?
                    /      \
                  Yes       No
                   |         |
                   v         |
              Remove from    |
                 end         |
                   |         |
                   +----+----+
                        |
                        v
                Remove leading
                    zeroes
                        |
                        v
              Is result empty?
                  /       \
                Yes        No
                 |          |
                 v          v
               Print 0   Print result
                  \         /
                   \       /
                     END
```

---

# 6. C++ Program

```cpp
#include <iostream>
#include <string>

using namespace std;

string removeKdigits(string num, int k)
{
    string st;

    // Process every digit
    for (char digit : num)
    {
        // Remove larger digits from the stack
        while (!st.empty() &&
               k > 0 &&
               st.back() > digit)
        {
            st.pop_back();
            k--;
        }

        // Add current digit
        st.push_back(digit);
    }

    // If digits are still left to remove,
    // remove them from the end
    while (k > 0 && !st.empty())
    {
        st.pop_back();
        k--;
    }

    // Remove leading zeros
    int start = 0;

    while (start < st.length() && st[start] == '0')
    {
        start++;
    }

    // If all digits are removed
    if (start == st.length())
    {
        return "0";
    }

    return st.substr(start);
}

int main()
{
    string num;
    int k;

    cout << "Enter the number: ";
    cin >> num;

    cout << "Enter number of digits to remove: ";
    cin >> k;

    string result = removeKdigits(num, k);

    cout << "Smallest possible integer = " << result << endl;

    return 0;
}
```

---

# 7. Java Program

```java
import java.util.*;

public class RemoveKDigits {

    static String removeKdigits(String num, int k) {

        StringBuilder stack = new StringBuilder();

        // Process every digit
        for (char digit : num.toCharArray()) {

            // Remove larger digits
            while (stack.length() > 0 &&
                   k > 0 &&
                   stack.charAt(stack.length() - 1) > digit) {

                stack.deleteCharAt(stack.length() - 1);
                k--;
            }

            // Add current digit
            stack.append(digit);
        }

        // If digits are still left to remove,
        // remove them from the end
        while (k > 0 && stack.length() > 0) {
            stack.deleteCharAt(stack.length() - 1);
            k--;
        }

        // Remove leading zeros
        int start = 0;

        while (start < stack.length() &&
               stack.charAt(start) == '0') {

            start++;
        }

        // If all digits are removed
        if (start == stack.length()) {
            return "0";
        }

        return stack.substring(start);
    }

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        System.out.print("Enter the number: ");
        String num = sc.next();

        System.out.print("Enter number of digits to remove: ");
        int k = sc.nextInt();

        String result = removeKdigits(num, k);

        System.out.println(
            "Smallest possible integer = " + result
        );

        sc.close();
    }
}
```

---

# 8. Sample Output

### Example 1

```text
Enter the number: 1432219
Enter number of digits to remove: 3

Smallest possible integer = 1219
```

### Example 2

```text
Enter the number: 10200
Enter number of digits to remove: 1

Smallest possible integer = 200
```

### Example 3

```text
Enter the number: 10
Enter number of digits to remove: 2

Smallest possible integer = 0
```

### Example 4

```text
Enter the number: 12345
Enter number of digits to remove: 2

Smallest possible integer = 123
```

---

# 9. Dry Run

Consider:

```text
num = "10200"
k = 1
```

We need to remove exactly one digit.

| Current Digit | Stack Before | Action              | Stack After |   k |
| ------------- | ------------ | ------------------- | ----------- | --: |
| `1`           | Empty        | Push `1`            | `1`         |   1 |
| `0`           | `1`          | `1 > 0`, remove `1` | `0`         |   0 |
| `2`           | `0`          | Push `2`            | `02`        |   0 |
| `0`           | `02`         | Push `0`            | `020`       |   0 |
| `0`           | `020`        | Push `0`            | `0200`      |   0 |

Now remove leading zeros:

```text
0200
 ↓
200
```

Therefore:

```text
Smallest possible integer = 200
```

### Why not `1000`?

If we remove `2`:

```text
10200
  ↓
1000
```

But if we remove `1`:

```text
10200
 ↓
0200
```

After removing the leading zero:

```text
0200 → 200
```

And:

```text
200 < 1000
```

Therefore, the correct answer is:

```text
200
```

---

# 10. Important Test Cases

| Input Number |   K | Output |
| ------------ | --: | ------ |
| `1432219`    |   3 | `1219` |
| `10200`      |   1 | `200`  |
| `10`         |   2 | `0`    |
| `12345`      |   2 | `123`  |
| `10001`      |   1 | `1`    |
| `7654321`    |   3 | `4321` |
| `112`        |   1 | `11`   |
| `9`          |   1 | `0`    |

---

# 11. Time Complexity

Let `n` be the number of digits in the input number.

Each digit is:

- pushed into the stack at most once
- removed from the stack at most once

Therefore:

```text
Time Complexity = O(n)
```

---

# 12. Space Complexity

The stack can contain up to `n` digits.

Therefore:

```text
Space Complexity = O(n)
```

---

# 13. Design Technique

**Greedy Algorithm + Stack**

### Greedy

At every step, we remove a larger previous digit when a smaller current digit is encountered.

### Stack

The stack maintains the digits that form the smallest possible number so far.

The combination allows us to solve the problem efficiently in `O(n)` time.

---

# 14. Conclusion

The **Remove K Digits** problem can be efficiently solved using a **Greedy Algorithm with a Stack**.

The algorithm removes larger digits whenever a smaller digit appears and then handles any remaining removals from the end. Finally, leading zeros are removed.

For example:

```text
Input:
10200
K = 1

Output:
200
```

Thus, the algorithm produces the smallest possible integer while preserving the order of the remaining digits.
