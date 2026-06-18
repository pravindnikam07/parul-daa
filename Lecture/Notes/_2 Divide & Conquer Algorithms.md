# Unit 2 – Divide and Conquer Algorithms

---

# 1. Introduction to Divide and Conquer

## Definition

**Divide and Conquer** is an algorithm design technique in which a problem is divided into smaller subproblems of the same type, each subproblem is solved independently, and the solutions are combined to obtain the final solution.

This technique is widely used because it often reduces time complexity and improves efficiency.

---

# Basic Idea

A Divide and Conquer algorithm follows three steps:

## 1. Divide

Break the problem into smaller subproblems.

## 2. Conquer

Solve the subproblems recursively.

## 3. Combine

Merge the solutions of the subproblems to obtain the final solution.

---

## General Structure

```text
DivideAndConquer(problem P)

1. If P is small enough
       Solve directly
2. Divide P into smaller subproblems
3. Recursively solve each subproblem
4. Combine their solutions
5. Return result
```

---

# Generic Recurrence Relation

Most Divide and Conquer algorithms follow:

[
T(n)=aT\left(\frac{n}{b}\right)+f(n)
]

Where:

- (a) = Number of subproblems
- (n/b) = Size of each subproblem
- (f(n)) = Cost of dividing and combining

---

## Advantages

- Efficient for large inputs
- Reduces complexity
- Suitable for recursion
- Easy parallelization

---

## Disadvantages

- Recursive overhead
- Additional memory may be required
- Sometimes difficult to implement

---

# 2. Binary Search

## Introduction

Binary Search is used to search an element in a **sorted array**.

Instead of checking every element, it repeatedly divides the search space into two halves.

---

## Working Principle

1. Find middle element.
2. Compare key with middle.
3. If equal → found.
4. If smaller → search left half.
5. If larger → search right half.
6. Repeat until found or search space becomes empty.

---

## Example

Array:

```text
10 20 30 40 50 60 70
```

Search Key:

```text
50
```

### Iteration 1

```text
Middle = 40
```

50 > 40

Search right half.

### Iteration 2

```text
Middle = 60
```

50 < 60

Search left half.

### Iteration 3

```text
Middle = 50
```

Element found.

---

# Algorithm

```text
BinarySearch(A, low, high, key)

if(low > high)
    return -1

mid = (low + high)/2

if(A[mid] == key)
    return mid

if(key < A[mid])
    BinarySearch(A, low, mid-1, key)

else
    BinarySearch(A, mid+1, high, key)
```

---

# C++ Program

```cpp
#include <iostream>
using namespace std;

int binarySearch(int arr[], int low, int high, int key)
{
    if(low > high)
        return -1;

    int mid = (low + high) / 2;

    if(arr[mid] == key)
        return mid;

    if(key < arr[mid])
        return binarySearch(arr, low, mid - 1, key);

    return binarySearch(arr, mid + 1, high, key);
}

int main()
{
    int arr[] = {10,20,30,40,50,60,70};
    int n = 7;
    int key = 50;

    int pos = binarySearch(arr,0,n-1,key);

    if(pos != -1)
        cout<<"Element Found at Index "<<pos;
    else
        cout<<"Element Not Found";

    return 0;
}
```

---

# Java Program

```java
class BinarySearch
{
    static int search(int arr[], int low, int high, int key)
    {
        if(low > high)
            return -1;

        int mid = (low + high) / 2;

        if(arr[mid] == key)
            return mid;

        if(key < arr[mid])
            return search(arr, low, mid - 1, key);

        return search(arr, mid + 1, high, key);
    }

    public static void main(String args[])
    {
        int arr[] = {10,20,30,40,50,60,70};

        int result = search(arr,0,arr.length-1,50);

        if(result != -1)
            System.out.println("Element Found at Index "+result);
        else
            System.out.println("Element Not Found");
    }
}
```

---

# Complexity Analysis

Recurrence:

[
T(n)=T(n/2)+1
]

Using Master Theorem:

[
T(n)=O(\log n)
]

### Complexity

| Case    | Complexity |
| ------- | ---------- |
| Best    | O(1)       |
| Average | O(log n)   |
| Worst   | O(log n)   |

Space Complexity:

[
O(\log n)
]

---

# 3. Merge Sort

## Introduction

Merge Sort repeatedly divides the array into two halves until individual elements remain, then merges them in sorted order.

---

## Working

Array:

```text
38 27 43 3 9 82 10
```

### Divide Phase

```text
38 27 43 3 9 82 10

38 27 43 3      9 82 10

38 27   43 3    9 82   10

38 27   43   3  9   82  10
```

---

### Merge Phase

```text
27 38

3 43

9 82

3 27 38 43

9 10 82

3 9 10 27 38 43 82
```

---

