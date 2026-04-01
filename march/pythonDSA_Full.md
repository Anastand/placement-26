````python
words = ["level", "algo", "eye", "racecar", "a", "ab", "nod"]
filter_word = [ x for x in words if len(x) > 2]
palindrome_word =[ x for x in filter_word if x == x[::-1] ]
print(sorted(palindrome_word,key=lambda x: (-len(x), x)))

# One-liner version:
res = sorted([w for w in words if len(w) > 2 and w == w[::-1]], key=lambda x: len(x), reverse=True)

# Or using the negative length trick (very common in competitive programming):
res = sorted([w for w in words if len(w) > 2 and w == w[::-1]], key=lambda x: -len(x))

"""
The above example res is solution in a very simple, senior like manner.
"""
````

---

# Module 1: Interview Basics

## 1️⃣ Slicing `[start:stop:step]`

- `arr[::-1]` → reverse
- `arr[1:4]` → index 1–3
- `arr[::2]` → every second element
- `arr[:]` → shallow copy

_Time:_ **O(k)** (slice length). Always creates a shallow copy.

---

## 2️⃣ List Comprehensions

Stop writing manual loops + `append()`.

**Format:**
```python
[expression for item in iterable if condition]
```

**Flatten 2D:**
```python
[x for row in matrix for x in row]
```

**Dict comprehension:**
```python
{k: v for k, v in zip(keys, values)}
{word: len(word) for word in words}
```

**Set comprehension:**
```python
{x * x for x in range(10)}  # unique squares
```

---

## 3️⃣ `enumerate()` over `range(len())`

**Avoid:**
```python
for i in range(len(arr)):
```

**Use:**
```python
for i, val in enumerate(arr):
```

Use `range(len())` only when manipulating indices non-linearly.

---

## 4️⃣ Sorting with `lambda`

Python uses Timsort → O(N log N).

**Examples:**
```python
arr.sort(key=lambda x: x[1])               # by 2nd element
sorted(arr, key=abs)                        # by absolute value
arr.sort(key=lambda x: (-x.score, x.name)) # multi-level
```

Tuple keys → primary sort, then tie-breaker.

---

## 5️⃣ `zip()` — Parallel Iteration

```python
for a, b in zip(list1, list2):
    print(a, b)

# Transpose a matrix:
transposed = list(zip(*matrix))

# Unzip:
keys, values = zip(*pairs)
```

`zip` stops at the shortest iterable. Use `itertools.zip_longest` to pad instead.

---

## 6️⃣ `any()` / `all()`

Short-circuit evaluators. Stop early, don't write loops for these.

```python
any(x < 0 for x in arr)   # True if any negative
all(x > 0 for x in arr)   # True if all positive
any(arr)                   # True if any truthy element
```

---

## Bad vs Pythonic

### Filtering + Mapping

**Bad:**
```python
squares = []
for x in nums:
    if x % 2 == 0:
        squares.append(x * x)
```

**Pythonic:**
```python
squares = [x * x for x in nums if x % 2 == 0]
```

---

### Index + Value

**Bad:**
```python
for i in range(len(names)):
    print(i, names[i])
```

**Good:**
```python
for i, name in enumerate(names):
    print(i, name)
```

---

### Reverse Sub-portion

**Manual:**
```python
while i < j:
    arr[i], arr[j] = arr[j], arr[i]
```

**Pythonic:**
```python
arr[:3] = arr[:3][::-1]
```

---

# Module 2: Collections — Your DSA Utility Belt

## 1️⃣ `Counter`

```python
from collections import Counter

freq = Counter("aabbccc")
# Counter({'c': 3, 'a': 2, 'b': 2})

freq.most_common(2)  # [('c', 3), ('a', 2)]
freq['z']            # 0 — no KeyError, safe default

# Anagram check:
Counter("listen") == Counter("silent")  # True
```

---

## 2️⃣ `defaultdict`

Eliminates the `if key not in dict` boilerplate.

```python
from collections import defaultdict

graph = defaultdict(list)
graph['a'].append('b')  # no KeyError

freq = defaultdict(int)
for ch in s:
    freq[ch] += 1

# Group anagrams pattern:
groups = defaultdict(list)
for word in words:
    groups[tuple(sorted(word))].append(word)
```

---

## 3️⃣ `deque` — O(1) Both Ends

```python
from collections import deque

q = deque()
q.append(1)       # push right
q.appendleft(0)   # push left
q.pop()           # pop right
q.popleft()       # pop left — O(1), unlike list.pop(0)

# BFS template:
queue = deque([start])
while queue:
    node = queue.popleft()
    # process node
```

> `list.pop(0)` is O(N). `deque.popleft()` is O(1). Always use `deque` for queues.

---

## 4️⃣ `OrderedDict` (mostly replaced by `dict` in Python 3.7+)

Regular `dict` maintains insertion order since Python 3.7. Use `OrderedDict` only when you need `move_to_end()` (LRU cache pattern).

```python
from collections import OrderedDict

cache = OrderedDict()
cache['a'] = 1
cache.move_to_end('a')        # move to back
cache.move_to_end('a', last=False)  # move to front
cache.popitem(last=False)     # pop from front (LRU eviction)
```

---

# Module 3: `heapq` — Priority Queue

Python only has **min-heap**. For max-heap, negate values.

```python
import heapq

nums = [3, 1, 4, 1, 5]
heapq.heapify(nums)         # O(N), in-place

heapq.heappush(nums, 2)     # O(log N)
heapq.heappop(nums)         # O(log N), returns smallest

# Max-heap trick:
max_heap = [-x for x in nums]
heapq.heapify(max_heap)
largest = -heapq.heappop(max_heap)

# K largest elements:
heapq.nlargest(k, nums)     # O(N log k)
heapq.nsmallest(k, nums)    # O(N log k)

# Heap with tuple (priority, value):
pq = []
heapq.heappush(pq, (priority, value))
```

