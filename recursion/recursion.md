# Recursion

## One-sentence definition
Recursion solves a problem by calling the same function on smaller subproblems until a base case is reached.

## When to use it (use cases)
- Tree traversal and tree-based computations.
- Backtracking problems (subsets, permutations, combinations).
- Divide-and-conquer algorithms (merge sort, quick sort patterns).
- Problems with a clear recurrence relation.

## Step-by-step explanation
1. Define a base case that returns directly.
2. Define a recursive case that reduces the problem size.
3. Ensure every recursive call moves toward the base case.
4. Combine returned results as the call stack unwinds.
5. Validate termination to avoid infinite recursion.

## Visual intuition (ASCII)

### Factorial call stack (`fact(4)`)
```text
fact(4)
 -> 4 * fact(3)
      -> 3 * fact(2)
           -> 2 * fact(1)
                -> 1  (base case)

Unwind:
fact(2)=2, fact(3)=6, fact(4)=24
```

### Binary recursion shape
```text
               solve(node)
               /        \
      solve(left)    solve(right)
```

## Templates (Python)
### 1) Generic recursion skeleton
Use when: any problem can be reduced to a smaller same-type state.
Input: current state.
Output: solved value for that state.
```python
def solve(state):
    # Base case (must terminate).
    if is_base(state):
        return base_answer(state)

    # Recursive case (must shrink state).
    next_state = reduce_state(state)
    sub = solve(next_state)
    return combine(state, sub)
```

### 2) Simple single-branch recursion (factorial)
Use when: one recursive subproblem per call.
Input: non-negative integer `n`.
Output: `n!`.
```python
def factorial(n):
    # TC: O(n), SC: O(n) due to recursion stack.
    if n <= 1:
        return 1
    return n * factorial(n - 1)
```

### 3) Tree recursion (preorder traversal)
Use when: tree problems that naturally split into left/right subtrees.
Input: root node.
Output: preorder list.
```python
def preorder(root):
    # TC: O(n), SC: O(h) stack; h = tree height.
    if root is None:
        return []

    # Root, then left subtree, then right subtree.
    return [root.val] + preorder(root.left) + preorder(root.right)
```

### 4) Backtracking recursion (choose / explore / unchoose)
Use when: enumerate all valid combinations/permutations.
Input: current path, available choices, result container.
Output: fills `result` with solutions.
```python
def backtrack(path, choices, result):
    if is_solution(path):
        result.append(path[:])
        return

    for choice in choices:
        if not is_valid(path, choice):
            continue

        # Choose -> Explore -> Undo (core backtracking pattern).
        path.append(choice)
        backtrack(path, next_choices(path, choices), result)
        path.pop()
```

### 5) Recursion + memoization (top-down DP)
Use when: recursive subproblems repeat many times.
Input: integer `n`, optional memo map.
Output: computed value with caching.
```python
def fib(n, memo=None):
    # TC: O(n), SC: O(n) with memo + recursion stack.
    if memo is None:
        memo = {}
    if n <= 1:
        return n
    if n in memo:
        return memo[n]

    memo[n] = fib(n - 1, memo) + fib(n - 2, memo)
    return memo[n]
```

## Time & space complexity (Big O)
- Time depends on branching factor and recursion depth.
- Call stack space is at least `O(depth)`.
- Backtracking is often exponential in worst case.
- Memoization can reduce repeated work significantly (for example Fibonacci `O(2^n)` to `O(n)`).

## Practice questions (concept check)
1. What are the two required ingredients of every recursive function?
2. Why does recursion use additional memory compared with iterative loops?
3. What problem does memoization solve in recursive algorithms?

<details>
<summary>Answers</summary>

1. A base case and a recursive case that makes progress toward the base case.
2. Each call adds a stack frame until returning, so memory grows with depth.
3. It caches repeated subproblem results to avoid recomputation.

</details>

## Practice questions (LeetCode)
1. [Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching) *(Hard)*
2. [Sudoku Solver](https://leetcode.com/problems/sudoku-solver) *(Hard)*
3. [Expression Add Operators](https://leetcode.com/problems/expression-add-operators) *(Hard)*
4. [N-Queens](https://leetcode.com/problems/n-queens) *(Hard)*
5. [Word Search II](https://leetcode.com/problems/word-search-ii) *(Hard)*

## One thing that was confusing to me
At first, I thought recursion was only about calling the same function again, but I learned the critical part is designing the base case and guaranteeing progress toward it.

## See also
- [Depth-First Search (DFS)](../graph/dfs.md)
- [Prefix Sum](../prefix-sum/prefix-sum.md)