# Algorithm

```text
MergeSort(A,l,r)

if(l < r)

    mid=(l+r)/2

    MergeSort(A,l,mid)

    MergeSort(A,mid+1,r)

    Merge(A,l,mid,r)
```

---

# C++ Program

```cpp
#include <iostream>
using namespace std;

void merge(int arr[], int l, int m, int r)
{
    int n1 = m-l+1;
    int n2 = r-m;

    int L[n1], R[n2];

    for(int i=0;i<n1;i++)
        L[i]=arr[l+i];

    for(int j=0;j<n2;j++)
        R[j]=arr[m+1+j];

    int i=0,j=0,k=l;

    while(i<n1 && j<n2)
    {
        if(L[i]<=R[j])
            arr[k++]=L[i++];
        else
            arr[k++]=R[j++];
    }

    while(i<n1)
        arr[k++]=L[i++];

    while(j<n2)
        arr[k++]=R[j++];
}

void mergeSort(int arr[], int l, int r)
{
    if(l<r)
    {
        int m=(l+r)/2;

        mergeSort(arr,l,m);
        mergeSort(arr,m+1,r);

        merge(arr,l,m,r);
    }
}

int main()
{
    int arr[]={38,27,43,3,9,82,10};
    int n=7;

    mergeSort(arr,0,n-1);

    for(int i=0;i<n;i++)
        cout<<arr[i]<<" ";

    return 0;
}
```

---

# Java Program

```java
class MergeSort
{
    void merge(int arr[], int l, int m, int r)
    {
        int n1 = m-l+1;
        int n2 = r-m;

        int L[] = new int[n1];
        int R[] = new int[n2];

        for(int i=0;i<n1;i++)
            L[i]=arr[l+i];

        for(int j=0;j<n2;j++)
            R[j]=arr[m+1+j];

        int i=0,j=0,k=l;

        while(i<n1 && j<n2)
        {
            if(L[i]<=R[j])
                arr[k++]=L[i++];
            else
                arr[k++]=R[j++];
        }

        while(i<n1)
            arr[k++]=L[i++];

        while(j<n2)
            arr[k++]=R[j++];
    }

    void mergeSort(int arr[], int l, int r)
    {
        if(l<r)
        {
            int m=(l+r)/2;

            mergeSort(arr,l,m);
            mergeSort(arr,m+1,r);

            merge(arr,l,m,r);
        }
    }
}
```

---

# Complexity Analysis

Recurrence:

[
T(n)=2T(n/2)+n
]

Using Master Theorem:

[
T(n)=O(n\log n)
]

### Complexity

| Case    | Complexity |
| ------- | ---------- |
| Best    | O(n log n) |
| Average | O(n log n) |
| Worst   | O(n log n) |

Space Complexity:

[
O(n)
]

---

# 4. Quick Sort

## Introduction

Quick Sort selects a pivot element and partitions the array into:

- Smaller elements
- Pivot
- Larger elements

The process is recursively repeated.

---

## Example

Array:

```text
50 20 60 10 80 30
```

Pivot = 50

Partition:

```text
20 10 30 | 50 | 60 80
```

Sort left and right parts recursively.

Result:

```text
10 20 30 50 60 80
```

---

# Algorithm

```text
QuickSort(A,low,high)

if(low < high)

    p = Partition(A,low,high)

    QuickSort(A,low,p-1)

    QuickSort(A,p+1,high)
```

---

# CPP Program

```cpp
#include <iostream>
using namespace std;

int partition(int arr[], int low, int high)
{
    int pivot = arr[high];
    int i = low - 1;

    for(int j = low; j < high; j++)
    {
        if(arr[j] < pivot)
        {
            i++;
            swap(arr[i], arr[j]);
        }
    }

    swap(arr[i + 1], arr[high]);

    return i + 1;
}

void quickSort(int arr[], int low, int high)
{
    if(low < high)
    {
        int pi = partition(arr, low, high);

        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

int main()
{
    int arr[] = {50,20,60,10,80,30};
    int n = sizeof(arr)/sizeof(arr[0]);

    quickSort(arr,0,n-1);

    cout<<"Sorted Array: ";

    for(int i=0;i<n;i++)
        cout<<arr[i]<<" ";

    return 0;
}
```

# Java Program

```java
class QuickSort
{
    int partition(int arr[], int low, int high)
    {
        int pivot = arr[high];
        int i = low - 1;

        for(int j=low;j<high;j++)
        {
            if(arr[j] < pivot)
            {
                i++;

                int temp = arr[i];
                arr[i] = arr[j];
                arr[j] = temp;
            }
        }

        int temp = arr[i+1];
        arr[i+1] = arr[high];
        arr[high] = temp;

        return i+1;
    }

    void quickSort(int arr[], int low, int high)
    {
        if(low < high)
        {
            int pi = partition(arr, low, high);

            quickSort(arr, low, pi-1);
            quickSort(arr, pi+1, high);
        }
    }

    public static void main(String args[])
    {
        int arr[] = {50,20,60,10,80,30};

        QuickSort qs = new QuickSort();

        qs.quickSort(arr,0,arr.length-1);

        System.out.println("Sorted Array:");

        for(int x : arr)
            System.out.print(x+" ");
    }
}
```

