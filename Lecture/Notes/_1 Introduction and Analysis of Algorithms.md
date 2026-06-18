# Unit 1 – Introduction and Analysis of Algorithms

## Step 1: Algorithm and Algorithm Analysis

---

# 1. Algorithm

## Definition

An **algorithm** is a finite sequence of well-defined instructions used to solve a problem or perform a computation. It takes some input, processes it through a series of steps, and produces the desired output.

### Example

Problem: Find the sum of two numbers.

**Algorithm:**

1. Start
2. Read two numbers A and B
3. Compute Sum = A + B
4. Display Sum
5. Stop

---

## Characteristics (Properties) of an Algorithm

An algorithm must satisfy the following properties:

### 1. Input

An algorithm may have zero or more inputs.

**Example:**
To calculate the area of a circle, the radius is the input.

---

### 2. Output

An algorithm must produce at least one output.

**Example:**
Area of the circle is the output.

---

### 3. Definiteness

Each step must be clear and unambiguous.

**Correct:**

```
Add A and B
```

**Incorrect:**

```
Process the numbers somehow
```

The second statement is vague and cannot be executed properly.

---

### 4. Finiteness

The algorithm must terminate after a finite number of steps.

**Example:**
Finding the maximum element in an array of size n always finishes after examining n elements.

---

### 5. Effectiveness

Each instruction must be basic and executable within a finite amount of time.

**Example:**

```
A = A + 1
```

is effective because it can be executed directly.

---

### 6. Correctness

The algorithm must produce the correct result for all valid inputs.

---

### 7. Generality

The algorithm should solve a class of problems rather than a single instance.

**Example:**
An algorithm that sorts any list of numbers is more general than one that sorts only five numbers.

---

# 2. Why Study Algorithms?

Algorithms are important because they:

- Provide systematic solutions to problems.
- Improve efficiency.
- Reduce execution time.
- Reduce memory usage.
- Help compare different solutions.
- Form the basis of software development.

---

# 3. Types of Algorithms

Algorithms can be classified based on their design approach.

---

## 3.1 Brute Force Algorithm

A straightforward method that tries all possible solutions until the correct one is found.

### Characteristics

- Simple to design
- Easy to implement
- Usually inefficient for large inputs

### Example

Linear Search

Search every element one by one until the target is found.

---

## 3.2 Divide and Conquer

The problem is divided into smaller subproblems, solved independently, and combined.

### Steps

1. Divide
2. Conquer
3. Combine

### Examples

- Merge Sort
- Quick Sort
- Binary Search

---

## 3.3 Greedy Algorithm

Makes the best possible choice at each step hoping to achieve the overall optimal solution.

### Examples

- Kruskal's Algorithm
- Prim's Algorithm
- Dijkstra's Algorithm

### Advantages

- Fast
- Easy to implement

### Disadvantages

- May not always produce the optimal solution.

---

## 3.4 Dynamic Programming

Used when a problem contains:

- Overlapping subproblems
- Optimal substructure

Stores results of solved subproblems to avoid repeated calculations.

### Examples

- Fibonacci Series
- Knapsack Problem
- Matrix Chain Multiplication

---

## 3.5 Backtracking

Builds a solution step by step and abandons a path when it becomes invalid.

### Examples

- N-Queens Problem
- Sudoku Solver
- Graph Coloring

---

## 3.6 Branch and Bound

An optimization technique that eliminates non-promising solutions.

### Examples

- Travelling Salesman Problem
- 0/1 Knapsack Problem

---

## 3.7 Recursive Algorithm

An algorithm that calls itself until a base condition is reached.

### Example

Factorial

```
Factorial(n)
{
    if(n==0)
       return 1;
    return n * Factorial(n-1);
}
```

---

## 3.8 Iterative Algorithm

Uses loops instead of recursive calls.

### Example

Factorial using loop

```
fact = 1
for i = 1 to n
    fact = fact * i
```

---

# 4. Writing an Algorithm

There is no strict syntax for writing algorithms. However, algorithms should be:

- Clear
- Sequential
- Unambiguous
- Easy to understand

---

## General Format

```
Algorithm Name

Step 1: Start
Step 2: Input data
Step 3: Process data
Step 4: Produce output
Step 5: Stop
```

---

## Example: Largest of Two Numbers

```
Algorithm Largest

Step 1: Start
Step 2: Read A, B
Step 3: If A > B
            Print A
        Else
            Print B
Step 4: Stop
```

---

## Example: Sum of First N Natural Numbers

