# CH1 -- intro
## objectives
You will know

+ what is `SortedList<TKey,TValue>`

+ difference between `SortedList<TKey,TValue>` and `SortedDictionary<TKey,TValue>`

## CH1.1 -- what is `SortedList<TKey,TValue>`

The name `SortedList<TKey,TValue>` makes lots of people get confused.

`SortedList<TKey,TValue>` is NOT a list.

It consists of one or more key-value pairs.

Thus, it's likely a dictionary.

Although, in `SortedList<TKey,TValue>` the keys are automatically sorted after performing an operation that changes entries.

its API, data structure and implementation details are different than `SortedDictionary<TKey,TValue>`.

## CH1.2 -- difference between `SortedList<TKey,TValue>` and `SortedDictionary<TKey,TValue>`
Although keys are automatically sorted after performing an operation that changes entries in `SortedList<TKey,TValue>` and `SortedDictionary<TKey,TValue>`,

their data structure implementaion details are different, thus there are different performance with same operations.

| item | `SortedDictionary<TKey,TValue>` | `SortedList<TKey,TValue>` |
| :-- | :-- | :-- |
| data structure | internally uses RBT (Red-Black Tree) | internally uses two arrays—one for keys and one for values. |
| search | `O(log n)` since Red-Black Tree is a special kind of binary tree, it uses binary search. | `O(log n)` since it uses binary search. |
| insertion | `O(log n)` since it will perform binary search | `O(n)` since it uses arrays,<br>and it can sort keys and values using insertion sort<br>which takes `O(n)` iff the original array is fully sorted (here, is this case). |
| search | `O(log n)` since Red-Black Tree is a special kind of binary tree, it uses binary search. | `O(log n)` since it uses binary search. |





