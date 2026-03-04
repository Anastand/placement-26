```py
from collections import defaultdict
class Solution:
    def groupAnagrams(self, strs: List[str]) -> List[List[str]]:
        groups = defaultdict(list)

        for word in strs:
            key = tuple(sorted(word))
            groups[key].append(word)
        
        return list(groups.values())
```

## Module 2: Hashing Mastery (The Bread & Butter)

In JS, you have `Map` and `Set`.  
In Python, `dict` and `set` are much more powerful thanks to the `collections` module.  
If you're not using `defaultdict` or `Counter` in a coding interview, you're writing too much boilerplate.

---

### 1️⃣ dict vs defaultdict

In a standard `dict`, accessing a missing key raises a `KeyError`. You’d usually do:
```python
if key not in d:
    d[key] = 0
d[key] += 1
```

**Pythonic way (`defaultdict`):**
```python
from collections import defaultdict
d = defaultdict(int) # Default value of int is 0
d["missing_key"] += 1 # No error, automatically initializes to 0
```

---

### 2️⃣ Counter (The Cheat Code)

If the problem involves counting frequencies (very common in "Sliding Window" or "Anagrams"), use `Counter`:
```python
from collections import Counter
c = Counter("apple") # {'p': 2, 'a': 1, 'l': 1, 'e': 1}
print(c.most_common(1)) # [('p', 2)]
```

---

### 3️⃣ Set Operations

- Sets are **O(1)** lookup. Use them for "Seen" logic.
```python
s = set()
s.add(x)
s.remove(x)     # raises error if missing
s.discard(x)    # no error if missing
```
- Set Algebra:  
  - Intersection: `s1 & s2`  
  - Union: `s1 | s2`  
  - Difference: `s1 - s2`

---

### 4️⃣ Iterating Efficiently

```python
d = {"a": 1, "b": 2}
for k, v in d.items():   # Get both key and value
for k in d.keys():       # Get keys
for v in d.values():     # Get values
```

---

### Module 2 Challenge

**The Problem: Group Anagrams**

> Given an array of strings `strs`, group the anagrams together. You can return the answer in any order.
>
> An *Anagram* is a word or phrase formed by rearranging the letters of a different word or phrase.

**Input:**  
`strs = ["eat", "tea", "tan", "ate", "nat", "bat"]`  
**Expected Output (one grouping):**  
`[["bat"], ["nat","tan"], ["ate","eat","tea"]]`

**Constraints for you:**
- Use a `defaultdict`.
- For the dictionary key, use a `tuple` (since lists are unhashable and cannot be keys).
- Try to do this in **O(N·KlogK)** where **N** is the number of strings and **K** is the max length of a string.

