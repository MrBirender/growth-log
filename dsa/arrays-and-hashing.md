# DSA Topic: Arrays & Hashing

> NeetCode Blind 75 — Section 1
> Language: JavaScript

---

## Key Concepts

### Array Internals
- Contiguous block of memory — elements stored next to each other
- O(1) index access: `address = base + (index × element_size)`
- Insert/delete at middle: O(n) — must shift elements
- Append at end: O(1) amortized (doubling strategy when capacity exceeded)

### JS Arrays vs Classical Arrays
- Classical arrays: fixed size, same type, raw contiguous memory
- JS arrays: dynamic, mixed types, V8 optimizes as real arrays when elements are uniform type
- V8 degrades JS array to hash map internally if you: add non-numeric keys, leave holes, mix types
- Implication: `arr[999999] = 1` on an otherwise small array → hash map internally

### Hash Tables
- Hash function: `value → index` (same value always same index)
- O(1) add, O(1) lookup on average
- In JS: plain object `{}`, `Set`, or `Map`
- Use `Set` when you only need existence (`has`/`add`)
- Use `{}` or `Map` when you need to store a value against a key
- `Map` vs `{}`: Map allows any key type, has `.size`, guaranteed insertion order — but `{}` is fine for string keys
- Map methods: `map.get(key)`, `map.set(key, val)`, `map.has(key)`
- Collision handling: chaining or open addressing (V8 uses hidden classes + pointer compression)

### Complexity Calculation Rules
- **Time**: count how many times the core work executes relative to n
- **Space**: count only new memory you allocate (input array doesn't count)
- Single variables = O(1) each (fixed size regardless of input)
- One extra array of size n = O(n) space
- Nested loops over n = O(n²) time

---

## Problem-Solving Framework

1. Read problem twice — understand input, output, constraints
2. Brute force first → time/space → identify bottleneck → think optimal
3. Code (15–20 min easy, 25–30 min medium)
4. Test manually with given examples + one edge case
5. After passing: check complexity, read top solutions, identify pattern
6. If stuck 10+ min → NeetCode video → rewrite cold from blank file

**Key rule:** Never move on until you can solve it from blank without help.

---

## Problems

### hasDuplicate
**Problem:** Given `nums[]`, return true if any value appears more than once.

#### Brute Force — O(n²) time, O(1) space
```js
class Solution {
    hasDuplicate(nums) {
        for (let i = 0; i < nums.length; i++) {
            const temp = nums[i]
            for (let j = i + 1; j < nums.length; j++) {
                if (temp === nums[j]) return true
            }
        }
        return false
    }
}
```
- Bottleneck: inner loop scans the rest of the array for every element → O(n²)

#### Optimal — O(n) time, O(n) space (Set)
```js
class Solution {
    hasDuplicate(nums) {
        const seen = new Set()
        for (let i = 0; i < nums.length; i++) {
            if (seen.has(nums[i])) return true
            seen.add(nums[i])
        }
        return false
    }
}
```
**Pattern:** trade space for time — store what you've seen, O(1) lookup instead of O(n) scan.
**Status:** Accepted ✓

---

### Valid Anagram
**Problem:** Given two strings `s` and `t`, return true if they are anagrams of each other.

**Key insight:** length check first — if lengths differ, impossible to be anagrams.

**Edge case caught:** `map.has(key)` is not enough when decrementing — key still exists when count is 0. Must check `map.get(key) > 0`.

#### Optimal — O(n) time, O(1) space* (Map)
```js
class Solution {
    isAnagram(s, t) {
        if (s.length !== t.length) return false

        const map = new Map()

        for (let i = 0; i < s.length; i++) {
            if (map.has(s[i])) {
                map.set(s[i], map.get(s[i]) + 1)
            } else {
                map.set(s[i], 1)
            }
        }

        for (let j = 0; j < t.length; j++) {
            if (map.has(t[j]) && map.get(t[j]) > 0) {
                map.set(t[j], map.get(t[j]) - 1)
            } else {
                return false
            }
        }

        return true
    }
}
```
*Space is O(1) in practice — character set is fixed at 26 lowercase letters, so map never exceeds 26 entries regardless of input size.

**Status:** Accepted ✓

---

### Two Sum
**Problem:** Given `nums[]` and a `target`, return indices `[i, j]` where `nums[i] + nums[j] === target`.

**Key insight:** for each number, the complement is `target - nums[i]`. Store complement as key, index as value. On each step check if current number exists as a key.

#### Brute Force — O(n²) time, O(1) space
```js
class Solution {
    twoSum(nums, target) {
        for (let i = 0; i < nums.length; i++) {
            for (let j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] === target) return [i, j]
            }
        }
    }
}
```

#### Optimal — O(n) time, O(n) space (Map)
```js
class Solution {
    twoSum(nums, target) {
        const map = new Map()
        for (let i = 0; i < nums.length; i++) {
            if (map.has(nums[i])) {
                return [map.get(nums[i]), i]
            } else {
                map.set(target - nums[i], i)
            }
        }
    }
}
```
**Status:** Accepted ✓

---

### Group Anagrams
**Problem:** Given an array of strings, group all anagrams together into sublists.

**Key insight:** Sort each string's characters alphabetically — all anagrams produce the same sorted key. Use that as a Map key.

**Why not sum of character positions?** Collisions — `"az"` and `"by"` both sum to 27 but are not anagrams.

**JS tip:** `map.values()` returns a MapIterator, not an array. Use `Array.from(map.values())` to convert.

#### Optimal — O(n × k log k) time, O(n) space
```js
class Solution {
    groupAnagrams(strs) {
        const map = new Map()
        for (let i = 0; i < strs.length; i++) {
            const key = strs[i].split("").sort().join("")
            if (map.has(key)) {
                map.get(key).push(strs[i])
            } else {
                map.set(key, [strs[i]])
            }
        }
        return Array.from(map.values())
    }
}
```
**Status:** Accepted ✓

---

### Top K Frequent Elements
**Problem:** Given `nums[]` and integer `k`, return the `k` most frequent elements.

**Approach 1 — Sorting: O(n log n) time, O(n) space**
```js
class Solution {
    topKFrequent(nums, k) {
        const map = new Map()
        for (let i = 0; i < nums.length; i++) {
            if (map.has(nums[i])) {
                map.set(nums[i], map.get(nums[i]) + 1)
            } else {
                map.set(nums[i], 1)
            }
        }
        const result = Array.from(map.entries()).sort((a, b) => b[1] - a[1])
        const topK = []
        for (let i = 0; i < k; i++) {
            topK.push(result[i][0])
        }
        return topK
    }
}
```

**Approach 2 — Bucket Sort: O(n) time (in progress)**
- Create bucket array of size `nums.length + 1`
- Index = frequency, value = array of numbers with that frequency
- Fill buckets from map without sorting
- Read from end of bucket array, collect k elements
- **Status:** In progress — resume tomorrow

---

## Pending / Open Questions
- Why do arrays often outperform linked lists in real-world benchmarks despite worse theoretical complexity? (cache locality, CPU cache lines — not yet answered)
