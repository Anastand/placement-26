### Efficient Method

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        last_seen = {}
        l = 0
        res = 0
        
        for r, char in enumerate(s):
            # If we've seen the char and it's inside our current window
            if char in last_seen and last_seen[char] >= l:
                # Jump the left pointer to the right of the previous occurrence
                l = last_seen[char] + 1
            
            # Update/Insert the last seen index
            last_seen[char] = r
            res = max(res, r - l + 1)
            
        return res
```

---

### My Method (Final Working Version)

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        charSet = set()

        l, res = 0, 0

        for r, val in enumerate(s):
            while val in charSet:
                charSet.remove(s[l])
                l += 1

            charSet.add(val)
            res = max(res, r - l + 1)

        return res
```

---

# Module 4: Sliding Window

Sliding window is one of the most common patterns used in **FAANG medium problems**.
Instead of recomputing substrings repeatedly (`O(N²)`), we maintain a **dynamic window** and adjust it in **O(N)** time.

---

## Core Idea

We maintain a window:

```
[l ........ r]
```

Where:

* `l` → left boundary
* `r` → right boundary

The substring represented by the window is:

```python
s[l : r + 1]
```

Goal for this problem:

```
The window must contain only unique characters
```

---

# Window Size Formula

Length of the window is always:

```python
window_size = r - l + 1
```

Example:

```
index:   0 1 2
string:  a b c
```

```
l = 0
r = 2

window size = 2 - 0 + 1 = 3
```

---

# Data Structure Used — `set`

We use a set to track characters currently inside the window.

```python
charSet = set()
```

Operations used:

```python
charSet.add(x)      # add element
charSet.remove(x)   # remove element
x in charSet        # membership check
```

Time complexity:

| Operation | Complexity |
| --------- | ---------- |
| add       | O(1)       |
| remove    | O(1)       |
| lookup    | O(1)       |

This allows us to detect duplicates quickly.

---

# Sliding Window Steps

For each character:

1. Expand the window (move `r`)
2. If duplicate appears → shrink window (move `l`)
3. Update the maximum window size

---

# Why We Use `while`

Duplicates may require **multiple removals**.

Example:

```
s = "abba"
```

When second `"b"` appears:

```
ab[b]
```

First removal:

```
remove 'a'
window → bb
```

Still invalid.

Second removal:

```
remove 'b'
window → b
```

Now valid.

So we must repeat shrinking:

```python
while val in charSet:
```

Not:

```python
if val in charSet:
```

---

# Why We Remove `s[l]` (Not `val`)

Important invariant:

```
charSet == characters inside s[l : r + 1]
```

If we shrink the window, the character leaving is always:

```
s[l]
```

Example:

```
[a b c]
 ^
 l
```

After shrinking:

```
[b c]
 ^
 l
```

So we must remove:

```python
charSet.remove(s[l])
l += 1
```

---

# Why Removing `val` Is Wrong

Example:

```
s = "abca"
```

Window before duplicate:

```
[a b c]
```

Incoming character:

```
val = 'a'
```

If we remove `val`:

```
set → {b,c}
```

But the real window is still:

```
[a b c]
```

Now the set **does not match the window**, breaking the algorithm.

---

# Tracking the Best Window

We only store the **length**, not the substring.

```python
res = max(res, r - l + 1)
```

Example progression:

| Window | Size | Best |
| ------ | ---- | ---- |
| `a`    | 1    | 1    |
| `ab`   | 2    | 2    |
| `abc`  | 3    | 3    |
| `bca`  | 3    | 3    |

---

# Example Walkthrough

```
s = "abcabcbb"
```

Progression:

```
a       → {a}           size = 1
ab      → {a,b}         size = 2
abc     → {a,b,c}       size = 3
abca    → duplicate
bca     → {b,c,a}       size = 3
cab     → {c,a,b}       size = 3
```

Answer:

```
3
```

---

# Common Mistakes (You Hit These)

### 1. Removing `val` instead of `s[l]`

Wrong:

```python
charSet.remove(val)
```

Correct:

```python
charSet.remove(s[l])
```

---

### 2. Using `if` instead of `while`

Wrong:

```python
if val in charSet:
```

Correct:

```python
while val in charSet:
```

---

### 3. Returning inside the loop

Wrong:

```python
return res
```

inside loop.

Correct:

```python
return res
```

after loop.

---

### 4. Confusing `val` and `s[l]`

| Variable | Meaning                     |
| -------- | --------------------------- |
| `val`    | incoming character (`s[r]`) |
| `s[l]`   | character leaving window    |

---

# Time Complexity

```
O(N)
```

Each character enters and leaves the window **at most once**.

---

# Space Complexity

```
O(K)
```

Where `K` = number of unique characters in the window.

Worst case:

```
O(N)
```

---

# Sliding Window Mental Model

Think of the window like a **rubber band**:

```
[l -------- r]
```

Rules:

1. `r` always moves forward
2. `l` moves only when duplicates appear
3. window must always remain **valid**

---

# Pattern Recognition

If a problem asks for:

* **longest substring**
* **no repeating characters**
* **unique elements in window**

Your brain should trigger:

```
Sliding Window + Set
```
