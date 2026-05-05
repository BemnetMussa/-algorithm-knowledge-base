# Algorithm Knowledge Base

Welcome to our collaborative DSA learning space.

This repository is a student-powered knowledge base for the A2SV Software Engineer Trainee Training Program, and it is open to all algorithm lovers who want to learn and share. Each contributor adds at least one clear, beginner-friendly algorithm note, and everyone benefits from shared explanations, examples, and practice questions. Progress can be fast or slow, and that is okay; this repo is about learning together, building confidence, and helping one another.

> "Shared knowledge is far greater than individual knowledge."

## How to Contribute

### 1) Claim an Algorithm First (avoid duplicates)

Before writing, claim the algorithm you want to document:

- **Option A:** Open a GitHub Issue titled `Claim: <Algorithm Name>`
- **Option B:** Add your name to a claim table in this README (example below)

| Algorithm | Claimed by | Status |
| --- | --- | --- |
| Binary Search | @student-username | In progress |
| Bubble Sort | @another-student | Done |
| Sliding Window | @Amanuel-Merara | Done |
| Depth First Search | @BemnetMussa| Done |
| Breadth First Search | @BemnetMussa | Done |

### 2) Create Your Note File in the Correct Folder

Add your note in the appropriate category folder. Example path:

```text
/sorting/bubble-sort.md
```

Use lowercase and hyphens for file names.

### 3) Follow the Required Note Template

Copy the template below exactly and fill in your content.

### 4) Open a Pull Request (PR)

Submit your branch as a Pull Request to this repository for review and merge.

PR title example:

```text
Add note: Bubble Sort by @your-username
```

## Required Note Template

Copy this template into your algorithm file:

````markdown
# <Algorithm Name>

## One-sentence definition
<Explain the algorithm in one simple sentence.>

## When to use it (use cases)
- <Use case 1>
- <Use case 2>

## Step-by-step explanation (plain language, no code first)
1. <Step 1 in simple words>
2. <Step 2>
3. <Step 3>

## Templates (<Choose one: Python / Java / C++ >)
<You can include multiple templates when relevant, for example: standard, iterative/recursive, or problem-specific variants.>
```python
# Put your template here
```

## Time & space complexity (Big O)
- Time: <best / average / worst if relevant>
- Space: <Big O>

## Practice questions (concept check)
1. <Concept question 1>
2. <Concept question 2>
3. <Concept question 3 (optional)>

<details>
<summary>Answers</summary>

1. <Answer 1>
2. <Answer 2>
3. <Answer 3 (if Q3 exists)>

</details>

## Practice questions (LeetCode/Codeforces)
1. <Problem link 1>
2. <Problem link 2>
3. <Problem link 3 (optional)>

## One thing that was confusing to me
<Write one concept that confused you, even if you understand it now. This helps identify learning gaps.>

## See also
- [<Related Algorithm 1>](../<path-to-related-file>.md)
- [<Related Algorithm 2>](../<path-to-related-file>.md)
````

## Suggested Folder Structure

You can organize notes like this:

```text
sorting/
searching/
graph/
linked-list/
recursion/
heap/
dynamic-programming/
data-structures/
greedy/
backtracking/
bit-manipulation/
```

Example full paths:

- `sorting/bubble-sort.md`
- `searching/binary-search.md`
- `graph/bfs.md`
- `linked-list/singly-linked-list.md`
- `recursion/recursion.md`
- `heap/heap.md`
- `dynamic-programming/knapsack.md`
- `data-structures/stack.md`

## Quality Guidelines

Write for a fellow student who is struggling, not for an expert.

- Be kind, clear, and practical.
- Use plain language before advanced terms.
- Add small diagrams when useful (ASCII diagrams or image links are welcome).
- Prefer short sections and concrete examples over long theory.
- Include practice problems from platforms like LeetCode or Codeforces when relevant.
- Perfection is not required; clarity and effort matter most.

## Example Note (Reference): Binary Search

````markdown
# Binary Search

## One-sentence definition
Binary Search finds a target value in a sorted list by repeatedly checking the middle element and cutting the search space in half.

## When to use it (use cases)
- When data is sorted and you need fast lookups.
- When linear search is too slow for large arrays.

## Step-by-step explanation (plain language, no code first)
1. Start with two pointers: one at the beginning (`left`) and one at the end (`right`) of the sorted list.
2. Find the middle index.
3. Compare the middle value with the target:
   - If equal, you found it.
   - If target is smaller, continue in the left half.
   - If target is larger, continue in the right half.
4. Repeat until found or until `left` passes `right` (not found).

## Templates (Python)
```python
def binary_search(nums, target):
    left, right = 0, len(nums) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if nums[mid] == target:
            return mid
        elif nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

## Time & space complexity (Big O)
- Time: `O(log n)`
- Space: `O(1)` (iterative version)

## Practice questions
1. Why does Binary Search require a sorted array?
2. What happens when the target does not exist in the array?
3. How is Binary Search faster than linear search for large inputs?

<details>
<summary>Answers</summary>

1. Because the decision to go left or right only works when elements are ordered.
2. The loop ends when `left > right`, and we return `-1` (or "not found").
3. Binary Search removes half the remaining data each step (`O(log n)`), while linear search checks one by one (`O(n)`).

</details>

## One thing that was confusing to me
At first, I was confused about why the loop condition is `left <= right` and not `left < right`; I learned that `<=` is needed so single-element ranges are still checked.

## See also
- [Linear Search](../searching/linear-search.md)
- [Jump Search](../searching/jump-search.md)
````

## FAQ

### Can I submit more than one algorithm?
Yes. Please submit one first, wait until it is merged, then you can claim and submit another.

### What if I do not know Git/GitHub well?
That is completely fine. You can use this beginner guide: [GitHub Pull Request Guide](https://docs.github.com/en/get-started/quickstart/contributing-to-projects).  
If needed, you may also share your `.md` file with a maintainer, and we can help upload it.

### How will I be credited?
You will be credited by your GitHub username in commit/PR history, and you may include your real name in your note if you want.

## Code of Conduct

Be respectful, write in your own words (no plagiarism), and help your classmates learn.
