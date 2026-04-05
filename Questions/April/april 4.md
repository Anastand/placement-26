[https://neetcode.io/problems/two-integer-sum/question?list=neetcode150](https://neetcode.io/problems/two-integer-sum/question?list=neetcode150)

> Points to remember

- The solution uses a dictionary for lookups.
- For each value, check if `target - val` exists in the dictionary:
  - If it does, return the indices.
  - If not, add the current value and its index to the dictionary.
- Using `defaultdict` is convenient because it automatically handles missing keys.

[https://neetcode.io/problems/anagram-groups/question?list=neetcode150](https://neetcode.io/problems/anagram-groups/question?list=neetcode150)

> Points to remember

- Dictionary keys must be immutable (use a tuple instead of a list).
- `sorted` returns a list.
- Pay attention to the required return format (e.g., if a nested list is needed, use `list(group.values())` where `group = defaultdict(list)`).

[https://neetcode.io/problems/top-k-elements-in-list/question?list=neetcode150](https://neetcode.io/problems/top-k-elements-in-list/question?list=neetcode150)

> Points to remember

- The main solution works as follows:

  ### Compressed Explanation

  1. `sorted(group)`  
     Sorts the dictionary keys (the numbers).
  2. `key=group.get`  
     Sorts keys by their frequency (value in the dictionary).
  3. `reverse=True`  
     Sorts from highest to lowest frequency.
  4. `[:k]`  
     Takes the first `k` elements (the most frequent numbers).

  ### Meaning of the Line

  ```
  Sort the numbers by their frequency (highest first) and return the top k numbers.
  ```
