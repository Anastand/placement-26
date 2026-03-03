```py
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
```


# Module 1: Interview Basics

---

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
arr.sort(key=lambda x: x[1])              # by 2nd element
sorted(arr, key=abs)                      # by absolute value
arr.sort(key=lambda x: (-x.score, x.name))  # multi-level
```

Tuple keys → primary sort, then tie-breaker.

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
