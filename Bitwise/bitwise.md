
# Bitwise Operations

## One-sentence definition
Bitwise operations manipulate integers at the binary level, treating each bit (0 or 1) separately to perform fast checks, updates, or calculations.

## When to use it (use cases)
- When you need extremely fast checks (e.g., even/odd, power of two)
- When you want to store multiple flags or states in a single integer (bit masking)
- When solving problems that ask for subset enumeration, missing numbers, or counting bits
- When arithmetic is too slow or you need direct hardware‑style control

## Step-by-step explanation (plain language, no code first)

1. **Bits are 0 or 1** – everything in a computer is stored as bits. An integer like `5` is `101` in binary (from least significant bit to most).

2. **AND (`&`)** – compares two bits: result is `1` only if **both** bits are `1`.  
   Example: `5 & 3` → `101 & 011` = `001` (decimal `1`).

3. **OR (`|`)** – result is `1` if **at least one** bit is `1`.  
   `5 | 3` → `101 | 011` = `111` (decimal `7`).

4. **XOR (`^`)** – result is `1` if the bits are **different** (one `0`, one `1`).  
   `5 ^ 3` → `101 ^ 011` = `110` (decimal `6`).

5. **NOT (`~`)** – flips every bit: `0` becomes `1`, `1` becomes `0`.  
   In Python, `~x` = `-x-1` because integers are signed.

6. **Left shift (`<<`)** – moves bits to the left, adding zeros on the right.  
   `1 << 3` means `1` becomes `1000` (binary) = decimal `8`.  
   Effectively multiplies by `2**k`.

7. **Right shift (`>>`)** – moves bits to the right, discarding bits that fall off.  
   `16 >> 3` = `10` (binary `10000` → `10`) = decimal `2`.  
   Effectively integer division by `2**k`.

8. **Bit masking** – use an integer (mask) to read, set, clear, or toggle specific bits.

## Templates (Python)

### Common bitwise operations
```python
# Check k-th bit (0-indexed from LSB)
def is_kth_bit_set(num: int, k: int) -> bool:
    return (num >> k) & 1 == 1   # or (num & (1 << k)) != 0

# Set k-th bit to 1
def set_kth_bit(num: int, k: int) -> int:
    return num | (1 << k)

# Clear k-th bit (set to 0)
def clear_kth_bit(num: int, k: int) -> int:
    return num & ~(1 << k)

# Toggle k-th bit (0→1, 1→0)
def toggle_kth_bit(num: int, k: int) -> int:
    return num ^ (1 << k)

# Count set bits (Brian Kernighan’s method)
def count_set_bits(num: int) -> int:
    count = 0
    while num:
        num &= (num - 1)   # removes the lowest set bit
        count += 1
    return count

# Check if number is power of two
def is_power_of_two(num: int) -> bool:
    return num > 0 and (num & (num - 1)) == 0
```

### Problem‑specific template: Counting bits from 0 to n
```python
def count_bits(n: int) -> list[int]:
    ans = [0] * (n + 1)
    for i in range(1, n + 1):
        # i >> 1 is i//2, i & 1 checks LSB
        ans[i] = ans[i >> 1] + (i & 1)
    return ans
```

### Missing number (using XOR)
```python
def missing_number(nums: list[int]) -> int:
    n = len(nums)
    xor_all = 0
    for i in range(n + 1):
        xor_all ^= i
    for num in nums:
        xor_all ^= num
    return xor_all
```

## Time & space complexity (Big O)
- **Single bitwise operation** (`&`, `|`, `^`, `<<`, `>>`, `~`): O(1) time, O(1) space.
- **Counting bits in a loop** (simple method): O(n log n) because each number has log bits.
- **Brian Kernighan’s count** (per number): O(number of set bits) ≤ O(log n).
- **DP counting bits for 0..n**: O(n) time, O(n) space.

## Practice questions (concept check)
1. Why is `x & (x - 1)` used to remove the lowest set bit?  
2. What is the result of `5 ^ 5` ? Why is that useful for finding a missing number?  
3. How can you check if an integer is even or odd using a bitwise operator?

<details>
<summary>Answers</summary>

1. `x - 1` flips all bits after the lowest 1; ANDing with `x` clears that 1.  
2. `5 ^ 5 = 0` because XOR cancels equal bits. This lets us cancel all numbers that appear twice, leaving only the one that appears once (or the missing one).  
3. `n & 1` — if `1`, the number is odd; if `0`, it is even.

</details>

## Practice questions (LeetCode/Codeforces)
1. [Counting Bits](https://leetcode.com/problems/counting-bits/) – exactly the DP example above.  
2. [Missing Number](https://leetcode.com/problems/missing-number/) – use XOR or sum formula.  
3. [Single Number](https://leetcode.com/problems/single-number/) – every element appears twice except one.  
4. [Number of 1 Bits](https://leetcode.com/problems/number-of-1-bits/) – popcount.  
5. [Subsets](https://leetcode.com/problems/subsets/) – use bitmask to enumerate all subsets.  
6. [Permutations](https://leetcode.com/problems/permutations/) – a bitmask can track used elements (as in your PDF).  

## One thing that was confusing to me
At first, I did not understand why `~` in Python gives negative numbers (`~5 = -6`).  
It is because Python uses **infinite‑bit two’s complement**. Flipping all bits of `...101` (5) gives `...010` which is -6 in two’s complement. For bit masking we often ignore this and use `&` with a finite mask (e.g., `~mask & 0xFF` for 8 bits).

## See also
- [Bitmask subsets approach](../backtracking/subsets.md) (if available)
- [XOR tricks for missing numbers](../bit-manipulation/xor-tricks.md)
- Python `int.bit_count()` (Python 3.8+), `bin()`, `int.from_bytes()`
```