```
Algorithm Sum_N

Step 1: Start
Step 2: Read N
Step 3: Sum = 0
Step 4: For i = 1 to N
            Sum = Sum + i
Step 5: Print Sum
Step 6: Stop
```

---

# 5. Algorithm Analysis

## Definition

Algorithm analysis is the process of determining the resources required by an algorithm.

Resources mainly include:

1. Time
2. Memory

The objective is to determine how efficiently an algorithm performs as input size increases.

---

# 6. Parameters of Algorithm Analysis

---

## 6.1 Input Size (n)

The amount of data given to the algorithm.

### Examples

| Problem               | Input Size         |
| --------------------- | ------------------ |
| Array Search          | Number of elements |
| Matrix Multiplication | Matrix dimensions  |
| Sorting               | Number of elements |

Input size is generally represented by **n**.

---

## 6.2 Time Complexity

Measures the amount of time required by an algorithm as a function of input size.

### Example

```
for(i=1;i<=n;i++)
    print(i);
```

The loop executes n times.

Time Complexity:

```
T(n) = n
```

---

## 6.3 Space Complexity

Measures memory consumption.

### Example

```
int a[n];
```

Memory required:

```
O(n)
```

because n locations are allocated.

---

# 7. Need for Algorithm Analysis

Different algorithms may solve the same problem.

Example:

Searching:

1. Linear Search
2. Binary Search

Both solve the same problem but have different efficiencies.

Analysis helps determine:

- Faster algorithm
- Less memory usage
- Better scalability

---

# 8. Design Techniques of Algorithms

Algorithm design techniques provide systematic methods for solving problems.

---

## 8.1 Brute Force Technique

Try all possibilities.

### Example

Linear Search

Complexity:

```
O(n)
```

---

## 8.2 Divide and Conquer

Divide → Solve → Combine

### Example

Merge Sort

Complexity:

```
O(n log n)
```

---

## 8.3 Greedy Method

Choose the locally optimal solution.

### Example

Prim's Algorithm

---

## 8.4 Dynamic Programming

Store previously computed results.

### Example

Fibonacci Series

---

## 8.5 Backtracking

Generate solutions and discard invalid paths.

### Example

N-Queens

---

## 8.6 Branch and Bound

Eliminate non-promising branches.

### Example

Travelling Salesman Problem

---

## 8.7 Randomized Algorithms

Use random numbers during execution.

### Example

Randomized Quick Sort

---

# Comparison of Design Techniques

| Technique           | Main Idea               | Example           |
| ------------------- | ----------------------- | ----------------- |
| Brute Force         | Try all solutions       | Linear Search     |
| Divide and Conquer  | Divide into subproblems | Merge Sort        |
| Greedy              | Best local choice       | Prim's            |
| Dynamic Programming | Store results           | Fibonacci         |
| Backtracking        | Explore and backtrack   | N-Queens          |
| Branch and Bound    | Prune bad solutions     | TSP               |
| Randomized          | Use randomness          | Random Quick Sort |

---

# Key Points

- An algorithm is a finite sequence of instructions used to solve a problem.
- Important properties: Input, Output, Definiteness, Finiteness, Effectiveness, Correctness, Generality.
- Time complexity measures execution time growth.
- Space complexity measures memory usage.
- Input size is represented by **n**.
- Algorithm analysis helps compare efficiency.
- Major design techniques include Brute Force, Divide and Conquer, Greedy, Dynamic Programming, Backtracking, Branch and Bound, and Randomized methods.

---

### Important Exam Questions

1. Define algorithm and explain its characteristics.
2. Differentiate between time complexity and space complexity.
3. Explain various types of algorithms with examples.
4. What is algorithm analysis? Why is it necessary?
5. Explain various algorithm design techniques.
6. Write an algorithm to find the largest of two numbers.
7. Discuss parameters used in algorithm analysis.

---

# Step 2: Asymptotic Analysis

---

# 1. Introduction to Asymptotic Analysis

When analyzing an algorithm, the exact execution time is usually not important because it depends on factors such as:

- Processor speed
- Programming language
- Compiler optimization
- Operating system
- Hardware configuration

Instead of measuring actual running time, we analyze how the running time grows as the input size increases.

This method is called **Asymptotic Analysis**.

---

## Definition

**Asymptotic Analysis** is the study of an algorithm's performance as the input size (n) approaches infinity.

It describes the growth rate of time complexity or space complexity while ignoring:

- Constant factors
- Lower-order terms

---

## Example

Consider:

[
T(n)=3n^2+5n+10
]

For very large values of (n):

- (3n^2) dominates
- (5n) becomes insignificant
- (10) becomes negligible

Therefore,

[
T(n)=O(n^2)
]

