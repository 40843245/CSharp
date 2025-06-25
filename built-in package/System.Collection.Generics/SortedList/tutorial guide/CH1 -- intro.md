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

Here is the comparison table of featurem, time complexity and (memory) space complexity etc.

| item | `SortedDictionary<TKey,TValue>` | `SortedList<TKey,TValue>` |
| :-- | :-- | :-- |
| data structure | internally uses RBT (Red-Black Tree) | internally uses two arrays—one for keys and one for values. |
| search | `O(log n)` since Red-Black Tree is a special kind of binary tree, it uses binary search. | `O(log n)` since it uses binary search. |
| insertion | `O(log n)` since it will perform binary search | `O(n)` since it uses arrays,<br>and it can sort keys and values using insertion sort<br>which takes `O(n)` iff the original array is fully sorted (here, is this case). |
| deleietion | `O(log n)` since it will perform binary search | `O(n)` since it just needs to find the value which takes `O(n)`, remove it, then shifts left which takes `O(n)`. The total time is `O(n)+O(n)=O(n)`  |
| memory usage | less efficient than the another due to it is stored using Red-Black Tree | more efficient than the another since it uses array to store, while array is contiguous. |
| provide (zero-based) index access? | NO, since it is NOT an array | YES, since it consists of two arrays. |
| use case | ideal for scenarios with frequent insertions and deletions, or when the dataset is large. | best for scenarios where the dataset is relatively small and you don't expect frequent insertions or deletions. |
