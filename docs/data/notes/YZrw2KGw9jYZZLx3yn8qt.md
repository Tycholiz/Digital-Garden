
Heaps can thought of as regular [[binary trees|general.lang.data-structs.tree.binary]] with two special characteristics:
1. Heaps must be Complete Binary Trees.
2. The nodes must be ordered according to the Heap order property. Two heap order properties are as follows:
    1. Max Heap Property: All the parent node keys must be greater than or equal to their child node keys in max-heaps. So the root node will always contain the largest element in the Heap. If Node A has a child node B, then, `key(A) >=key(B)key(A)>=key(B)`
    2. Min Heap Property: In Min-Heaps, all the parent node keys are less than or equal to their child node keys. So the root node, in this case, will always contain the smallest element present in the Heap. If Node A has a child node B, then: `key(A) <=key(B)key(A)<=key(B)`

Heaps are useful for sorting and implementing priority queues.