**When to use:** Dijkstra, K closest, merge K sorted lists, sliding window max.

---

# Module 4: Two Pointers

One of the most common interview patterns. O(N), no extra space.

## Classic: Pair Sum in Sorted Array

```python
def two_sum_sorted(arr, target):
    l, r = 0, len(arr) - 1
    while l < r:
        s = arr[l] + arr[r]
        if s == target:
            return [l, r]
        elif s < target:
            l += 1
        else:
            r -= 1
    return []
```

## Remove Duplicates In-place

```python
def remove_duplicates(nums):
    if not nums:
        return 0
    slow = 0
    for fast in range(1, len(nums)):
        if nums[fast] != nums[slow]:
            slow += 1
            nums[slow] = nums[fast]
    return slow + 1
```

## Reverse a String In-place

```python
def reverse(s):
    s = list(s)
    l, r = 0, len(s) - 1
    while l < r:
        s[l], s[r] = s[r], s[l]
        l += 1
        r -= 1
    return ''.join(s)
```

---

# Module 5: Sliding Window

Fixed and variable window. O(N).

## Fixed Window — Max Sum of K Elements

```python
def max_sum(arr, k):
    window = sum(arr[:k])
    best = window
    for i in range(k, len(arr)):
        window += arr[i] - arr[i - k]
        best = max(best, window)
    return best
```

## Variable Window — Longest Substring Without Repeating

```python
def length_of_longest_substring(s):
    seen = {}
    l = best = 0
    for r, ch in enumerate(s):
        if ch in seen and seen[ch] >= l:
            l = seen[ch] + 1
        seen[ch] = r
        best = max(best, r - l + 1)
    return best
```

**Pattern:** expand `r`, shrink `l` when constraint is violated.

---

# Module 6: Stack Patterns

## Monotonic Stack — Next Greater Element

```python
def next_greater(nums):
    result = [-1] * len(nums)
    stack = []  # stores indices
    for i, val in enumerate(nums):
        while stack and nums[stack[-1]] < val:
            result[stack.pop()] = val
        stack.append(i)
    return result
```

## Valid Parentheses

```python
def is_valid(s):
    stack = []
    mapping = {')': '(', '}': '{', ']': '['}
    for ch in s:
        if ch in mapping:
            if not stack or stack[-1] != mapping[ch]:
                return False
            stack.pop()
        else:
            stack.append(ch)
    return not stack
```

---

# Module 7: Binary Search

Always on **sorted** data. O(log N).

## Standard Template

```python
def binary_search(arr, target):
    l, r = 0, len(arr) - 1
    while l <= r:
        mid = l + (r - l) // 2   # avoids overflow (habit from C++)
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            l = mid + 1
        else:
            r = mid - 1
    return -1
```

## Leftmost / Lower Bound

```python
import bisect

bisect.bisect_left(arr, target)   # first position where target can go
bisect.bisect_right(arr, target)  # last position where target can go
bisect.insort(arr, val)           # insert in sorted order, O(N)
```

## Binary Search on Answer (powerful pattern)

```python
# "minimum capacity to ship in D days" type problems
def feasible(capacity):
    days, curr = 1, 0
    for w in weights:
        if curr + w > capacity:
            days += 1
            curr = 0
        curr += w
    return days <= D

lo, hi = max(weights), sum(weights)
while lo < hi:
    mid = (lo + hi) // 2
    if feasible(mid):
        hi = mid
    else:
        lo = mid + 1
# answer is lo
```

---

# Module 8: String Tricks

```python
# Reverse words:
" ".join(s.split()[::-1])

# Check palindrome (ignore case + non-alnum):
s = ''.join(ch.lower() for ch in s if ch.isalnum())
s == s[::-1]

# Count char frequency without Counter:
from collections import defaultdict
freq = defaultdict(int)
for c in s: freq[c] += 1

# ord / chr (for shift ciphers, etc.):
ord('a')        # 97
chr(97)         # 'a'
(ord(c) - ord('a'))  # 0-based index of letter
```

---

# Module 9: Useful `functools`

```python
from functools import lru_cache, reduce

# Memoization — turns recursion into DP automatically:
@lru_cache(maxsize=None)
def fib(n):
    if n < 2: return n
    return fib(n-1) + fib(n-2)

# Or in Python 3.9+:
from functools import cache
@cache
def fib(n): ...

# reduce:
from functools import reduce
reduce(lambda a, b: a * b, [1, 2, 3, 4])  # 24
```

> `@lru_cache` is your shortcut to top-down DP. Use it to avoid writing a memo dict manually.

---

# Module 10: Complexity Cheat Sheet

| Operation | List | Set / Dict | Heap | Sorted |
|---|---|---|---|---|
| Access by index | O(1) | — | — | O(1) |
| Search | O(N) | **O(1)** | O(N) | O(log N) |
| Insert (end) | O(1) amortized | O(1) | O(log N) | O(log N) |
| Insert (front) | **O(N)** | — | — | — |
| Delete | O(N) | O(1) | O(log N) | O(log N) |
| Min/Max | O(N) | O(N) | **O(1)** | O(1) |

**Key rules:**
- Need fast lookup → `set` or `dict`
- Need sorted + fast insert/delete → `SortedList` (from `sortedcontainers`)
- Need min/max repeatedly → `heapq`
- Need O(1) both-end insert/delete → `deque`