# Complexity Analysis

Best/Average:

[
T(n)=2T(n/2)+n
]

[
O(n\log n)
]

Worst Case:

[
T(n)=T(n-1)+n
]

[
O(n^2)
]

---

| Case    | Complexity |
| ------- | ---------- |
| Best    | O(n log n) |
| Average | O(n log n) |
| Worst   | O(n²)      |

Space Complexity:

[
O(\log n)
]

---

# 5. Strassen Matrix Multiplication

## Introduction

Traditional multiplication of two (n \times n) matrices requires:

[
O(n^3)
]

operations.

Strassen's algorithm reduces this complexity using Divide and Conquer.

---

## Idea

Divide matrices into four submatrices:

[
A=
\begin{bmatrix}
A_{11} & A_{12}\
A_{21} & A_{22}
\end{bmatrix}
]

[
B=
\begin{bmatrix}
B_{11} & B_{12}\
B_{21} & B_{22}
\end{bmatrix}
]

Instead of 8 multiplications, Strassen uses only 7 multiplications.

---

## Seven Products

[
M_1=(A_{11}+A_{22})(B_{11}+B_{22})
]

[
M_2=(A_{21}+A_{22})B_{11}
]

[
M_3=A_{11}(B_{12}-B_{22})
]

[
M_4=A_{22}(B_{21}-B_{11})
]

[
M_5=(A_{11}+A_{12})B_{22}
]

[
M_6=(A_{21}-A_{11})(B_{11}+B_{12})
]

[
M_7=(A_{12}-A_{22})(B_{21}+B_{22})
]

---

# CPP Program

```cpp
#include <iostream>
using namespace std;

int main()
{
    int A[2][2] = {{1,2},{3,4}};
    int B[2][2] = {{5,6},{7,8}};

    int M1,M2,M3,M4,M5,M6,M7;

    M1=(A[0][0]+A[1][1])*(B[0][0]+B[1][1]);
    M2=(A[1][0]+A[1][1])*B[0][0];
    M3=A[0][0]*(B[0][1]-B[1][1]);
    M4=A[1][1]*(B[1][0]-B[0][0]);
    M5=(A[0][0]+A[0][1])*B[1][1];
    M6=(A[1][0]-A[0][0])*(B[0][0]+B[0][1]);
    M7=(A[0][1]-A[1][1])*(B[1][0]+B[1][1]);

    int C[2][2];

    C[0][0]=M1+M4-M5+M7;
    C[0][1]=M3+M5;
    C[1][0]=M2+M4;
    C[1][1]=M1-M2+M3+M6;

    cout<<"Result Matrix:\n";

    for(int i=0;i<2;i++)
    {
        for(int j=0;j<2;j++)
            cout<<C[i][j]<<" ";

        cout<<endl;
    }

    return 0;
}
```

# Java Program

```java
#include <iostream>
using namespace std;

int main()
{
    int A[2][2] = {{1,2},{3,4}};
    int B[2][2] = {{5,6},{7,8}};

    int M1,M2,M3,M4,M5,M6,M7;

    M1=(A[0][0]+A[1][1])*(B[0][0]+B[1][1]);
    M2=(A[1][0]+A[1][1])*B[0][0];
    M3=A[0][0]*(B[0][1]-B[1][1]);
    M4=A[1][1]*(B[1][0]-B[0][0]);
    M5=(A[0][0]+A[0][1])*B[1][1];
    M6=(A[1][0]-A[0][0])*(B[0][0]+B[0][1]);
    M7=(A[0][1]-A[1][1])*(B[1][0]+B[1][1]);

    int C[2][2];

    C[0][0]=M1+M4-M5+M7;
    C[0][1]=M3+M5;
    C[1][0]=M2+M4;
    C[1][1]=M1-M2+M3+M6;

    cout<<"Result Matrix:\n";

    for(int i=0;i<2;i++)
    {
        for(int j=0;j<2;j++)
            cout<<C[i][j]<<" ";

        cout<<endl;
    }

    return 0;
}
```

## Complexity

Recurrence:

[
T(n)=7T(n/2)+O(n^2)
]

Using Master Theorem:

[
T(n)=O(n^{2.807})
]

---

# Comparison