The asymptotic complexity depends on the highest-order term.

---

# Why Asymptotic Analysis is Needed

It helps:

- Compare algorithms independently of hardware.
- Predict performance for large inputs.
- Select the most efficient algorithm.
- Estimate scalability.

---

# 2. Growth of Common Functions

| Complexity   | Name         |
| ------------ | ------------ |
| (O(1))       | Constant     |
| (O(\log n))  | Logarithmic  |
| (O(n))       | Linear       |
| (O(n\log n)) | Linearithmic |
| (O(n^2))     | Quadratic    |
| (O(n^3))     | Cubic        |
| (O(2^n))     | Exponential  |
| (O(n!))      | Factorial    |

---

## Increasing Order of Growth

[
O(1)
<
O(\log n)
<
O(n)
<
O(n\log n)
<
O(n^2)
<
O(n^3)
<
O(2^n)
<
O(n!)
]

Algorithms with smaller growth rates are generally more efficient.

---

# 3. Big O Notation (Upper Bound)

## Definition

Big O notation describes the **maximum growth rate** of an algorithm.

It gives an upper bound on running time.

---

## Formal Definition

A function (f(n)) belongs to (O(g(n))) if there exist positive constants (c) and (n_0) such that:

[
0 \leq f(n) \leq c,g(n)
]

for all

[
n \geq n_0
]

---

## Meaning

Big O answers:

> "At most how much time can the algorithm take?"

---

## Example 1

[
f(n)=5n+10
]

For large (n):

[
5n+10 \leq 6n
]

Thus,

[
f(n)=O(n)
]

---

## Example 2

[
f(n)=4n^2+7n+3
]

Dominant term:

[
4n^2
]

Therefore,

[
f(n)=O(n^2)
]

---

## Example 3

```c
for(i=1;i<=n;i++)
{
    printf("%d",i);
}
```

Loop executes (n) times.

[
T(n)=n
]

Therefore,

[
O(n)
]

---

# Characteristics of Big O

- Represents worst-case growth.
- Gives upper limit of performance.
- Most widely used notation.
- Useful for comparing algorithms.

---

# 4. Big Omega Notation (Lower Bound)

## Definition

Big Omega notation describes the minimum growth rate of an algorithm.

It provides a lower bound.

---

## Formal Definition

A function (f(n)) belongs to (\Omega(g(n))) if there exist positive constants (c) and (n_0) such that:

[
f(n)\geq c,g(n)
]

for all

[
n\geq n_0
]

---

## Meaning

Big Omega answers:

> "At least how much time is required?"

---

## Example 1

[
f(n)=3n^2+2n+1
]

Since

[
f(n)\geq 3n^2
]

Therefore,

[
f(n)=\Omega(n^2)
]

---

## Example 2

Linear Search (best case)

Target element found at first position.

Only one comparison needed.

[
\Omega(1)
]

---

# Characteristics of Big Omega

- Describes best possible growth.
- Provides minimum running time.
- Gives lower limit.

---

# 5. Big Theta Notation (Tight Bound)

## Definition

Big Theta notation provides both upper and lower bounds simultaneously.

---

## Formal Definition

A function (f(n)) belongs to (\Theta(g(n))) if there exist positive constants (c_1,c_2,n_0) such that:

[
c_1g(n)\leq f(n)\leq c_2g(n)
]

for all

[
n\geq n_0
]

---

## Meaning

Big Theta answers:

> "Exactly how fast does the algorithm grow?"

---

## Example

[
f(n)=4n^2+3n+7
]

For large (n),

[
f(n)
]

grows proportionally to

[
n^2
]

Therefore,

[
f(n)=\Theta(n^2)
]

---

## Example

```c
for(i=1;i<=n;i++)
{
    printf("%d",i);
}
```

Execution count:

[
n
]

Hence,

[
\Theta(n)
]

---

# Relationship Among O, Ω and Θ

If

[
f(n)=\Theta(g(n))
]

then

[
f(n)=O(g(n))
]

and

[
f(n)=\Omega(g(n))
]

also hold.

---

## Visualization

```text
Lower Bound ≤ Actual Growth ≤ Upper Bound

Ω(g(n)) ≤ f(n) ≤ O(g(n))

If both are equal:

f(n) = Θ(g(n))
```

---

# 6. Upper Bound, Lower Bound and Tight Bound

---

## Upper Bound

Maximum growth rate.

Represented by:

[
O(g(n))
]

Example:

[
4n^2+5n+1=O(n^2)
]

---

## Lower Bound

Minimum growth rate.

Represented by:

[
\Omega(g(n))
]

Example:

