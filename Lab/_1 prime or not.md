# Experiment 1: Program to Determine Whether a Given Number is Prime or Not

## Aim

To write a program to determine whether a given number is Prime or Not.

---

# Theory

A **Prime Number** is a natural number greater than 1 that has exactly two factors:

1. 1
2. Itself

Examples:

* Prime Numbers: 2, 3, 5, 7, 11, 13
* Non-Prime Numbers: 4, 6, 8, 9, 10

### Logic

* If the number is less than or equal to 1, it is not prime.
* Check divisibility from 2 up to √n.
* If any number divides n exactly, then n is not prime.
* Otherwise, n is prime.

---

# Algorithm

### Algorithm: Check Prime Number

**Step 1:** Start

**Step 2:** Input number `n`

**Step 3:** If `n <= 1`

* Display "Not Prime"
* Go to Step 8

**Step 4:** Set `isPrime = true`

**Step 5:** Repeat for `i = 2` to `√n`

* If `n % i == 0`

  * Set `isPrime = false`
  * Break the loop

**Step 6:** If `isPrime == true`

* Display "Prime Number"

**Step 7:** Else

* Display "Not Prime Number"

**Step 8:** Stop

---

# Flowchart

```text
 ┌─────────┐
 │  Start  │
 └────┬────┘
      │
      ▼
 ┌─────────────┐
 │ Input n     │
 └────┬────────┘
      │
      ▼
 ┌─────────────┐
 │ n <= 1 ?    │
 └───┬─────┬───┘
     │Yes  │No
     ▼     ▼
┌────────┐ ┌─────────────────┐
│Not     │ │ isPrime = true  │
│Prime   │ └───────┬─────────┘
└────┬───┘         │
     │             ▼
     │      ┌──────────────┐
     │      │ i=2 to √n    │
     │      └──────┬───────┘
     │             │
     │             ▼
     │      ┌──────────────┐
     │      │ n%i == 0 ?   │
     │      └───┬─────┬────┘
     │          │Yes  │No
     │          ▼     │
     │   ┌──────────┐ │
     │   │isPrime = │ │
     │   │ false    │ │
     │   └────┬─────┘ │
     │        │       │
     └────────┼───────┘
              ▼
      ┌───────────────┐
      │ isPrime ?     │
      └───┬─────┬─────┘
          │Yes  │No
          ▼     ▼
   ┌─────────┐ ┌──────────┐
   │ Prime   │ │ Not Prime│
   └────┬────┘ └────┬─────┘
        │           │
        └─────┬─────┘
              ▼
         ┌────────┐
         │  Stop  │
         └────────┘
```

---

# Java program

```Java
import java.util.Scanner;

public class PrimeNumber {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.print("Enter a number: ");
        int n = sc.nextInt();

        boolean isPrime = true;

        if (n <= 1) {
            isPrime = false;
        } else {
            for (int i = 2; i <= Math.sqrt(n); i++) {
                if (n % i == 0) {
                    isPrime = false;
                    break;
                }
            }
        }

        if (isPrime) {
            System.out.println(n + " is a Prime Number.");
        } else {
            System.out.println(n + " is Not a Prime Number.");
        }

        sc.close();
    }
}
```

# C++ Program

```cpp
#include <iostream>
#include <cmath>
using namespace std;

int main()
{
    int n;
    bool isPrime = true;

    cout << "Enter a number: ";
    cin >> n;

    if (n <= 1)
    {
        isPrime = false;
    }
    else
    {
        for (int i = 2; i <= sqrt(n); i++)
        {
            if (n % i == 0)
            {
                isPrime = false;
                break;
            }
        }
    }

    if (isPrime)
        cout << n << " is a Prime Number.";
    else
        cout << n << " is Not a Prime Number.";

    return 0;
}
```

---

# Sample Output 1

```text
Enter a number: 13
13 is a Prime Number.
```

# Sample Output 2

```text
Enter a number: 15
15 is Not a Prime Number.
```

---

# Time Complexity

* **O(√n)**

# Space Complexity

* **O(1)**

---

# Conclusion

The program successfully checks whether a given number is prime or not by testing divisibility from 2 to √n. This approach is efficient and reduces the number of unnecessary iterations.
