# DSA Topic: Linked Lists

> Covered in earlier sessions before NeetCode problems started.
> Language: JavaScript (concepts apply universally)

---

## Structure

### Singly Linked List
- Each node: `{ value, next }`
- `next` of last node = `null`
- Only forward traversal

### Doubly Linked List
- Each node: `{ value, prev, next }`
- Both forward and backward traversal
- Used in LRU Cache (O(1) removal from any position)

---

## Complexity (corrected)

| Operation | Singly | Doubly |
|---|---|---|
| Access by index | O(n) | O(n) |
| Insert/delete at head | O(1) | O(1) |
| Insert/delete at tail | O(n)* | O(1) with tail pointer |
| Insert/delete at middle | O(n) | O(n) to find, O(1) to remove |
| Search | O(n) | O(n) |

*O(1) at tail only if you maintain a `tail` pointer — otherwise O(n) to traverse to end.

---

## Use Cases

### LRU Cache — Doubly Linked List + HashMap
- Most recently used → head of list
- Least recently used → tail of list
- HashMap: `key → node` reference for O(1) lookup
- On `get(key)`: find node via map, move to head → O(1)
- On `put(key, val)`: insert at head; if over capacity, remove tail → O(1)
- Without doubly linked list, removal from middle is O(n)

---

## Pending Problems (NeetCode)
- [ ] Reverse Linked List (LeetCode 206)
- [ ] Merge Two Sorted Lists (LeetCode 21)
- [ ] Linked List Cycle (LeetCode 141)
- [ ] Reorder List (LeetCode 143)