[
4n^2+5n+1=\Omega(n^2)
]

---

## Tight Bound

Exact growth rate.

Represented by:

[
\Theta(g(n))
]

Example:

[
4n^2+5n+1=\Theta(n^2)
]

---

# Comparison

| Bound       | Notation | Meaning        |
| ----------- | -------- | -------------- |
| Upper Bound | O        | Maximum growth |
| Lower Bound | Ω        | Minimum growth |
| Tight Bound | Θ        | Exact growth   |

---

# 7. Best Case Analysis

## Definition

Best case refers to the minimum time required by an algorithm.

It occurs when the input is in the most favorable condition.

---

## Example: Linear Search

Array:

```text
10 20 30 40 50
```

Search:

```text
10
```

Found immediately.

Comparisons:

[
1
]

Complexity:

[
O(1)
]

---

# Best Case Formula

[
T_{best}(n)
]

represents minimum running time.

---

# 8. Worst Case Analysis

## Definition

Worst case refers to the maximum time required by an algorithm.

It occurs when the input is in the least favorable condition.

---

## Example: Linear Search

Array:

```text
10 20 30 40 50
```

Search:

```text
50
```

or not present.

Comparisons:

[
n
]

Complexity:

[
O(n)
]

---

# Worst Case Formula

[
T_{worst}(n)
]

represents maximum running time.

---

# 9. Average Case Analysis

## Definition

Average case represents expected running time over all possible inputs.

---

## Formula

[
T\_{avg}(n)
==========

\frac{\sum T(i)}{\text{Number of Inputs}}
]

---

## Example: Linear Search

Element may occur anywhere.

Average comparisons:

[
\frac{n+1}{2}
]

Complexity:

[
O(n)
]

---

# Comparison of Cases

| Case         | Meaning               |
| ------------ | --------------------- |
| Best Case    | Minimum running time  |
| Average Case | Expected running time |
| Worst Case   | Maximum running time  |

---

# Example: Linear Search

```c
for(i=0;i<n;i++)
{
   if(arr[i]==key)
      return i;
}
```

| Situation            | Complexity |
| -------------------- | ---------- |
| First element found  | O(1)       |
| Middle element found | O(n)       |
| Last element found   | O(n)       |
| Element absent       | O(n)       |

---

# Example: Insertion Sort

| Case    | Complexity |
| ------- | ---------- |
| Best    | O(n)       |
| Average | O(n²)      |
| Worst   | O(n²)      |

---

# Example: Bubble Sort

| Case             | Complexity |
| ---------------- | ---------- |
| Best (optimized) | O(n)       |
| Average          | O(n²)      |
| Worst            | O(n²)      |

---

# Example: Binary Search

| Case    | Complexity |
| ------- | ---------- |
| Best    | O(1)       |
| Average | O(log n)   |
| Worst   | O(log n)   |

---

# Finding Complexity of Simple Statements

---

## Constant Statement

```c
x = 5;
```

Complexity:

[
O(1)
]

---

## Single Loop

```c
for(i=1;i<=n;i++)
{
   printf("%d",i);
}
```

Complexity:

[
O(n)
]

---

## Nested Loop

```c
for(i=1;i<=n;i++)
{
   for(j=1;j<=n;j++)
   {
      printf("*");
   }
}
```

Complexity:

[
O(n^2)
]

---

## Triple Nested Loop

```c
for(i=1;i<=n;i++)
{
   for(j=1;j<=n;j++)
   {
      for(k=1;k<=n;k++)
      {
          printf("*");
      }
   }
}
```

Complexity:

[
O(n^3)
]

---

## Halving Loop

```c
while(n>1)
{
   n=n/2;
}
```

Complexity:

[
O(\log n)
]

---

# Key Points

- Asymptotic analysis studies growth of running time for large inputs.
- Big O gives an upper bound.
- Big Omega gives a lower bound.
- Big Theta gives a tight bound.
- Best case = minimum time.
- Worst case = maximum time.
- Average case = expected time.
- Constants and lower-order terms are ignored.
- Complexity comparison helps select efficient algorithms.

---

# Important Exam Questions

1. Define asymptotic analysis and explain its significance.
2. Explain Big O notation with examples.
3. Explain Big Omega notation with examples.
4. Explain Big Theta notation with examples.
5. Differentiate between Big O, Big Ω and Big Θ.
6. What are upper bound, lower bound and tight bound?
7. Differentiate between best case, worst case and average case analysis.
8. Find the time complexity of single, nested and logarithmic loops.
9. Arrange common complexity classes in increasing order of growth.
10. Explain asymptotic notations using suitable examples.