| Method                     | Complexity |
| -------------------------- | ---------- |
| Traditional Multiplication | O(n³)      |
| Strassen Multiplication    | O(n²·⁸⁰⁷)  |

---

# 6. Max-Min Problem

## Problem Statement

Find both maximum and minimum elements of an array using Divide and Conquer.

---

## Brute Force Approach

Separate search for max and min:

[
2(n-1)
]

comparisons.

---

## Divide and Conquer Approach

1. Divide array into two halves.
2. Find max and min in each half.
3. Compare results.
4. Return overall max and min.

---

## Algorithm

```text
MaxMin(arr,l,r)

if l==r
    max=min=arr[l]

if r==l+1

    compare both elements

else

    mid=(l+r)/2

    MaxMin(left)

    MaxMin(right)

    combine results
```

---

# C++ Structure

```cpp
struct Pair
{
    int min;
    int max;
};
```

---

# CPP Program

```cpp
#include <iostream>
using namespace std;

struct Pair
{
    int min;
    int max;
};

Pair getMinMax(int arr[], int low, int high)
{
    Pair result, left, right;

    if(low == high)
    {
        result.min = result.max = arr[low];
        return result;
    }

    if(high == low + 1)
    {
        if(arr[low] < arr[high])
        {
            result.min = arr[low];
            result.max = arr[high];
        }
        else
        {
            result.min = arr[high];
            result.max = arr[low];
        }

        return result;
    }

    int mid = (low + high)/2;

    left = getMinMax(arr, low, mid);
    right = getMinMax(arr, mid+1, high);

    result.min = (left.min < right.min)
                    ? left.min : right.min;

    result.max = (left.max > right.max)
                    ? left.max : right.max;

    return result;
}

int main()
{
    int arr[] = {70,11,85,42,5,95,21};
    int n = sizeof(arr)/sizeof(arr[0]);

    Pair ans = getMinMax(arr,0,n-1);

    cout<<"Minimum = "<<ans.min<<endl;
    cout<<"Maximum = "<<ans.max;

    return 0;
}
```

---

# Java Program

```java
class MaxMin
{
    static class Pair
    {
        int min;
        int max;
    }

    static Pair getMinMax(int arr[], int low, int high)
    {
        Pair result = new Pair();

        if(low == high)
        {
            result.min = result.max = arr[low];
            return result;
        }

        if(high == low + 1)
        {
            if(arr[low] < arr[high])
            {
                result.min = arr[low];
                result.max = arr[high];
            }
            else
            {
                result.min = arr[high];
                result.max = arr[low];
            }

            return result;
        }

        int mid = (low + high)/2;

        Pair left = getMinMax(arr, low, mid);
        Pair right = getMinMax(arr, mid+1, high);

        result.min = Math.min(left.min, right.min);
        result.max = Math.max(left.max, right.max);

        return result;
    }

    public static void main(String args[])
    {
        int arr[] = {70,11,85,42,5,95,21};

        Pair ans = getMinMax(arr,0,arr.length-1);

        System.out.println("Minimum = "+ans.min);
        System.out.println("Maximum = "+ans.max);
    }
}
```

---

# Complexity

Recurrence:

[
T(n)=2T(n/2)+2
]

Applying Master Theorem:

[
T(n)=O(n)
]

Number of comparisons:

[
\frac{3n}{2}-2
]

which is better than:

[
2n-2
]

comparisons of the brute-force approach.

---

# Comparison of Divide and Conquer Algorithms

| Algorithm               | Recurrence      | Time Complexity |
| ----------------------- | --------------- | --------------- |
| Binary Search           | T(n)=T(n/2)+1   | O(log n)        |
| Merge Sort              | T(n)=2T(n/2)+n  | O(n log n)      |
| Quick Sort (Average)    | T(n)=2T(n/2)+n  | O(n log n)      |
| Quick Sort (Worst)      | T(n)=T(n−1)+n   | O(n²)           |
| Strassen Multiplication | T(n)=7T(n/2)+n² | O(n²·⁸⁰⁷)       |
| Max-Min Problem         | T(n)=2T(n/2)+2  | O(n)            |

---

# Important Exam Questions

1. Explain the Divide and Conquer strategy with a general structure.
2. Explain Binary Search using Divide and Conquer and analyze its complexity.
3. Explain Merge Sort with suitable example and recurrence relation.
4. Explain Quick Sort and derive its best, average, and worst-case complexity.
5. What is Strassen Matrix Multiplication? How is it better than conventional multiplication?
6. Solve the Max-Min problem using Divide and Conquer.
7. Compare Merge Sort and Quick Sort.
8. Derive recurrence relations for Binary Search, Merge Sort, and Quick Sort.
9. Explain the advantages and disadvantages of Divide and Conquer.
10. Apply Master Theorem to analyze Merge Sort and Strassen Multiplication.
